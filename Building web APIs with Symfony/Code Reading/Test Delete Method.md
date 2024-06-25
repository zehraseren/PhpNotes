+ Bu `testDelete()` method'u, Symfony uygulamasında bulunan `SymphonyController` class'ının delete method'unu test etmek için kullanılan bir PHPUnit testidir. 
~~~~~~~
public function testDelete(): void
{
    // Send DELETE request to symphony app
    $this->client->request('DELETE', '/symphonies/1');
    
    // Check HTTP status code
    $this->assertEquals(204, $this->client->getResponse()->getStatusCode());
}
~~~~~~~

###### 1. DELETE İsteği Gönderme
+ `$this->client->request('DELETE', '/symphonies/1')` satırıyla `Symfony` uygulamasına `DELETE` method'unu kullanarak `/symphonies/1` endpoint'ine bir HTTP isteği gönderilir.
+ `/symphonies/1` endpoint'i, `delete` method'unda belirtilen `id` parametresiyle bir `Symphony` kaydının silinmesi için kullanılır. Burada `1` yerine mevcut bir `Symphony id`'si geçirilmiştir.

###### 2. HTTP Durum Kodunu Kontrol Etme
+ `$this->assertEquals(204, $this->client->getResponse()->getStatusCode())` satırıyla, Symfony'nin `getResponse()` method'uyla alınan HTTP yanıtının durum kodunun `204` (`No Content`) olup olmadığını kontrol edilir.
+ `204` durum kodu, belirtilen `id`'ye sahip `Symphony` kaydının başarıyla silindiğini ve istemcinin doğru bir şekilde yanıt aldığını gösterir.

##### Testin İşlevi
+ Bu test method'unun amacı, `SymphonyController` class'ındaki `delete` method'unu doğru şekilde test etmektir. Yani:
  - `/symphonies/1` endpoint'ine `DELETE` isteği yaparak belirtilen `id`'ye sahip bir `Symphony` kaydı silinebilir mi?
  - Silme işlemi başarılı olduysa, HTTP durum kodu `204` (`No Content`) dönüyor mu?
> Bu test method'u çalıştırılarak, Symfony uygulamasının `SymphonyController` class'ındaki `delete` method'unun doğru çalışıp çalışılmadığını ve olası hataların tespit edilmesini sağlar. Testler, uygulamanın farklı bölümlerinin doğru işlediğini doğrulamak için önemli bir araçtır ve sürekli entegrasyon (`CI`) süreçlerinde de sıkça kullanılır.
