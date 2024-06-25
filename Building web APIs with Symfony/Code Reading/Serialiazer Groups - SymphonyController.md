+ `SymphonyController` class'ı, Symfony uygulamasında symphony nesneleri ile ilgili CRUD (Create, Read, Update, Delete) işlemlerini gerçekleştiren bir controller olarak tanımlanmıştır.
+ Bu class, NelmioApiDocBundle ve OpenAPI annotations kullanarak API dokümantasyonu da sağlar.
~~~~~~~
namespace App\Controller;

use App\Entity\Symphony;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;
use OpenApi\Annotations as OA;
use Nelmio\ApiDocBundle\Annotation\Model;

class SymphonyController extends AbstractController
{
    /**
     * @Route("/symphonies", methods={"GET"})
     * @OA\Get(
     *     @OA\Response(
     *         response=200,
     *         description="Symphonies list",
     *         @OA\JsonContent(
     *             type="array",
     *             @OA\Items(ref=@Model(type=Symphony::class, groups={"read"}))
     *         )
     *     )
     * )
     */
    public function index(): JsonResponse
    {
        $symphonies = $this->getDoctrine()->getRepository(Symphony::class)->findAll();
        return $this->json($symphonies, 200, [], ['groups' => 'read']);
    }

    /**
     * @Route("/symphonies", methods={"POST"})
     * @OA\Post(
     *     @OA\RequestBody(
     *         @OA\JsonContent(
     *             ref=@Model(type=Symphony::class, groups={"create"})
     *         )
     *     ),
     *     @OA\Response(
     *         response=201,
     *         description="New symphony created",
     *         @OA\JsonContent(
     *             ref=@Model(type=Symphony::class, groups={"read"})
     *         )
     *     )
     * )
     */
    public function create(SerializerInterface $serializer): JsonResponse
    {
        $request = $this->container->get('request_stack')->getCurrentRequest();
        $symphony = $serializer->deserialize($request->getContent(), Symphony::class, 'json');

        $entityManager = $this->getDoctrine()->getManager();
        $entityManager->persist($symphony);
        $entityManager->flush();

        return $this->json($symphony, 201, [], ['groups' => 'read']);
    }

    /**
     * @Route("/symphonies/{id}", methods={"PUT"})
     * @OA\Put(
     *     @OA\RequestBody(
     *         @OA\JsonContent(
     *             ref=@Model(type=Symphony::class, groups={"update"})
     *         )
     *     ),
     *     @OA\Response(
     *         response=200,
     *         description="Symphony updated",
     *         @OA\JsonContent(
     *             ref=@Model(type=Symphony::class, groups={"read"})
     *         )
     *     )
     * )
     */
    public function update(int $id, SerializerInterface $serializer): JsonResponse
    {
        $request = $this->container->get('request_stack')->getCurrentRequest();
        $entityManager = $this->getDoctrine()->getManager();
        $symphony = $entityManager->getRepository(Symphony::class)->find($id);

        if (!$symphony) {
            return $this->json(['message' => 'Symphony not found'], 404);
        }

        $serializer->deserialize($request->getContent(), Symphony::class, 'json', ['object_to_populate' => $symphony]);
        $entityManager->flush();

        return $this->json($symphony, 200, [], ['groups' => 'read']);
    }
}
~~~~~~~

##### 1. Namespace ve Kullanılan Class'lar
+ `namespace App\Controller;`: Class'ın bulunduğu namespace.
+ `use` ifadeleri: Gerekli class'ları ve anotasyonları kullanabilmek için dahil eder.

##### 2. index Method
+ `@Route("/symphonies", methods={"GET"})`: `/symphonies` URL'ine GET isteği yapıldığında bu method'un çalışacağını belirtir.
+ `@OA\Get`: OpenAPI dokümantasyonu için GET isteğini tanımlar.
+ `@OA\Response`: HTTP 200 yanıtında döndürülecek veriyi tanımlar.
+ `public function index(): JsonResponse`: Symphony'lerin listesini döndüren method.
+ `findAll()` method'u ile tüm Symphony nesneleri veritabanından çekilir ve JSON formatında döndürülür.

##### 3. create Method
+ `@Route("/symphonies", methods={"POST"})`: `/symphonies` URL'ine POST isteği yapıldığında bu method'un çalışacağını belirtir.
+ `@OA\Post`: OpenAPI dokümantasyonu için POST isteğini tanımlar.
+ `@OA\RequestBody`: İstek gövdesinin JSON formatında olacağını ve Symphony class'ını referans alacağını belirtir.
+ `@OA\Response`: HTTP 201 yanıtında döndürülecek veriyi tanımlar.
+ `public function create(SerializerInterface $serializer): JsonResponse`: Yeni bir symphony oluşturma method'u.
+ `$serializer->deserialize` method'u ile JSON formatındaki istek verisi `Symphony` nesnesine dönüştürülür ve veritabanına kaydedilir.

##### 4. update Method
+ `@Route("/symphonies/{id}", methods={"PUT"})`: `/symphonies/{id}` URL'ine PUT isteği yapıldığında bu method'un çalışacağını belirtir.
+ `@OA\Put`: OpenAPI dokümantasyonu için PUT isteğini tanımlar.
+ `@OA\RequestBody`: İstek gövdesinin JSON formatında olacağını ve `Symphony` class'ını referans alacağını belirtir.
+ `@OA\Response`: HTTP 200 yanıtında döndürülecek veriyi tanımlar.
+ `public function update(int $id, SerializerInterface $serializer): JsonResponse`: Varolan bir Symphony'i güncelleme method'u.
+ `$serializer->deserialize` method'u ile JSON formatındaki istek verisi mevcut `Symphony` nesnesine dönüştürülür ve veritabanında güncellenir.

