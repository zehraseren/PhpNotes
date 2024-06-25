### Nelmio Bundle
##### 1. Nelmio ve Swagger PHP Bağımlılıklarının Kurulması
+ NelmioApiDocBundle ve OpenAPI ile ilgili bağımlılıklar projeye eklenir. Bu işlem `composer require` komutuyla yapılır.
~~~~~~~
composer require nelmio/api-doc-bundle
~~~~~~~
> Bu komut, NelmioApiDocBundle ve gerekli diğer bağımlılıkları projeye ekler.

##### 2. NelmioApiDocBundle'ın Yapılandırılması
+ Kurulumdan sonra Symfony, NelmioApiDocBundle'ın `recipe`'sini uygulanmasını ister. Bu recipe, projeye gerekli yapılandırma dosyalarını ekleyecektir.
+ `recipe` uygulanması kabul edilmelidir. Sonrasında, yeni yapılandırma dosyaları ve rotalar eklenecektir.

###### `config/packages/nelmio_api_doc.yaml` dosyasında gerekli ayarlamaları aşağıdaki gibidir:
~~~~~~~
nelmio_api_doc:
    documentation:
        info:
            title: 'Awesome Composer and Symphonies App API'
            description: 'API documentation for the Awesome Composer and Symphonies application.'
            version: '1.0.0'
    areas:
        default:
            path_patterns: 
                - '^/' 
            host_patterns: []
    routes:
        path_patterns: 
            - '^/'
        exclude:
            - '^/_'
            - '^/docs'
            - '^/docs.json'
            - '^/error'
~~~~~~~
> Yukarıdaki yapılandırmada, tüm rotalar için dokümantasyon oluşturulmasını sağlanır (`path_patterns'da ^/` ile belirtilmiştir). Ayrıca `exclude` kısmında dokümantasyon oluşturulması istenmeyen yollar belirtilmiştir.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/nelmio_api_doc.yaml.md)

##### 3. Dokümantasyon URL'inin Yapılandırılması
+ Dokümantasyon JSON çıktısı alınacak URL yapılandırılmalıdır.
~~~~~~~
# config/routes/nelmio_api_doc.yaml
nelmio_api_doc:
    resource: '@NelmioApiDocBundle/Resources/config/routing.yaml'
    prefix: /docs
~~~~~~~
> Bu yapılandırma ile API dokümantasyonu `/docs.json` URL'i üzerinden erişilebilir hale getirilir. Bu URL, OpenAPI uyumlu JSON döndürecektir.

##### 4. Route ve Method Yapılandırmalarının Düzeltilmesi
+ Dokümantasyonun doğru çalışması için, route'ların ve method'ların doğru yapılandırılması gerekmektedir.
###### Login rotası örneğinde olduğu gibi:
~~~~~~~
// src/Controller/SecurityController.php

use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

class SecurityController extends AbstractController
{
    /**
     * @Route("/login", name="login", methods={"POST"})
     */
    public function login(Request $request): Response
    {
        // login logic
    }
}
~~~~~~~
> Aynı şekilde diğer controller'larda da route'ların method'ları belirtildiğinden emin olunulmalıdır.

##### 5. Dokümantasyonun Test Edilmesi
+ Yapılandırmalar tamamladıktan sonra, `/docs.json` URL'i tarayıcıda veya bir API istemcisi ile kontrol edilmelidir.
~~~~~~~
curl http://localhost:8000/docs.json
~~~~~~~
> Bu URL'e erişilerek, API dokümantasyonunun JSON çıktısı görülebilir. JSON çıktısı, tüm route'ları ve method'lar için dokümantasyon bilgilerini içermektedir.

##### 6. Post ve Diğer HTTP Method'lar için Ek Bilgi Ekleme
+ Son olarak, özellikle POST method'ları için istenen veri formatını ve örnek cevapları belirtilmesi gerekmektedir. Bu bilgiler NelmioApiDocBundle ile aşağıdaki gibi eklenebilir:
~~~~~~~
use Nelmio\ApiDocBundle\Annotation\Model;
use OpenApi\Annotations as OA;

class ComposerController extends AbstractController
{
    /**
     * @OA\Post(
     *     path="/composers",
     *     summary="Create a new composer",
     *     @OA\RequestBody(
     *         required=true,
     *         @OA\JsonContent(ref=@Model(type=Composer::class))
     *     ),
     *     @OA\Response(
     *         response=201,
     *         description="Composer created successfully",
     *         @OA\JsonContent(ref=@Model(type=Composer::class))
     *     ),
     *     @OA\Response(
     *         response=400,
     *         description="Invalid input"
     *     )
     * )
     * @Route("/composers", name="create_composer", methods={"POST"})
     */
    public function create(Request $request): Response
    {
        // create composer logic
    }
}
~~~~~~~
> Bu şekilde, POST method'u için gerekli veri formatını ve olası cevaplar detaylandırılmış olunur.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/NelmioBundle%20Post%20and%20Other%20Methods.md)

