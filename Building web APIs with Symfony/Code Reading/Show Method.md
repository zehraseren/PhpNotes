#### Route Annotation
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_show", methods={"GET"})
 */
~~~~~~~
+ `@Route("/symphonies/{id}")`: Bu rota, `/symphonies/{id}` URL'i için geçerlidir.
+ `name="symphony_show"`: Bu rota için bir isimdir.
+ `methods={"GET"}`: Bu rota sadece GET isteklerini kabul eder.

#### Method Defination | Method Tanımlama
##### 1. ID'ye Göre Symphony Nesnesini Veritabanından Alma
~~~~~~~
$symphony = $this->symphonyRepository->find($id);
~~~~~~~
+ `$this->symphonyRepository->find($id)`: Symphony repository'sini kullanarak belirli ID'ye sahip `Symphony` nesnesini database'den alır. Eğer belirtilen ID'ye sahip bir `Symphony` bulunamazsa, `null` döner.
   - `$this->symphonyRepository`: Bu repository, `Symphony` nesneleriyle ilgili veritabanı işlemlerini gerçekleştirir.

##### 2. Nesne Bulunamazsa Hata Yanıtı Döndürme
~~~~~~~
if (!$symphony) {
    return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
}
~~~~~~~
+ `if (!$symphony)`: Eğer `null` dönerse, yani belirtilen ID'ye sahip bir `Symphony` bulunamazsa.
+ `new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND)`: `HTTP 404` (`Not Found`) durumu ile hata mesajı döner.

##### 3. Serialization
~~~~~~~
$data = $this->serializer->serialize($symphony, 'json');
~~~~~~~
+ `$this->serializer->serialize()`: `Symphony` nesnesini JSON formatına dönüştürür.
   - `$this->serializer`: Bu serializer, nesneleri JSON (veya başka formatlara) dönüştürmek için kullanılır.

##### 4. JsonResponse Döndürme
~~~~~~~
return new JsonResponse($data, JsonResponse::HTTP_OK, [], true);
~~~~~~~
+ `new JsonResponse($data, JsonResponse::HTTP_OK, [], true)`: JSON yanıtı döndürür.
   - `$response`: JSON string'i.
   - `JsonResponse::HTTP_OK`: HTTP 200 durumu (`Succeeded`).
   - `[]`: Ek başlıklar (`headers`).
   - `true`: `$response`'un zaten JSON formatında olduğunu belirtir.
