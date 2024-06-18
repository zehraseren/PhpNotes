+ Bu `testShow()` methodu, Symfony uygulamasında bulunan `SymphonyController` class'ının `show` method'unu test etmek için kullanılan bir PHPUnit testidir.
~~~~~~~
public function testShow(): void
{
    // Send GET request to symphony app
    $this->client->request('GET', '/symphonies/1');
    
    // Check HTTP status code
    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~

###### 1. GET İsteği Gönderme
+ `$this->client->request('GET', '/symphonies/1')`; satırıyla Symfony uygulamasına `GET` method'unu kullanarak `/symphonies/1` endpoint'ine bir HTTP isteği gönderilir.
+ `/symphonies/1` endpoint'i, `show` method'unda belirtilen `id` parametresiyle bir `Symphony` kaydını görüntülemek için kullanılır. Burada `1` yerine mevcut bir `Symphony id`'si geçirilmiştir.

###### 2. HTTP Durum Kodunu Kontrol Etme
+ `$this->assertEquals(200, $this->client->getResponse()->getStatusCode());` satırıyla, Symfony'nin `getResponse()` method'uyla alınan HTTP yanıtının durum kodunun `200` (`OK`) olup olmadığını kontrol edilir.
+ `200` durum kodu, belirtilen `id`'ye sahip `Symphony` kaydının başarıyla bulunduğunu ve istemcinin doğru bir şekilde yanıt aldığını gösterir.

##### Testin İşlevi
+ Bu test method'unun amacı, `SymphonyController` class'ındaki `show` method'unu doğru şekilde test etmektir. Yani:
  - `/symphonies/1` endpoint'ine `GET` isteği yaparak belirtilen `id`'ye sahip bir `Symphony` kaydı bulunabilir mi?
  - Görüntüleme işlemi başarılı olduysa, HTTP durum kodu `200` (`OK`) dönüyor mu?
> Bu test method'unu çalıştırılarak, Symfony uygulamasının `SymphonyController` class'ındaki `show` method'unun doğru çalışıp çalışmadığını ve olası hataların tespit edilmesini sağlar. Testler, uygulamanın farklı bölümlerinin doğru işlediğini doğrulamak için önemli bir araçtır ve sürekli entegrasyon (`CI`) süreçlerinde de sıkça kullanılır.