> Bu adımlar takip edilerek, API dokümantasyonu oluşturulur ve yapılandırılır. Dokümantasyonun doğru çalıştığından ve tüm gerekli bilgileri içerdiğinden emin olmak için NelmioApiDocBundle ve OpenAPI'yi doğru şekilde yapılandırılmalıdır. Bu sayede, geliştiriciler API'ı kolayca kullanabilir ve entegrasyonlarını yapabilirler.

***
### OpenApi Attributes and Local UI
##### 1. Nelmio ve Bağımlılıklar Yükleme
+ Nelmio API Doc bundle ve OpenAPI bağımlılıklarını yüklemek için `composer require nelmio/api-doc-bundle` komutu çalıştırılır.
+ Symfony, Nelmio için bir recipe yürütülmesini ister, bu recipe kabul edilmelidir; `evet` seçeneği seçilmelidir.

##### 2. Nelmio'yu Yapılandırma
+ `config/packages/nelmio_api_doc.yaml` dosyasındaki yapılandırma ayarlanmalıdır. Gereksiz yol önekleri kaldırılmalıdır ve yollar uygulaman için doğru şekilde ayarlanmalıdır.

##### 3. Twig ve Asset Bileşenlerini Yükleme
+ Swagger UI'nin HTML render etmesi gerektiğinden, Twig ve Asset bileşenlerini yüklenmelidir: `composer require twig asset`.

##### 4. Swagger UI'yı Ayarlama
+ Swagger UI'ya erişim için varsayılan yolu `/docs` olarak değiştirilmelidir. Bu yol, `config/routes/nelmio_api_doc.yaml` dosyasında güncellenerek yapılmalıdır.
+ Tarayıcı açılarak `http://localhost/docs` adresine gidilir ve ayarların doğru olup olmadığını kontrol edilir.

##### 5. Controller için Etiketler Tanımlama
+ OpenAPI etiketlerini kullanarak endpoint'ler mantıksal olarak gruplanmalıdır. OpenAPI öznitelikleri içe aktarılır ve controller'larda kullanılır.
~~~~~~~
use OpenApi\Annotations as OA;

/**
 * @OA\Tag(name="Composer")
 */
class ComposerController extends AbstractController
{
    // Controller methods
}
~~~~~~~
> Diğer controller'lar için de aynı işlemi tekrarlanmalıdır, örneğin `SymphonyController` ve `AuthController`.

##### 6. Endpoint'ler Tanımlama
+ Endpoint'ler için detaylı açıklamalar eklenmelidir.
~~~~~~~
use OpenApi\Annotations as OA;

/**
 * @OA\Get(
 *     path="/composers",
 *     @OA\Response(
 *         response=200,
 *         description="Besteciler listesi",
 *         @OA\JsonContent(
 *             type="array",
 *             @OA\Items(ref=@Model(type=Composer::class))
 *         )
 *     )
 * )
 */
public function index(): JsonResponse
{
    // Endpoint logic
}
~~~~~~~
> POST, PUT ve DELETE endpoint'leri için response ve request gövdeleri benzer şekilde tanımlanmalıdır.

##### 7. Güvenliği Ayarlama
+ `nelmio_api_doc.yaml` dosyasında `bearer` kimlik doğrulaması yapılandırılmalıdır.
~~~~~~~
nelmio_api_doc:
    documentation:
        components:
            securitySchemes:
                Bearer:
                    type: http
                    scheme: bearer
~~~~~~~
+ Güvenliği controller'da global olarak uygulanmalıdır ve belirli endpoint'ler için üzerine yazılmalıdır:
~~~~~~~
/**
 * @OA\Post(
 *     path="/login",
 *     @OA\RequestBody(
 *         @OA\JsonContent(
 *             type="object",
 *             @OA\Property(property="username", type="string"),
 *             @OA\Property(property="password", type="string")
 *         )
 *     ),
 *     @OA\Response(
 *         response=200,
 *         description="Successful entry",
 *         @OA\JsonContent(
 *             type="object",
 *             @OA\Property(property="token", type="string")
 *         )
 *     )
 * )
 * @Security(name="")
 */
public function login(Request $request): JsonResponse
{
    // Input logic
}
~~~~~~~

##### 8. Swagger UI Kullanarak Endpoint'leri Test Etme
+ Swagger UI'daki `Try it out` düğmesi kullanılarak endpoint'ler test edilir. Yetkilendirme düğmesini kullanılarak kimlik doğrulaması yapılabilir ve doğrulanmış endpoint'ler test edilebilir.

##### 9. Güncel Dokümantasyon Sağlama
+ Endpoint'ler değiştirildikçe açıklamalar güncel tutulmalıdır. Böylece dokümantasyonun doğru ve kullanışlı kalması sağlanır.
  
> Bu adımlar izlenerek Symfony uygulamasına Nelmio API dokümantasyonu Swagger UI ile entegre edilmiş olunur. Böylece API endpoint'lerin belgelenmesi ve test edilmesi için güçlü bir araç sağlanır.

