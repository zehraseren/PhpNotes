+ Bu PHP kodu, Symfony framework'ü kullanarak yazılmış bir `ComposerController` class'ını temsil eder.
+ Bu class, `Composer` varlıkları üzerinde CRUD (Create, Read, Update, Delete) işlemlerini gerçekleştiren çeşitli method'lar içerir.

#### Kullanılan Namespace ve Bileşenler
~~~~~~~
namespace App\Controller;

use App\Entity\Composer;
use App\Repository\ComposerRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;
use Symfony\Component\Serializer\Normalizer\AbstractNormalizer;
~~~~~~~
> + `App\Entity\Composer`: `Composer` entity class.
> + `App\Repository\ComposerRepository`: `Composer` entity'leri için database deposu (repostory).
> + `Symfony\Bundle\FrameworkBundle\Controller\AbstractController`: Symfony'nin temel controller class'ı. 
> + `Symfony\Component\HttpFoundation\JsonResponse`: JSON formatında HTTP yanıtı oluşturmak için kullanılır.
> + `Symfony\Component\HttpFoundation\Request`: HTTP isteklerini temsil eder.
> + `Symfony\Component\Routing\Annotation\Route`: Routing işlemlerini tanımlamak için kullanılır.
> + `Symfony\Component\Serializer\SerializerInterface`: Symfony'nin seri hale getirme (serialization) ve seri halden çıkarma (deserialization) işlevlerini sağlar.
> + `Symfony\Component\Serializer\Normalizer\AbstractNormalizer`: Normalleştirme (normalization) işlemlerinde kullanılabilecek bir temel class.

##### `ComposerController` Class
+ Bu class, `Composer` entity'leri üzerinde CRUD işlemleri gerçekleştiren beş method içerir:
  - `index`
  - `show`
  - `create`
  - `update`
  - `delete`

###### `index` Method
~~~~~~~
#[Route('/composer', name: 'composer_index', methods: ['GET'])]
public function index(ComposerRepository $repo): JsonResponse
{
    $composers = $repo->findAll();
    return $this->json($composers);
}
~~~~~~~
> + `/composer` URL'ine bir GET isteği yapıldığında çalışır.
> + Tüm `Composer` entity'lerini database'den getirir ve JSON formatında döner.

##### `show` Method
~~~~~~~
#[Route('/composer/{id}', name: 'composer_show', methods: ['GET'])]
public function show(Composer $composer): JsonResponse
{
    return $this->json($composer);
}
~~~~~~~
> + `/composer/{id}` URL'ine bir GET isteği yapıldığında çalışır.
> + Belirtilen ID'ye göre `Composer` entity'sini database'den getirir ve JSON formatında döner.

##### `create` Method
~~~~~~~
#[Route('/composer', name: 'composer_create', methods: ['POST'])]
public function create(Request $request, ComposerRepository $repo, SerializerInterface $serializer): JsonResponse
{
    $composer = $serializer->deserialize($request->getContent(), Composer::class, 'json');
    $repo->save($composer, true);
    return $this->json($composer, JsonResponse::HTTP_CREATED);
}
~~~~~~~
> + `/composer` URL'ine bir POST isteği yapıldığında çalışır.
> + İstek içeriğini (JSON formatında) `Composer` entity'sine dönüştürür ve database'e kaydeder.
> + Yeni oluşturulan `Composer` entity'sini JSON formatında döner ve HTTP 201 (Created) durumu ile yanıt verir.

##### `update` Method
~~~~~~~
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
~~~~~~~
> + `/composer/{id}` URL'ine bir PUT isteği yapıldığında çalışır.
> + İstek içeriğini (JSON formatında) mevcut `Composer` entity'sine uygular ve database'e kaydeder.
> + Güncellenmiş `Composer` entity'sini JSON formatında döner.

##### `delete` Method
~~~~~~~
#[Route('/composer/{id}', name: 'composer_delete', methods: ['DELETE'])]
public function delete(ComposerRepository $repo, Composer $composer): JsonResponse
{
    $repo->remove($composer, true);
    return new JsonResponse(null, JsonResponse::HTTP_NO_CONTENT);
}
~~~~~~~
> + `/composer/{id}` URL'ine bir DELETE isteği yapıldığında çalışır.
> + Belirtilen ID'ye göre `Composer` entity'sini database'den siler.
> + Boş bir JSON yanıtı döner ve HTTP 204 (No Content) durumu ile yanıt verir.
