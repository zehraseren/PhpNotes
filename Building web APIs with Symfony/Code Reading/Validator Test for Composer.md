+ Bu test, bir `ComposerController`'da geçersiz bir `Composer` oluşturma işleminin doğru şekilde ele alınıp alınmadığını kontrol etmek içindir.

#### Testin Amacı
+ Test, geçersiz bir `Composer` nesnesi oluşturulmaya çalışıldığında, API'ın beklenen hatayı döndürüp döndürmediğini kontrol eder. Bu kodda, geçersiz ülke kodu (`countryCode`) kullanılarak hata oluşturulmaya çalışılmaktadır.

#### Test Adımları
##### 1. Geçersiz Verinin Tanımlanması
+ `$invalidComposer` array'i, geçersiz bir `Composer` nesnesini temsil eder. Bu örnekte, `countryCode` olarak `XX` kullanılmıştır ki bu geçersiz bir ülke kodudur.

##### 2. İstek Gönderme
+ `$this->client->post('/composers', ['json' => $invalidComposer])` ifadesi, geçersiz veriyi JSON formatında `/composers` endpoint'ine `POST` isteği olarak gönderir.

##### 3. Yanıtın Kontrol Edilmesi
+ `$this->assertEquals(422, $response->getStatusCode())` ifadesi, API'ın `HTTP 422 (Unprocessable Entity)` durum kodunu döndürüp döndürmediğini kontrol eder.
+ `$this->assertStringContainsString('This value is not a valid country.', $response->getContent())` ifadesi, yanıt içeriğinde `This value is not a valid country.` mesajının bulunup bulunmadığını kontrol eder.

###### Detaylı Açıklama
~~~~~~~
public function testCreateInvalidComposer()
{
    // Defining an array with invalid composer data
    $invalidComposer = [
        'firstName' => 'Wolfgang', // Valid
        'dateOfBirth' => '1756-01-27', // Valid
        'countryCode' => 'XX' // Invalid country code
    ];

    // Sending POST request with invalid data
    $response = $this->client->post('/composers', [
        'json' => $invalidComposer,
    ]);

    // Verify that the response returns HTTP 422 (Unprocessable Entity) status code
    $this->assertEquals(422, $response->getStatusCode());

    // Check for the presence of a specific error message in the response content
    $this->assertStringContainsString('This value is not a valid country.', $response->getContent());
}
~~~~~~~

#### Neden Önemlidir?
+ `Geçersiz Girişleri Kontrol Etme:` Bu tür testler, API'ın geçersiz girişlere karşı sağlam olduğunu ve uygun hata mesajlarını döndürdüğünü doğrular.
+ `Kullanıcı Geri Bildirimi:` Kullanıcıların geçersiz veriler gönderdiğinde neyin yanlış olduğunu anlamalarına yardımcı olur.
+ `Veri Bütünlüğü:` Bu tür doğrulama kontrolleri, sistemdeki veri bütünlüğünü korumak için önemlidir.

> Bu test, geçersiz bir `Composer` nesnesi oluşturulmaya çalışıldığında, API'ın doğru şekilde `HTTP 422 hatası` döndürdüğünü ve uygun hata mesajını içerdiğini doğrular. Bu, API'ın veri doğrulama mekanizmalarının doğru çalıştığını gösterir.
