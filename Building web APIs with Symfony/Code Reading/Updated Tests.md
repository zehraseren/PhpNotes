+ Bu iki test method'u, `Composer` oluşturma işlemi için gerekli yetki kontrollerini doğrulamaktadır.
+ Aşağıda bu test methodlarının yer aldığı class'ı ve gerekli diğer method'ları içeren örnek kod bulunmaktadır.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Console\Application;
use Symfony\Component\Console\Input\ArrayInput;
use Symfony\Component\Console\Output\BufferedOutput;
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SomeControllerTest extends WebTestCase
{
    private static $testUserToken;
    private static $testAdminToken;
    private $client;

    public function setUp(): void
    {
        parent::setUp();

        $kernel = self::bootKernel();
        $application = new Application($kernel);
        $application->setAutoExit(false);

        // Run user creation commands
        $command = $application->find('app:user-create');
        $output = new BufferedOutput();

        // Create test users
        $command->run(new ArrayInput([
            'username' => 'testuser',
            'password' => 'Test1234',
            'roles' => ['ROLE_USER']
        ]), $output);

        $command->run(new ArrayInput([
            'username' => 'testadmin',
            'password' => 'Test1234',
            'roles' => ['ROLE_ADMIN']
        ]), $output);

        $this->client = static::createClient();

        // Getting tokens for test users
        self::$testUserToken = $this->loginAndGetToken('testuser', 'Test1234');
        self::$testAdminToken = $this->loginAndGetToken('testadmin', 'Test1234');
    }

    private function loginAndGetToken($username, $password)
    {
        $this->client->request('POST', '/login', [], [], [
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'username' => $username,
            'password' => $password
        ]));

        $response = $this->client->getResponse();
        $data = json_decode($response->getContent(), true);

        return $data['token'];
    }

    protected function post(string $uri, array $data = [], ?string $token = null)
    {
        $headers = ['CONTENT_TYPE' => 'application/json'];
        if ($token) {
            $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
        }

        return $this->client->request('POST', $uri, [], [], $headers, json_encode($data));
    }

    public function testCreateComposer()
    {
        $response = $this->post('/composer', [
            'name' => 'Test Composer'
        ], self::$testAdminToken);

        $this->assertEquals(201, $response->getStatusCode());
    }

    public function testCreateComposerWithUserRole()
    {
        $response = $this->post('/composer', [
            'name' => 'Test Composer'
        ], self::$testUserToken);

        $this->assertEquals(403, $response->getStatusCode());
    }
}
~~~~~~~

##### 1. setUp Methodu
+ Test kullanıcılarını oluşturur ve her iki kullanıcı için token alır.

##### 2. loginAndGetToken Methodu
+ Verilen kullanıcı adı ve şifre ile giriş yapar ve token'ı döner.

##### 3. post Methodu
+ `POST` isteği gönderir ve gerekli yetki başlıklarını ayarlar.

##### 4. testCreateComposer ve testCreateComposerWithUserRole Method'ları
+ `testCreateComposer`: Admin kullanıcısının yeni bir `Composer` oluşturabildiğini doğrular.
+ `testCreateComposerWithUserRole`: Normal bir kullanıcının `Composer` oluşturamadığını (`403 Forbidden`) doğrular.

> Bu testler, Composer oluşturma işlemi için gerekli yetki kontrollerini doğrulamaktadır ve uygulamanın güvenli ve beklenildiği gibi çalıştığını test etmek için kullanılabilir.
