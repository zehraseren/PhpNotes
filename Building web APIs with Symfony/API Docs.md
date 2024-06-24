### Nelmio Bundle
##### 1. Nelmio ve Swagger PHP Bağımlılıklarının Kurulması
+ NelmioApiDocBundle ve OpenAPI ile ilgili bağımlılıklar projeye eklenir. Bu işlem `composer require` komutuyla yapılır.
~~~~~~~
composer require nelmio/api-doc-bundle
~~~~~~~
> Bu komut, NelmioApiDocBundle ve gerekli diğer bağımlılıkları projeye ekler.

##### 2. NelmioApiDocBundle'ın Yapılandırılması
+ Kurulumdan sonra Symfony, NelmioApiDocBundle'ın `recipe`'sini uygulamasını ister. Bu recipe, projeye gerekli yapılandırma dosyalarını ekleyecektir. `recipe` uygulamayı kabul edilmelidir. Sonrasında, yeni yapılandırma dosyaları ve rotalar eklenecektir.
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

##### 3. Dokümantasyon URL'sinin Yapılandırılması
+ Dokümantasyon JSON çıktısı alınacak URL'i yapılandırılmalıdır.
~~~~~~~
# config/routes/nelmio_api_doc.yaml
nelmio_api_doc:
    resource: '@NelmioApiDocBundle/Resources/config/routing.yaml'
    prefix: /docs
~~~~~~~
> Bu yapılandırma ile API dokümantasyonu `/docs.json` URL'i üzerinden erişilebilir hale getirilir. Bu URL, OpenAPI uyumlu JSON döndürecektir.

##### 4. Rota ve Method Yapılandırmalarının Düzeltilmesi
+ Dokümantasyonun doğru çalışması için, rotaların ve method'ların doğru yapılandırılması gerekmektedir.
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
> Aynı şekilde diğer controller'larda da rotaların method'ları belirtildiğinden emin olunulmalıdır.

##### 5. Dokümantasyonun Test Edilmesi
+ Yapılandırmalar tamamladıktan sonra, `/docs.json` URL'i tarayıcıda veya bir API istemcisi ile kontrol edilmelidir.
~~~~~~~
curl http://localhost:8000/docs.json
~~~~~~~
> Bu URL'e erişerek, API dokümantasyonunun JSON çıktısı grülebilir. JSON çıktısı, tüm rotaları ve method'lar için dokümantasyon bilgilerini içerecektir.

##### 6. Post ve Diğer HTTP Metodları için Ek Bilgi Ekleme
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

> Bu adımlar takip edilerek, API dokümantasyonu oluşturuldu ve yapılandırıldı. Dokümantasyonun doğru çalıştığından ve tüm gerekli bilgileri içerdiğinden emin olmak için NelmioApiDocBundle ve OpenAPI'yi doğru şekilde yapılandırılmalıdır. Bu sayede, geliştiriciler API'yi kolayca kullanabilir ve entegrasyonlarını yapabilirler.

***
### OpenApi Attributes and Local UI
+ 
~~~~~~~
~~~~~~~
>

***
### Serializer Groups 
+ 
~~~~~~~
~~~~~~~
>
