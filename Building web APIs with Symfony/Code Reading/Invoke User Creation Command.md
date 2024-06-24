+ `SomeControllerTest` class'ı, bir test kullanıcı oluşturmak ve bu kullanıcı için bir erişim token'ı almak amacıyla düzenlenmiştir. `setUp` method'unda, Symfony'nin konsol komutları kullanılarak test kullanıcı oluşturulmakta ve ardından bu kullanıcıyla giriş yapılarak erişim token'ı alınmaktadır.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Console\Application;
use Symfony\Component\Console\Input\ArrayInput;
use Symfony\Component\Console\Output\BufferedOutput;
use Symfony\Component\Console\Tester\CommandTester;
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SomeControllerTest extends WebTestCase
{
    private static $testUserToken;

    public function setUp(): void
    {
        parent::setUp();

        $kernel = self::bootKernel();
        $application = new Application($kernel);
        $application->setAutoExit(false);

        $command = $application->find('app:user-create');

        $input = new ArrayInput([
            'username' => 'testuser',
            'password' => 'Test1234',
            'roles' => ['ROLE_USER']
        ]);

        $output = new BufferedOutput();
        $command->run($input, $output);

        // Login to get the access token
        $client = static::createClient();
        $client->request('POST', '/login', [], [], [
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'username' => 'testuser',
            'password' => 'Test1234'
        ]));

        $response = $client->getResponse();
        $data = json_decode($response->getContent(), true);
        self::$testUserToken = $data['token'];
    }
}
~~~~~~~

##### 1. setUp Method
+ Symfony kernel'ini başlatır ve `Application` nesnesi oluşturur.
+ `app:user-create` komutunu çalıştırarak bir test kullanıcısı oluşturur.
+ Bu kullanıcı ile giriş yaparak bir JWT token'ı alır ve bu token'ı statik bir değişkene (`self::$testUserToken`) kaydeder.

##### 2.Test Metotları:
+ `testSomeEndpoint`: Token ile korunan bir endpoint'e istek gönderir ve başarılı yanıt alınıp alınmadığını kontrol eder.
+ `testInvalidToken`: Geçersiz bir token ile istek gönderir ve `401 Unauthorized` yanıtını doğrular.
+ `testSomeOtherEndpoint`: Token ile korunan bir endpoint'e `POST` isteği gönderir ve başarılı (`201 Created`) yanıt alınıp alınmadığını kontrol eder.

> Bu test class'ı, Symfony'de token tabanlı yetkilendirme kullanan API'ların test edilmesi için iyi bir örnektir. Token oluşturma, doğrulama ve korunan endpoint'lerin erişilebilirliğini test eder.
