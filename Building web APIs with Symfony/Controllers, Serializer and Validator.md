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
~~~~~~~
// tests/Controller/ComposerControllerTest.php
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
+ 
~~~~~~~
~~~~~~~
>

***
### Validator
+
~~~~~~~
~~~~~~~
> 
