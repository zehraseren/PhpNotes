### Composer Endpoints
#### 1. ComposerController Oluşturma 
+ Symfony'nin MakerBundle'ı kullanılarak bir controller oluşturulur.
~~~~~~~
php bin/console make:controller ComposerController
~~~~~~~

#### 2. `ComposerController.php` Dosyasındaki Route ve Method'ları Tanımlama
+ Oluşturulan `ComposerController.php` dosyasını gerekli CRUD işlemlerini içerecek şekilde düzenlenmelidir.
###### ComposerController.php
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\Composer;
use App\Repository\ComposerRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;
use Symfony\Component\Serializer\Normalizer\AbstractNormalizer;

class ComposerController extends AbstractController
{
    #[Route('/composer', name: 'composer_index', methods: ['GET'])]
    public function index(ComposerRepository $repo): JsonResponse
    {
        $composers = $repo->findAll();
        return $this->json($composers);
    }

    #[Route('/composer/{id}', name: 'composer_show', methods: ['GET'])]
    public function show(Composer $composer): JsonResponse
    {
        return $this->json($composer);
    }

    #[Route('/composer', name: 'composer_create', methods: ['POST'])]
    public function create(Request $request, ComposerRepository $repo, SerializerInterface $serializer): JsonResponse
    {
        $composer = $serializer->deserialize($request->getContent(), Composer::class, 'json');
        $repo->save($composer, true);
        return $this->json($composer, JsonResponse::HTTP_CREATED);
    }

    #[Route('/composer/{id}', name: 'composer_update', methods: ['PUT'])]
    public function update(Request $request, ComposerRepository $repo, SerializerInterface $serializer, Composer $composer): JsonResponse
    {
        $serializer->deserialize(
            $request->getContent(),
            Composer::class,
            'json',
            [AbstractNormalizer::OBJECT_TO_POPULATE => $composer]
        );
        $repo->save($composer, true);
        return $this->json($composer);
    }

    #[Route('/composer/{id}', name: 'composer_delete', methods: ['DELETE'])]
    public function delete(ComposerRepository $repo, Composer $composer): JsonResponse
    {
        $repo->remove($composer, true);
        return new JsonResponse(null, JsonResponse::HTTP_NO_CONTENT);
    }
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Composer%20Endpoints.md)

#### 3. Serializer Bileşini Kurma
+ Symfony'nin serializer bileşeninin yüklü olduğundan emin olunmalıdır.
~~~~~~~
composer require symfony/serializer-pack
~~~~~~~

#### 4. CURL ile Endpoint'leri Test Etme
+ Endpoint'ler `curl` kullanılarak kontrol edilir.

##### Composer Oluşturma:
~~~~~~~
curl -X POST http://localhost:8000/composer \
     -H "Content-Type: application/json" \
     -d '{"firstName": "Wolfgang", "lastName": "Mozart", "dateOfBirth": "1756-01-27", "countryCode": "AT"}'
~~~~~~~

##### Tüm Composer'ları Listeleme:
~~~~~~~
curl -X GET http://localhost:8000/composer \
     -H "Accept: application/json"
~~~~~~~

##### Belirli Bir Composer'ı Gösterme (ID 1 varsayılsın):
~~~~~~~
curl -X GET http://localhost:8000/composer/1 \
     -H "Accept: application/json"
~~~~~~~

##### Composer Güncelleme (ID 1 varsayılsın):
~~~~~~~
curl -X PUT http://localhost:8000/composer/1 \
     -H "Content-Type: application/json" \
     -d '{"firstName": "Wolfgang Amadeus", "lastName": "Mozart", "dateOfBirth": "1756-01-27", "countryCode": "AT"}'
~~~~~~~

##### Composer Silme (ID 1 varsayılsın):
~~~~~~~
curl -X DELETE http://localhost:8000/composer/1
~~~~~~~

