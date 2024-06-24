+ `SomeControllerTest` class, Symfony framework kullanılarak bir controller testinin nasıl yapılacağını gösterir.
+ Test class'ı, belirli bir API endpoint'ini test etmek için gerekli adımları içerir ve güvenli bir endpoint'e erişmek için bir erişim token'ı kullanır.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SomeControllerTest extends WebTestCase
{
    private $accessToken;

    public function setUp(): void
    {
        parent::setUp();

        //Steps to create test users and access tokens
        $client = static::createClient();
        $userRepository = $client->getContainer()->get(UserRepository::class);
        $user = $userRepository->findOneByEmail('test@example.com');
        
        // Mock the AccessTokenHandler or use the real one to generate a token
        $accessTokenHandler = $client->getContainer()->get(AccessTokenHandler::class);
        $this->accessToken = $accessTokenHandler->createForUser($user);
    }

    public function testSomeEndpoint()
    {
        $client = static::createClient();

        // Send request with authorization header
        $client->request('GET', '/some-endpoint', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . $this->accessToken,
        ]);

        $this->assertEquals(200, $client->getResponse()->getStatusCode());
        // Other test validations...
    }
}
~~~~~~~

##### 1. Namespace ve Imports
+ `use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;`: Symfony'nin web testi için kullanılan temel class'ını dahil eder.

##### 2. SomeControllerTest Class
+ Bu class, `WebTestCase` class'ından türetilmiştir ve Symfony uygulamalarında web testi yapmak için kullanılır.

##### 3. Özellikler
+ $accessToken: Testlerde kullanılacak erişim token'ını tutar.

##### 4. setUp Method
+ `public function setUp(): void`: Testlerin başlamasından önce her seferinde çalıştırılan yapılandırma method'udur.
+ `parent::setUp();`: Üst class'ın `setUp` method'unu çağırarak temel yapılandırma işlemlerini yapar.
+ `static::createClient();`: Bir test istemcisi oluşturur.
+ `$userRepository = $client->getContainer()->get(UserRepository::class);`: Kullanıcı deposunu alır.
+ `$user = $userRepository->findOneByEmail('test@example.com');`: Test için kullanılacak kullanıcıyı e-posta adresine göre bulur.
+ `$accessTokenHandler = $client->getContainer()->get(AccessTokenHandler::class);`: Erişim token'ı yöneticisini alır.
+ `$this->accessToken = $accessTokenHandler->createForUser($user);`: Test kullanıcısı için bir erişim token'ı oluşturur ve `$accessToken` değişkenine atar.

##### 5. testSomeEndpoint Method
+ `public function testSomeEndpoint()`: Belirli bir endpoint'i test eden method.
+ `static::createClient();`: Yeni bir test istemcisi oluşturur.
+ `$client->request('GET', '/some-endpoint', [], [], ['HTTP_AUTHORIZATION' => 'Bearer ' . $this->accessToken]);`: Bu satır, `GET` isteğini `/some-endpoint` URL'ine yapar ve istek başlığında oluşturulan erişim token'ını `Authorization` başlığına ekler.
+ `$this->assertEquals(200, $client->getResponse()->getStatusCode());`: Yanıt durum kodunun `200` (`OK`) olduğunu doğrular.

> Bu test class'ı, Symfony uygulamasında belirli bir endpoint'in güvenliğini test etmek için kullanılır. `setUp` method'unda, test için gerekli kullanıcıyı ve erişim token'ını oluşturur. `testSomeEndpoint` method'unda, belirtilen endpoint'e bir `GET` isteği gönderir ve yanıtın başarılı olup olmadığını doğrular. Bu yapı, API endpoint'lerinin güvenliğini ve işlevselliğini test etmek için kullanışlıdır.
