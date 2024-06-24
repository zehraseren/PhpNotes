+ Bu ek test method'u, geçersiz bir erişim token'ı ile bir endpoint'e erişmeye çalışırken doğru şekilde `401 Unauthorized` yanıtını döndüğünü doğrular.
~~~~~~~
public function testInvalidToken()
{
    $client = static::createClient();

    // Sending request with invalid token
    $client->request('GET', '/some-endpoint', [], [], [
        'HTTP_AUTHORIZATION' => 'Bearer invalid_token',
    ]);

    $this->assertEquals(401, $client->getResponse()->getStatusCode());
    // Other test validations...
}
~~~~~~~

##### testInvalidToken Method
+ `public function testInvalidToken()`: Bu method, geçersiz bir erişim token'ı kullanılarak belirtilen endpoint'e bir istek yapar ve yanıt durum kodunun `401 Unauthorized` olup olmadığını kontrol eder.
+ `static::createClient();`: Yeni bir test istemcisi oluşturur.
+ `$client->request('GET', '/some-endpoint', [], [], ['HTTP_AUTHORIZATION' => 'Bearer invalid_token']);`: Bu satır, `GET` isteğini `/some-endpoint` URL'ine yapar ve istek başlığında geçersiz bir token (`invalid_token`) kullanır.
+ `$this->assertEquals(401, $client->getResponse()->getStatusCode());`: Yanıt durum kodunun `401` (`Unauthorized`) olduğunu doğrular.

#### Kullanım Senaryoları
+ Bu test method'u, API'ın güvenlik özelliklerini test etmek için kullanılır. Geçersiz token'larla yapılan isteklerin doğru şekilde `401 Unauthorized` durum kodu döndürdüğünden emin olunulmasını sağlar. Bu, uygulamanın güvenlik mekanizmalarının doğru şekilde çalıştığını ve sadece geçerli erişim token'larına sahip kullanıcıların korunan kaynaklara erişebildiğini doğrulamak için önemlidir.

#### Genişletme ve Ek Doğrulamalar
+ Testleri daha kapsamlı hale getirmek için ek doğrulamalar eklenebilir:
  - Yanıt gövdesinin belirli bir hata mesajı içerip içermediğini kontrol edilebilir.
  - Başka endpoint'ler ve farklı HTTP yöntemleri (`POST`, `PUT`, `DELETE` vb.) için benzer testler yazılabilir.
  - Geçersiz token'ların yanı sıra, süresi dolmuş token'lar için de testler eklenebilir.

###### Örnek Ek Doğrulama
+ Yanıt gövdesinin belirli bir hata mesajı içerip içermediğini kontrol eden ek bir doğrulama örneği:
~~~~~~~
public function testInvalidToken()
{
    $client = static::createClient();

    // Sending request with invalid token
    $client->request('GET', '/some-endpoint', [], [], [
        'HTTP_AUTHORIZATION' => 'Bearer invalid_token',
    ]);

    $response = $client->getResponse();

    $this->assertEquals(401, $response->getStatusCode());
    $this->assertStringContainsString('Invalid credentials', $response->getContent());
}
~~~~~~~

> Bu ek doğrulama, yanıtın içerik kısmında `Invalid credentials` mesajını içerip içermediğini kontrol eder. Bu şekilde, hata mesajlarının beklendiği gibi olup olmadığı da test edilmiş olunur.