#### 5. Otomatik Testler
+ `curl` ile manuel test yapmanın yanı sıra, endpoint'lerin çalıştığından emin olmak için otomatik testler oluşturmak daha verimlidir.
+ Symfony'nin test araçları, API'ın beklendiği gibi çalışmasını sağlamak için yardımcı olabilir.
###### Index endpoint'i için basit bir test örneği:
###### ComposerControllerTest.php
~~~~~~~
namespace App\Tests\Controller;

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class ComposerControllerTest extends WebTestCase
{
    public function testIndex()
    {
        $client = static::createClient();
        $client->request('GET', '/composer');

        $this->assertEquals(200, $client->getResponse()->getStatusCode());
        $this->assertJson($client->getResponse()->getContent());
    }
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Index%20Endpoint%20Test%20%C3%96rne%C4%9Fi.md)

###### Bu testi çalıştırmak için: 
~~~~~~~
php bin/phpunit
~~~~~~~

***
### Symphony endpoints and tests 
#### Symphony Controller ve Test Oluşturma
##### 1. Symphony Controller Oluşturma
+ Symfony uygulaması için yeni bir controller oluşturmak için aşağıdaki komut kullanılır.
~~~~~~~
php bin/console make:controller SymphonyController
~~~~~~~

##### 2. Symphony Test Oluşturma
+ Symphony controller için bir test class oluşturulur.
~~~~~~~
php bin/console make:test SymphonyControllerTest
~~~~~~~

#### Symphony Controller'ın Uygulanması
##### 1. Dependencies and Constructor
+ Gerekli bağımlılıklar enjekte edilir (SymphonyRepository, SerializerInterface ve ValidatorInterface).
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;
use Symfony\Component\Validator\Validator\ValidatorInterface;

class SymphonyController extends AbstractController
{
    private $symphonyRepository;
    private $serializer;
    private $validator;

    public function __construct(SymphonyRepository $symphonyRepository, SerializerInterface $serializer, ValidatorInterface $validator)
    {
        $this->symphonyRepository = $symphonyRepository;
        $this->serializer = $serializer;
        $this->validator = $validator;
    }
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Dependencies%20and%20Constructor.md)

##### 2. Index Method
+ Bütün symphony'ler alınır ve döndürülür.
~~~~~~~
/**
 * @Route("/symphonies", name="symphony_index", methods={"GET"})
 */
public function index(): JsonResponse
{
    $symphonies = $this->symphonyRepository->findAll();
    $data = $this->serializer->serialize($symphonies, 'json');

    return new JsonResponse($data, JsonResponse::HTTP_OK, [], true);
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Index%20Method.md)

##### 3. Create Method
+ Request yeniden serileştirilir, symphony doğrulanır ve database'e kaydedilir.
~~~~~~~
/**
 * @Route("/symphonies", name="symphony_create", methods={"POST"})
 */
public function create(Request $request): JsonResponse
{
    $data = $request->getContent();
    $symphony = $this->serializer->deserialize($data, Symphony::class, 'json');

    $errors = $this->validator->validate($symphony);
    if (count($errors) > 0) {
        $errorsString = (string) $errors;

        return new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST);
    }

    $this->symphonyRepository->save($symphony);

    $response = $this->serializer->serialize($symphony, 'json');

    return new JsonResponse($response, JsonResponse::HTTP_CREATED, [], true);
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Create%20Method.md)

##### 4. Show Method
+ ID'ye göre belirli bir symphony alınır ve döndürülür.
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_show", methods={"GET"})
 */
public function show(int $id): JsonResponse
{
    $symphony = $this->symphonyRepository->find($id);

    if (!$symphony) {
        return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
    }

    $data = $this->serializer->serialize($symphony, 'json');

    return new JsonResponse($data, JsonResponse::HTTP_OK, [], true);
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Show%20Method.md)

##### 5. Update Method
+ Request yeniden serileştirilir, symphony güncellenir ve bütün değişiklikler kaydedilir.
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_update", methods={"PUT"})
 */
public function update(Request $request, int $id): JsonResponse
{
    $data = $request->getContent();
    $symphony = $this->symphonyRepository->find($id);

    if (!$symphony) {
        return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
    }

    $this->serializer->deserialize($data, Symphony::class, 'json', [AbstractNormalizer::OBJECT_TO_POPULATE => $symphony]);

    $errors = $this->validator->validate($symphony);
    if (count($errors) > 0) {
        $errorsString = (string) $errors;

        return new JsonResponse($errorsString, JsonResponse::HTTP_BAD_REQUEST);
    }

    $this->symphonyRepository->save($symphony);

    $response = $this->serializer->serialize($symphony, 'json');

    return new JsonResponse($response, JsonResponse::HTTP_OK, [], true);
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Update%20Method.md)

