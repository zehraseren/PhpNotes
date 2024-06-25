+ Bu `testUpdate()` method'u, Symfony uygulamasında bulunan `SymphonyController` class'ının `update` method'unu test etmek için kullanılan bir PHPUnit testidir.
~~~~~~~
public function testUpdate(): void
{
    // Data to update
    $data = ['name' => 'Updated Symphony'];

    // Send PUT request to symphony app
    $this->client->request(
        'PUT',                                   // HTTP method
        '/symphonies/1',                         // Endpoint
        [],                                      // URL parameters
        [],                                      // Request Content (body)
        ['CONTENT_TYPE' => 'application/json'],  // Headers
        json_encode($data)                       // Content (json format)
    );

    // Check HTTP status code
    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~

###### 1. Güncellenecek Veri (`$data`)
+ `$data` array'i, güncellenecek `Symphony` kaydının yeni verilerini içerir. Bu örnekte sadece `name` alanı güncellenecek şekilde ayarlanmıştır.

###### 2. PUT İsteği Gönderme
+ `$this->client->request()` method'uyla `Symfony` uygulamasına `PUT` isteği gönderilir.
+ `'PUT'`: HTTP method'u olarak `PUT` seçilmiştir.
+ `'/symphonies/1'`: Endpoint olarak `/symphonies/1` adresi belirlenmiştir. `1`, güncellenmek istenen `Symphony kaydının id`'si olarak kullanılmıştır.
+ `['CONTENT_TYPE' => 'application/json']`: İsteğin başlığında içeriğin JSON formatında olduğu belirtilmiştir.
+ `json_encode($data)`: `$data` array'i JSON formatına çevrilerek isteğin içeriği olarak gönderilmiştir.

###### 3. HTTP Durum Kodunu Kontrol Etme
+ `$this->assertEquals(200, $this->client->getResponse()->getStatusCode())` satırıyla, Symfony'nin `getResponse()` method'uyla alınan HTTP yanıtının durum kodunun `200` (`OK`) olup olmadığını kontrol edilir.
+ `200` durum kodu, güncelleme işleminin başarılı bir şekilde tamamlandığını ve istemcinin doğru bir şekilde yanıt aldığını gösterir.

##### Testin İşlevi
+ Bu test method'unun amacı, `SymphonyController` class'ındaki `update` method'unu doğru şekilde test etmektir. Yani:
  - `/symphonies/1` endpoint'ine `PUT` isteği yaparak belirtilen `id`'ye sahip bir Symphony kaydı güncellenebilir mi?
  - Güncelleme işlemi başarılı olduysa, HTTP durum kodu `200` (`OK`) dönüyor mu?
> Bu test method'u çalıştırılarak, Symfony uygulamasının `SymphonyController` class'ındaki `update` method'unun doğru çalışıp çalışılmadığını ve olası hataların tespit edilmesini sağlar. Testler, uygulamanın farklı bölümlerinin doğru işlediğini doğrulamak için önemli bir araçtır ve sürekli entegrasyon (`CI`) süreçlerinde de sıkça kullanılır.