***
### Serializer Groups 
+ ID'lerin oluşturma ve güncelleme işlemlerindeki request'lere dahil edilmemesi gerekir. Symfony Serializer kullanarak gruplar aracılığıyla bu durumun nasıl yönetileceği ve Nelmio API dokümantasyonunda nasıl doğru şekilde gösterileceği aşağıda maddeler halinde sıralanmıştır.

##### 1. Grupları Tanımlama
+ Öncelikle, entity class'larında Serializer grupları tanımlanmalıdır. Daha sonra oluşturma ve güncelleme işlemleri için ayrı gruplar tanımlanır.

##### 2. API Controller'larını Güncelleme
+ Her bir endpoint için uygun gruplar belirtilir. Oluşturma (create) işlemi için `create` grubu, güncelleme (update) işlemi için `update` grubu kullanılır.

##### 1. Entity Class'larını Güncelleme
###### Composer Entity
~~~~~~~
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Serializer\Annotation\Groups;

/**
 * @ORM\Entity()
 */
class Composer
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue()
     * @ORM\Column(type="integer")
     * @Groups({"read"})
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100)
     * @Groups({"create", "update", "read"})
     */
    private $firstName;

    /**
     * @ORM\Column(type="string", length=100)
     * @Groups({"create", "update", "read"})
     */
    private $lastName;

    /**
     * @ORM\Column(type="date")
     * @Groups({"create", "update", "read"})
     */
    private $dateOfBirth;

    /**
     * @ORM\Column(type="string", length=2)
     * @Groups({"create", "update", "read"})
     */
    private $countryCode;

    // Other properties and getter/setter methods
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Serialiazer%20Groups%20-%20Composer%20Entity.md)

###### Symphony Entity
~~~~~~~
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Serializer\Annotation\Groups;

/**
 * @ORM\Entity()
 */
class Symphony
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue()
     * @ORM\Column(type="integer")
     * @Groups({"read"})
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100)
     * @Groups({"create", "update", "read"})
     */
    private $name;

    /**
     * @ORM\Column(type="text")
     * @Groups({"create", "update", "read"})
     */
    private $description;

    /**
     * @ORM\Column(type="datetime")
     * @Groups({"update", "read"})
     */
    private $finishedAt;

    // Other properties and getter/setter methods
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Serialiazer%20Groups%20-%20Symphony%20Entity.md)

##### 2. API Controller'larını Güncelleme
###### ComposerController
~~~~~~~
namespace App\Controller;

use App\Entity\Composer;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;
use OpenApi\Annotations as OA;
use Nelmio\ApiDocBundle\Annotation\Model;

class ComposerController extends AbstractController
{
    /**
     * @Route("/composers", methods={"GET"})
     * @OA\Get(
     *     @OA\Response(
     *         response=200,
     *         description="Composers list",
     *         @OA\JsonContent(
     *             type="array",
     *             @OA\Items(ref=@Model(type=Composer::class, groups={"read"}))
     *         )
     *     )
     * )
     */
    public function index(): JsonResponse
    {
        // Composer listing logic
    }

    /**
     * @Route("/composers", methods={"POST"})
     * @OA\Post(
     *     @OA\RequestBody(
     *         @OA\JsonContent(
     *             ref=@Model(type=Composer::class, groups={"create"})
     *         )
     *     ),
     *     @OA\Response(
     *         response=201,
     *         description="New composer created",
     *         @OA\JsonContent(
     *             ref=@Model(type=Composer::class, groups={"read"})
     *         )
     *     )
     * )
     */
    public function create(SerializerInterface $serializer): JsonResponse
    {
        // Composer creation logic
    }

    /**
     * @Route("/composers/{id}", methods={"PUT"})
     * @OA\Put(
     *     @OA\RequestBody(
     *         @OA\JsonContent(
     *             ref=@Model(type=Composer::class, groups={"update"})
     *         )
     *     ),
     *     @OA\Response(
     *         response=200,
     *         description="Composer updated",
     *         @OA\JsonContent(
     *             ref=@Model(type=Composer::class, groups={"read"})
     *         )
     *     )
     * )
     */
    public function update(int $id, SerializerInterface $serializer): JsonResponse
    {
        // Composer update logic
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Serialiazer%20Groups%20-%20ComposerController.md)

###### SymphonyController
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
        // Symphony listing logic
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
        // Symphony creation logic
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
        // Symphony update logic
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Serialiazer%20Groups%20-%20SymphonyController.md)

##### 3. Dokümantasyonu Güncelleme ve Kontrol Etme
+ Tüm bu değişikliklerden sonra Swagger UI yenilenerek dokümantasyonun güncel ve doğru olduğunu kontrol edilebilir. Böylece `create` ve `update` isteklerinde ID dahil edilmemiş olacak ancak `read` grubu sayesinde yanıtların içinde yer alacaktır.

> Bu adımlar takip edilerek Symfony projesinde API endpoint'ler daha net ve kullanıcı dostu hale getirilir. Dokümantasyon, tüketicilerin doğru ve net bir şekilde API kullanmalarına yardımcı olacaktır.
