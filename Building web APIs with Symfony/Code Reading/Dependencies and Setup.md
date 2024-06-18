+ `SymphonyControllerTest` class'ı, Symfony uygulamasındaki `SymphonyController` class'ının testlerini yapmak için kullanılır.
##### use İfadeleri
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;
~~~~~~~
> + `Symfony\Bundle\FrameworkBundle\Test\WebTestCase`: Bu class, `Symfony` uygulamasının test edilmesi için gerekli altyapıyı sağlar.
>   - `WebTestCase`, HTTP istekleri göndermek ve cevapları almak için Symfony'nin test istemcisini (`$this->client`) kullanılmasını sağlar.

##### Properties
~~~~~~~
private $client;
~~~~~~~
> + `$client`: WebTestCase class'ının korunan bir özelliği olarak tanımlanmıştır. Bu özellik, testler sırasında Symfony uygulamasına HTTP istekleri göndermek için kullanılacak olan test istemcisidir (`GuzzleHttpClient`).

##### `setUp` Method
~~~~~~~
 protected function setUp(): void
    {
        $this->client = static::createClient();
    }
~~~~~~~
> + `setUp()`: Her test çalıştırılmadan önce otomatik olarak çağrılır. Bu method, test class'ının başlangıç ayarlarını yapmak için kullanılır.
> + `static::createClient()` çağrısı ile Symfony test istemcisi oluşturulur ve `$this->client` özelliğine atanır. Bu sayede her test method'u içinde `$this->client` üzerinden Symfony uygulamasına HTTP istekleri gönderilebilir.
