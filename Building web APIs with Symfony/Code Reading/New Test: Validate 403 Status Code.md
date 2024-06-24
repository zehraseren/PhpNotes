+ `testCreateComposerWithUserRole` method'u, `ROLE_USER` yetkisine sahip bir kullanıcının `Composer` oluşturma işlemini gerçekleştiremeyeceğini test eder.
+ Bu method, `uygulamadaki yetki kontrollerinin doğru çalışıp çalışmadığını doğrulamak için önemlidir.`
+ Aşağıda, bu method'un yer aldığı ve diğer gerekli method'ları da içeren tam bir `SomeControllerTest` class'ı örneği bulunmaktadır.
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

    protected function get(string $uri, ?string $token = null)
    {
        $headers = [];
        if ($token) {
            $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
        }

        return $this->client->request('GET', $uri, [], [], $headers);
    }

    protected function put(string $uri, array $data = [], ?string $token = null)
    {
        $headers = ['CONTENT_TYPE' => 'application/json'];
        if ($token) {
            $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
        }

        return $this->client->request('PUT', $uri, [], [], $headers, json_encode($data));
    }

    protected function delete(string $uri, ?string $token = null)
    {
        $headers = [];
        if ($token) {
            $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
        }

        return $this->client->request('DELETE', $uri, [], [], $headers);
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

    // Other test validations...
}
~~~~~~~

##### 1. setUp Method'u
+ `setUp` method'unda, `app:user-create` komutu kullanılarak test kullanıcıları oluşturulur ve her iki kullanıcı için token alınır.

##### 2. loginAndGetToken Method'u
+ Bu method, verilen kullanıcı adı ve şifre ile giriş yapar ve dönen token'ı alır.

##### 3. HTTP Yardımcı Method'ları
+ `post`, `get`, `put` ve `delete` method'ları, HTTP isteklerini göndermek için kullanılır ve gerekli yetki başlıklarını ayarlar.

##### 4. Test Method'ları
+ `testCreateComposer`: `ROLE_ADMIN` yetkisine sahip bir kullanıcının yeni bir `Composer` oluşturabildiğini doğrular.
+ `testCreateComposerWithUserRole`: `ROLE_USER` yetkisine sahip bir kullanıcının `Composer` oluşturamadığını (`403 Forbidden`) doğrular.

> Bu yapı, testlerinizin daha okunabilir ve yönetilebilir olmasını sağlar.
