#### Route Annotation
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_update", methods={"PUT"})
 */
~~~~~~~
> + `@Route("/symphonies/{id}")`: Bu rota, `/symphonies/{id}` URL'i için geçerlidir.
> + `name="symphony_update"`: Bu rota için bir isimdir.
> + `methods={"PUT"}`: Bu rota sadece PUT isteklerini kabul eder.

#### Method Defination | Method Tanımlama
##### 1. Request Verisini Alma
~~~~~~~
$data = $request->getContent();
~~~~~~~
> + `$request->getContent()`: İstek gövdesindeki ham veriyi alır. Bu genellikle JSON formatında olur.

##### 2. Mevcut Symphony Nesnesini Bulma
~~~~~~~
$symphony = $this->symphonyRepository->find($id);

if (!$symphony) {
    return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
}
~~~~~~~
> + `$this->symphonyRepository->find($id)`: Veritabanında belirtilen ID'ye sahip `Symphony` nesnesini arar. Eğer bulunamazsa, HTTP 404 hata mesajı ile yanıt döner.

##### 3. Deserialization ve Nesneyi Güncelleme
~~~~~~~
$this->serializer->deserialize($data, Symphony::class, 'json', [AbstractNormalizer::OBJECT_TO_POPULATE => $symphony]);
~~~~~~~
> + `$this->serializer->deserialize()`: JSON formatındaki veriyi `Symphony` nesnesine dönüştürür. `OBJECT_TO_POPULATE` seçeneği ile mevcut `Symphony` nesnesini günceller.

##### 4. Validation
~~~~~~~
$errors = $this->validator->validate($symphony);
if (count($errors) > 0) {
    $errorsString = (string) $errors;

    return new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST);
}
~~~~~~~
> + `$this->validator->validate()`: Güncellenen Symphony nesnesini doğrulama kurallarına göre kontrol eder. Hatalar varsa, HTTP 400 hata mesajı ile yanıt döner.
>   - `$this->validator`: Bu validator, nesneleri doğrulama kurallarına göre kontrol eder.

##### 5. Veritabanına Kaydetme
~~~~~~~
$this->symphonyRepository->save($symphony);
~~~~~~~
> + `$this->symphonyRepository->save()`: Güncellenen `Symphony` nesnesini database'e kaydeder.
>   - `$this->symphonyRepository`: Bu repository, `Symphony` nesneleriyle ilgili veritabanı işlemlerini gerçekleştirir.

##### 6. Yanıt İçin Serialization
~~~~~~~
$response = $this->serializer->serialize($symphony, 'json');
~~~~~~~
> + `$this->serializer->serialize()`: Kaydedilen `Symphony` nesnesini JSON formatına dönüştürür.
>   - `$this->serializer`: Bu serializer, nesneleri JSON (veya başka formatlara) dönüştürmek için kullanılır.

##### 7. JsonResponse Döndürme
~~~~~~~
return new JsonResponse($response, JsonResponse::HTTP_OK, [], true);
~~~~~~~
> + `new JsonResponse($response, JsonResponse::HTTP_OK, [], true)`: JSON yanıtı döner.
>   - `$response`: JSON stringi.
>   - `JsonResponse::HTTP_OK`: HTTP 200 durumu (Succeeded).
>   - `[]`: Ek başlıklar (headers).
>   - `true`: `$response`'un zaten JSON formatında olduğunu belirtir.
