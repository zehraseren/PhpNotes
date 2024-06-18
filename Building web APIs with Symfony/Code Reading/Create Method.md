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
##### 1. İstek İçeriğini Alma
~~~~~~~
$data = $request->getContent();
~~~~~~~
> + `$request->getContent()`: HTTP isteğinin içeriğini alır (JSON formatında olması beklenir).

##### 2. Deserialization
~~~~~~~
$symphony = $this->serializer->deserialize($data, Symphony::class, 'json');
~~~~~~~
> + `$this->serializer->deserialize()`: JSON formatındaki veriyi Symphony class'ının bir nesnesine dönüştürür.

##### 3. Validation
~~~~~~~
$errors = $this->validator->validate($symphony);
if (count($errors) > 0) {
    $errorsString = (string) $errors;

    return new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST);
}
~~~~~~~
> + `$this->validator->validate()`: `Symphony` nesnesini doğrular.
> + `count($errors) > 0`: Hatalar olup olmadığını kontrol eder.
> + `new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST)`: Hatalar varsa, bunları döner ve HTTP 400 (Bad Request) durumu ile yanıt verir.

##### 4. Veri Kaydetme
~~~~~~~
$this->symphonyRepository->save($symphony);
~~~~~~~
> + `$this->symphonyRepository->save()`: Yeni `Symphony` nesnesini database'e kaydeder.

##### 5. Serialization ve Response
~~~~~~~
$response = $this->serializer->serialize($symphony, 'json');

return new JsonResponse($response, JsonResponse::HTTP_CREATED, [], true);
~~~~~~~
> + `$this->serializer->serialize()`: `Symphony` nesnesini JSON formatına dönüştürür.
> + `new JsonResponse($response, JsonResponse::HTTP_CREATED, [], true)`: Yeni oluşturulan `Symphony` nesnesini JSON formatında döner ve HTTP 201 (Created) durumu ile yanıt verir.
