+ Bu test method'u, Symfony uygulamasında bulunan `SymphonyController` class'ının index method'unu test etmek için kullanılan bir PHPUnit testidir.
~~~~~~~
public function testIndex(): void
{
    $this->client->request('GET', '/symphonies');
    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
###### 1. `$this->client->request('GET', '/symphonies');`
+ `client` özelliği, Symfony'nin `WebTestCase` class'ından miras alınan bir özelliktir.
+ `request('GET', '/symphonies')` method'u, Symfony uygulamasına `GET` method'unu kullanarak `/symphonies` endpoint'ine bir HTTP isteği gönderir.
###### 2. `$this->assertEquals(200, $this->client->getResponse()->getStatusCode());`
+ `assertEquals()` method'u, PHPUnit tarafından sağlanan bir method'dur. Bu method, iki değeri karşılaştırır ve eğer eşit değillerse bir hata verir.
+ `$this->client->getResponse()->getStatusCode()` ile alınan HTTP yanıtının durum kodu (status code) `200` olmalıdır. Bu, HTTP isteğinin başarılı bir şekilde işlendiğini ve istenen kaynak veya endpoint'in mevcut olduğunu gösterir.

##### Testin İşlevi
+ Bu test method'unun amacı, `/symphonies` endpoint'inin `GET` isteğine nasıl yanıt verdiğini doğrulamaktır. Başka bir deyişle, Symfony uygulamasının `SymphonyController` class'ındaki `index method`'u ile ilgili olarak şu kontrolleri sağlar:
  - `/symphonies` endpoint'i var mı?
  - `GET` isteği yapıldığında HTTP durum kodu `200` (`OK`) mü dönüyor?
> Bu test method'u çalıştırılarak, `/symphonies` endpoint'inin istenen şekilde çalışıp çalışmadığı doğrulanabilir ve olası hataları tespit etme olanağı sağlar.