##### 6. Delete Method
+ ID'ye göre belirli bir symphony silinir.
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_delete", methods={"DELETE"})
 */
public function delete(int $id): JsonResponse
{
    $symphony = $this->symphonyRepository->find($id);

    if (!$symphony) {
        return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
    }

    $this->symphonyRepository->delete($symphony);

    return new JsonResponse(null, JsonResponse::HTTP_NO_CONTENT);
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Delete%20Method.md)

#### SymphonyControllerTest'in Uygulanması
##### 1. Dependencies and Setup
+ Gerekli bağımlılıklar ve kurulum method'ları başlatılır.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SymphonyControllerTest extends WebTestCase
{
    private $client;

    protected function setUp(): void
    {
        $this->client = static::createClient();
    }
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Dependencies%20and%20Setup.md)

##### 2. Test Index Method
+ Index'deki tüm symphony'lerin döndürüldüğünden emin olunmalıdır.
~~~~~~~
public function testIndex(): void
{
    $this->client->request('GET', '/symphonies');
    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Test%20Index%20Method.md)

##### 3. Test Create Method
+ Oluşturulan smyphony'nin beklendiği gibi çalıştığından emin olunmalıdır.
~~~~~~~
public function testCreate(): void
{
    $data = [
        'name' => 'Symphony No. 1',
        'composer' => 'Beethoven',
        'createdAt' => (new \DateTime())->format('Y-m-d H:i:s')
    ];

    $this->client->request(
        'POST',
        '/symphonies',
        [],
        [],
        ['CONTENT_TYPE' => 'application/json'],
        json_encode($data)
    );

    $this->assertEquals(201, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Test%20Create%20Method.md)

##### 4. Test Show Method
+ Belirli bir symphony'nin beklendiği gibi çalıştığından emin olunmalıdır.
~~~~~~~
public function testShow(): void
{
    $this->client->request('GET', '/symphonies/1');
    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Test%20Show%20Method.md)

##### 5. Test Update Method
+ Güncellenen symphony'nin beklendiği gibi çalıştığından emin olunmalıdır.
~~~~~~~
public function testUpdate(): void
{
    $data = ['name' => 'Updated Symphony'];

    $this->client->request(
        'PUT',
        '/symphonies/1',
        [],
        [],
        ['CONTENT_TYPE' => 'application/json'],
        json_encode($data)
    );

    $this->assertEquals(200, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Test%20Update%20Method.md)

##### 6. Test Delete Method
+ Bir symphony'i silmenin beklendiği gibi çalıştığından emin olunmalıdır.
~~~~~~~
public function testDelete(): void
{
    $this->client->request('DELETE', '/symphonies/1');
    $this->assertEquals(204, $this->client->getResponse()->getStatusCode());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Test%20Delete%20Method.md)

***
### Validator
#### Symfony Validator Entegrasyon Adımları
##### 1. Symfony Validator Bileşeninin Yüklenmesi
+ İlk olarak, Composer aracılığıyla Symfony validator bileşeni yüklenmelidir. Bu, Symfony'nin güçlü doğrulama özelliklerini kullanılmasına olanak tanır.
~~~~~~~
composer require symfony/validator
~~~~~~~

##### 2. Entity Class'larını Validator Anotasyonları ile Güncelleme
+ Entity class'larına PHP öznitelikleri kullanılarak validator kısıtlamaları eklenir. Bunun için `Symfony\Component\Validator\Constraints` ad alanından gerekli kısıtlamaları içe aktarılıp ilgili özelliklere uygulanır.

###### Composer Entity
~~~~~~~
use Symfony\Component\Validator\Constraints as Assert;

class Composer
{
    #[Assert\NotBlank]
    private $firstName;

    #[Assert\NotBlank]
    private $lastName;

    #[Assert\NotBlank]
    private $dateOfBirth;

    #[Assert\NotBlank]
    #[Assert\Country]
    private $countryCode;

    // ... other properties and methods
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Validator%20Composer%20Entity.md)

###### Symphony Entity
~~~~~~~
use Symfony\Component\Validator\Constraints as Assert;

class Symphony
{
    #[Assert\NotBlank]
    private $name;

    #[Assert\NotBlank]
    #[Assert\DateTime(format: 'Y-m-d')]
    private $finishedAt;

    #[Assert\DateTime(format: 'Y-m-d')]
    private $createdAt;

    // ... other properties and methods
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Validator%20Symphony%20Entity.md)

##### 3. Validator'u Gerçekleştirmek İçin Controller Güncellemesi
+ Controller'da validator servisinin enjeksiyonunu yaparak doğrulama gerçekleştirilir ve verileri kaydetmeden önce doğrulama hataları olup olmadığını kontrol edilir. `Doğrulama başarısız olursa, 422 İşlenemeyen Varlık durumu ve doğrulama hataları ile birlikte bir yanıt döndürülür.`

###### ComposerController
~~~~~~~
use Symfony\Component\Validator\Validator\ValidatorInterface;
use Symfony\Component\HttpFoundation\Response;

class ComposerController
{
    public function create(Request $request, SerializerInterface $serializer, ValidatorInterface $validator): Response
    {
        $composer = $serializer->deserialize($request->getContent(), Composer::class, 'json');

        $errors = $validator->validate($composer);
        if (count($errors) > 0) {
            $errorsString = (string) $errors;

            return new Response($errorsString, Response::HTTP_UNPROCESSABLE_ENTITY);
        }

        // ... save to database
    }

    public function update(Request $request, SerializerInterface $serializer, ValidatorInterface $validator, int $id): Response
    {
        // ... fetch existing composer entity
        $composer = $serializer->deserialize($request->getContent(), Composer::class, 'json', ['object_to_populate' => $existingComposer]);

        $errors = $validator->validate($composer);
        if (count($errors) > 0) {
            $errorsString = (string) $errors;

            return new Response($errorsString, Response::HTTP_UNPROCESSABLE_ENTITY);
        }

        // ... update to database
    }
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Validator%20ComposerController.md)

###### SymphonyController
~~~~~~~
use Symfony\Component\Validator\Validator\ValidatorInterface;
use Symfony\Component\HttpFoundation\Response;

class SymphonyController
{
    public function create(Request $request, SerializerInterface $serializer, ValidatorInterface $validator): Response
    {
        $symphony = $serializer->deserialize($request->getContent(), Symphony::class, 'json');

        $errors = $validator->validate($symphony);
        if (count($errors) > 0) {
            $errorsString = (string) $errors;

            return new Response($errorsString, Response::HTTP_UNPROCESSABLE_ENTITY);
        }

        // ... save to database
    }

    public function update(Request $request, SerializerInterface $serializer, ValidatorInterface $validator, int $id): Response
    {
        // ... fetch existing composer entity
        $symphony = $serializer->deserialize($request->getContent(), Symphony::class, 'json', ['object_to_populate' => $existingSymphony]);

        $errors = $validator->validate($symphony);
        if (count($errors) > 0) {
            $errorsString = (string) $errors;

            return new Response($errorsString, Response::HTTP_UNPROCESSABLE_ENTITY);
        }

        // ... update to databae
    }
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Validator%20SymphonyController.md)

##### 4. Test Durumlarının Güncellenmesi
+ Doğrulamanın beklenildiği gibi çalıştığını doğrulamak ve doğrulama başarısız olduğunda doğru HTTP durum kodlarını ve hata mesajlarını alındığından emin olmak için test durumları güncellenmelidir.

###### Composer için Test
~~~~~~~
public function testCreateInvalidComposer()
{
    $invalidComposer = [
        'firstName' => 'Wolfgang',
        'dateOfBirth' => '1756-01-27',
        'countryCode' => 'XX' // Geçersiz ülke kodu
    ];

    $response = $this->client->post('/composers', [
        'json' => $invalidComposer,
    ]);

    $this->assertEquals(422, $response->getStatusCode());
    $this->assertStringContainsString('This value is not a valid country.', $response->getContent());
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Validator%20Test%20for%20Composer.md)
+ Bu adımlar takip edilerek, uygulamanın geçersiz girdileri zarif bir şekilde ele alınmasını, kullanıcılara anlamlı geri bildirimler sağlanmasını ve database'deki veri bütünlüğünü korumasını sağlar.
