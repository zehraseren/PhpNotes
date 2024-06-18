#### Route Annotation
~~~~~~~
/**
 * @Route("/symphonies", name="symphony_create", methods={"POST"})
 */
~~~~~~~
> + `@Route("/symphonies")`: Bu rota, `/symphonies` URL'i için geçerlidir.
> + `name="symphony_create"`: Bu rota için bir isimdir.
> + `methods={"POST"}`: Bu rota sadece POST isteklerini kabul eder.

#### Method Defination | Method Tanımlama 
##### 1. Request Verisini Alma
~~~~~~~
$data = $request->getContent();
~~~~~~~
> + `$request->getContent()`: HTTP isteğinin içeriğini alır (JSON formatında olması beklenir).

##### 2. Deserialization
~~~~~~~
$symphony = $this->serializer->deserialize($data, Symphony::class, 'json');
~~~~~~~
> + `$this->serializer->deserialize()`: JSON formatındaki veriyi Symphony class'ının bir nesnesine dönüştürür.
>   - `$this->serializer`: Bu serializer, nesneleri JSON (veya başka formatlara) dönüştürmek için kullanılır.

##### 3. Validation
~~~~~~~
$errors = $this->validator->validate($symphony);
if (count($errors) > 0) {
    $errorsString = (string) $errors;

    return new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST);
}
~~~~~~~
> + `$this->validator->validate()`: `Symphony` nesnesini doğrular.
>   - `$this->validator`: Bu validator, nesneleri doğrulama kurallarına göre kontrol eder.
> + `count($errors) > 0`: Hatalar olup olmadığını kontrol eder.
> + `new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST)`: Hatalar varsa, bunları döner ve HTTP 400 (Bad Request) durumu ile yanıt verir.

##### 4. Veritabanına Kaydetme
~~~~~~~
$this->symphonyRepository->save($symphony);
~~~~~~~
> + `$this->symphonyRepository->save()`: Yeni `Symphony` nesnesini database'e kaydeder.
>   - `$this->symphonyRepository`: Bu repository, `Symphony` nesneleriyle ilgili veritabanı işlemlerini gerçekleştirir.

##### 5. Yanıt İçin Serialization
~~~~~~~
$response = $this->serializer->serialize($symphony, 'json');
~~~~~~~
> + `$this->serializer->serialize()`: `Symphony` nesnesini JSON formatına dönüştürür.

##### 6. JsonResponse Döndürme
~~~~~~~
return new JsonResponse($response, JsonResponse::HTTP_CREATED, [], true);
~~~~~~~
> + `new JsonResponse($response, JsonResponse::HTTP_CREATED, [], true)`: JSON yanıtı döner.
>   - `$response`: JSON stringi.
>   - `JsonResponse::HTTP_CREATED`: HTTP 201 durumu (Created).
>   - `[]`: Ek başlıklar (headers).
>   - `true`: `$response`'un zaten JSON formatında olduğunu belirtir.
