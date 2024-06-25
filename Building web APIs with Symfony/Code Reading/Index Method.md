+ Bu kod parçası, Symfony framework kullanılarak geliştirilmiş bir controller method'udur.
+ Bu method, tüm `Symphony` nesnelerini listeleyen bir endpoint sağlar. 

#### Route Annotation
~~~~~~~
/**
 * @Route("/symphonies", name="symphony_index", methods={"GET"})
 */
~~~~~~~
+ `@Route("/symphonies")`: Bu rota `/symphonies` URL'i için geçerlidir.
+ `name="symphony_index"`: Bu rota için bir isimdir. Symfony içinde bu rotayı bu isimle referans alır.
+ `methods={"GET"}`: Bu rota sadece `GET` isteklerini kabul eder.

#### Method Defination | Method Tanımlama
##### 1. Symphony Nesnelerini Database'den Alma
~~~~~~~
$symphonies = $this->symphonyRepository->findAll();
~~~~~~~
+ `$this->symphonyRepository->findAll()`: Symphony repository'sini kullanarak tüm `Symphony` nesnelerini database'den alır. Bu method, tüm Symphony nesnelerini döndürür.
   - `$this->symphonyRepository`: Bu repository, `Symphony` nesneleriyle ilgili veritabanı işlemlerini gerçekleştirir.

##### 2. Serialization
~~~~~~~
$data = $this->serializer->serialize($symphonies, 'json');
~~~~~~~
+ `$this->serializer->serialize()`: Symphony nesnelerini JSON formatına dönüştürür. Bu işlem, `Symphony` nesnelerinin bir array'ini JSON string'ine dönüştürür.
   - `$this->serializer`: Bu serializer, nesneleri JSON (veya başka formatlara) dönüştürmek için kullanılır.

##### 3. JsonResponse Döndürme
~~~~~~~
return new JsonResponse($data, JsonResponse::HTTP_OK, [], true);
~~~~~~~
+ `new JsonResponse($data, JsonResponse::HTTP_OK, [], true)`: JSON yanıtı döndürür.
   - `$data`: JSON string'i.
   - `JsonResponse::HTTP_OK`: HTTP 200 durumu (`Succeeded`).
   - `[]`: Ek başlıklar (`headers`).
   - `true`: `$data`'nın zaten JSON formatında olduğunu belirtir. Bu nedenle Symfony, tekrar JSON dönüşümü yapmaz.
