### 1. Routing (Yönlendirme)
+ Symfony, bir HTTP isteğini belirli bir denetleyiciye (controller) yönlendirmek için bir yönlendirme sistemi kullanır. Yönlendirme sistemi, gelen URL'leri analiz eder ve bu URL'lerin hangi denetleyiciye ve eyleme (action) yönlendirilmesi gerektiğini belirler.

***
### 2. Controllers (Denetleyiciler)
+ Denetleyiciler, bir HTTP isteğini işler ve uygun bir yanıt (response) üretir. Bir denetleyici, iş mantığını içerir ve genellikle bir Model'den veri alır, bir Görünüm (view) oluşturur ve bu görünümü kullanıcıya döner.
~~~~~~~
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/hello", name="hello")
     */
    public function hello()
    {
        return new Response('Hello, Symfony!');
    }
}
~~~~~~~

***
### 3. Models (Modeller)
+ Model, veri tabanıyla ilgili işlemleri ve iş mantığını içerir. Symfony, genellikle Doctrine ORM (Object-Relational Mapping) kullanarak veri tabanı işlemlerini gerçekleştirir.
+ ORM, veritabanı tablolarını class'lar ve tablolardaki kayıtları nesneler olarak temsil eder. Bu sayede veritabanı işlemleri, SQL sorguları yazmadan, doğrudan bu class'lar ve nesneler üzerinden gerçekleştirilebilir. `Doctrine ORM ise daha geniş ve bağımsız bir ORM kütüphanesidir. Çeşitli PHP framework'leri ile entegre edilebilir.`
~~~~~~~
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity()
 */
class Product
{
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100)
     */
    private $name;

    // Getter and Setter methods
}
~~~~~~~

***
### 4. Views (Görünümler)
+ Views, kullanıcılara sunulacak HTML içeriğini üretir. Symfony, Twig şablon motorunu kullanarak görünümleri oluşturur. Twig, PHP tabanlı ve oldukça güçlü bir şablonlama dilidir.
~~~~~~~
{# templates/hello.html.twig #}
<!DOCTYPE html>
<html>
    <head>
        <title>Hello Symfony</title>
    </head>
    <body>
        <h1>Hello, {{ name }}!</h1>
    </body>
</html>
~~~~~~~

***
### 5. Components (Bileşenler)
+ Symfony, bir dizi yeniden kullanılabilir bileşen sunar. Bu bileşenler, veri doğrulama, form yönetimi, şablon oluşturma, güvenlik, ve daha birçok işlevi kapsar. Bu sayede, birçok işlevi sıfırdan yazmaya gerek yoktur.

***
### 6. Services & Dependency Injection (Servisler ve Bağımlılık Enjeksiyonu)
+ Servisler ve bağımlılık enjeksiyonu kullanarak uygulama bileşenlerinin bağımlılıklarını yönetir. Bu, kodun daha modüler, test edilebilir ve sürdürülebilir olmasını sağlar.
~~~~~~~
# config/services.yaml
services:
    App\Service\MyService:
        arguments:
            $someDependency: '@App\Service\SomeDependency'
~~~~~~~

***
### 7. Events and Listeners (Olaylar ve Olay Dinleyicileri)
+ Olay tabanlı bir mimariyi destekler. Belirli olaylar tetiklendiğinde, bu olaylara yanıt verecek dinleyiciler çalıştırılabilir. Bu, uygulamanın genişletilebilirliğini artırır.
~~~~~~~
namespace App\EventSubscriber;

use Symfony\Component\EventDispatcher\EventSubscriberInterface;

class MyEventSubscriber implements EventSubscriberInterface
{
    public static function getSubscribedEvents()
    {
        return [
            'kernel.request' => 'onKernelRequest',
        ];
    }

    public function onKernelRequest($event)
    {
        // Event handling codes
    }
}
~~~~~~~

+ Symfony'deki  `kernel` terimi, uygulamanın ana yapı taşı olan ve HTTP isteklerini işleyen çekirdek bileşeni ifade eder. `kernel` bileşeni, bir uygulamanın yaşam döngüsünü yönetir ve aşağıdaki görevleri yerine getirir:

1. `İstek Alma ve Yönlendirme:` `kernel`, gelen HTTP isteklerini alır ve uygun controller'a yönlendirir.
2.` Event Dispatching (Olay Yayma):` `kernel`, belirli olayları tetikler ve bu olaylara bağlı olarak event subscriber'lar veya listener'lar çağrılır.
3. `Response Handling (Yanıt İşleme):` Controller tarafından oluşturulan yanıtı alır ve HTTP yanıtı olarak döner.
4. `Exception Handling (Hata Yönetimi):` Uygulama boyunca meydana gelen istisnaları yakalar ve bunları uygun şekilde işler.

> + Yukarıdaki örnekte `kernel.request` olayı, Symfony kernel'inin bir HTTP isteği aldığında tetiklediği bir olaydır. `MyEventSubscriber` class'ı, `kernel.request` olayına abone olur ve `onKernelRequest` method'unu çağırarak bu olayı işler. Bu olay işleyici method'unda, gelen istek üzerinde çeşitli işlemler yapılabilir, örneğin doğrulama, kullanıcı oturumu kontrolü, isteğe özel verilerin eklenmesi gibi.
