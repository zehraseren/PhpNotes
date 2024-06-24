+ Symfony'de test class'ının yapılandırması, hem `ROLE_USER` hem de `ROLE_ADMIN` kullanıcıları için token oluşturma işlemlerini içerir.
+ Aşağıda, bu token'ları kullanarak `SymphonyController` üzerinde testler gerçekleştiren örnek bir `SomeControllerTest` class'ı bulunmaktadır.
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

    public function testIndexSymphony()
    {
        $this->client->request('GET', '/symphonies', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . self::$testUserToken,
        ]);

        $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
    }

    public function testCreateSymphony()
    {
        $this->client->request('POST', '/symphonies', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . self::$testAdminToken,
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'name' => 'Test Symphony',
            'composer' => 'Test Composer',
            'year' => 2024
        ]));

        $this->assertEquals(201, $this->client->getResponse()->getStatusCode());
    }

    public function testCreateSymphonyAsUser()
    {
        $this->client->request('POST', '/symphonies', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . self::$testUserToken,
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'name' => 'Test Symphony',
            'composer' => 'Test Composer',
            'year' => 2024
        ]));

        $this->assertEquals(403, $this->client->getResponse()->getStatusCode());
    }

    public function testUpdateSymphony()
    {
        $this->client->request('PUT', '/symphonies/1', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . self::$testAdminToken,
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'name' => 'Updated Symphony',
            'composer' => 'Updated Composer',
            'year' => 2025
        ]));

        $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
    }

    public function testDeleteSymphony()
    {
        $this->client->request('DELETE', '/symphonies/1', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . self::$testAdminToken,
        ]);

        $this->assertEquals(204, $this->client->getResponse()->getStatusCode());
    }
}
~~~~~~~

##### 1. setUp Method'u
+ `setUp` method'unda, Symfony uygulamasını başlatır ve test kullanıcılarını oluşturur.
+ `app:user-create` komutu ile kullanıcıları oluşturur.
+ `loginAndGetToken` method'u ile kullanıcılar için token alır.

##### loginAndGetToken Method'u
+ Verilen kullanıcı adı ve şifre ile giriş yapar ve token'ı döner.

##### Test Method'ları
+ `testIndexSymphony`: Kullanıcının symphony listesini görebildiğini doğrular.
+ `testCreateSymphony`: Admin kullanıcının yeni bir symphony oluşturabildiğini doğrular.
+ `testCreateSymphonyAsUser`: Normal bir kullanıcının symphony oluşturamadığını (403 Forbidden) doğrular.
+ `testUpdateSymphony`: Admin kullanıcının var olan bir symphony'yi güncelleyebildiğini doğrular.
+ `testDeleteSymphony`: Admin kullanıcının var olan bir symphony'yi silebildiğini doğrular.

> Bu class, `SymphonyController` üzerindeki farklı işlevleri ve erişim kontrollerini test eder. Bu testler, uygulamanızın güvenli ve beklenildiği gibi çalıştığını doğrulamak için önemlidir.
