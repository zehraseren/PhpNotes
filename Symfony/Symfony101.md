### Symfony Directory Structure Overview | Symfony Dosya ve Dizin Yapısı
+ Symfony projesinin dosya ve dizin yapısı oldukça düzenlidir ve birçok dosya ve dizin içerir. Bu yapı, projenin farklı bileşenlerini organize etmek için kullanılır ve projenin kolayca yönetilmesini sağlar.
1. `bin/:` Symfony uygulamasının çalıştırılabilir dosyalarını içerir. Örneğin, console betiği, uygulamanın konsoldan yönetmek için kullanılır.
2. `config/:` Projenin yapılandırma dosyalarını içerir. Genellikle YAML dosyalarıyla yapılandırma yapılır.
3. `public/:` Uygulamanın web sunucusuna erişilebilecek dosyalarını içerir. Ana giriş noktası olan `index.php` dosyası burada bulunur.
4. `src/:` Uygulamanın iş mantığını içerir. Controller, hizmetler, formalar gibi dosyalar burada yer alır.
5. `templates/:` Twig şablonlarını içerir. Kullanıcı arayüzünü oluşturmak için kullanılır.
6. `var/:` Uygulamanın çalışması sırasında oluşturulan geçici dosyaları içerir. Örneğin, önbellek dosyaları, günlük dosyaları burada bulunur.
7. `vendor/:` Composer tarafından yönetilen bağımlılıkları içerir. Symfony çekirdeği ve diğer üçüncü taraf kütüphaneleri burada bulunur.
8. `composer.json:` Projenin Composer bağımlılıklarını ve yapılandırmasını tanımlar.
9. `composer.lock:` Composer tarafından oluşturulan ve yüklü bağımlılıkların sürümlerini içeren bir kilit dosyasıdır.
10. `env:` Uygulamanın ortam değişkenlerini içerir. Örneğin, veritabanı bağlantı bilgileri gibi gizli bilgiler burada saklanır.
11. `index.php:` Uygulamanın ana giriş noktasıdır. Symfony çekirdeğini başlatır ve istekleri işler.

***
### How Symfony Works? | Symfony Nasıl Çalışır?
![How Symfony Works@2x](https://github.com/zehraseren/PhpNotes/assets/94180168/a9f4be4f-13e2-4daa-8622-4ab9c054fbc6)
+ Symfony uygulamasının işlem akışı, diyagramda tasvir edildiği gibi şu şekilde özetlenebilir:
1. `Kullanıcı Tarayıcı İsteği:` Etkileşim, kullanıcının Symfony uygulamasında bir web sayfasını görüntülemek veya bir işlem gerçekleştirmek için web tarayıcısından bir istek göndermesiyle başlar.
2. `HTTP Sunucusu:` İstek, uzaktaki bir fiziksel sunucuda çalışan bir HTTP sunucusu (örneğin, Apache veya Nginx) tarafından alınır. HTTP sunucusunun görevi gelen istekleri işlemek ve bunları PHP ikilisine iletmektir.
3. `PHP İkilisi:` HTTP sunucusu, isteği Symfony uygulamasının kodunu çalıştırmakla sorumlu olan PHP ikilisine iletilir. Uygulamanın yürütülmesinin başlangıç noktası genellikle `index.php` dosyasıdır.
4. `Yönlendirme:` İçsel olarak, Symfony uygulaması her gelen isteği nasıl ele alacağını belirlemek için yönlendirme adı verilen bir mekanizma kullanır. Yönlendirme, isteğin URL'ni belirli bir denetleyici eylemine eşler.
5. `Controller | Denetleyici:` Yönlendirme sistemi, istek için uygun denetleyici eylemini belirlediğinde, denetleyici, isteği işlemek ve bir yanıt oluşturmakla sorumludur. Denetleyiciler, uygulamanın ana operatörleri olarak hizmet verir ve veritabanından veri almak, form gönderimlerini işlemek ve HTML çıktısı oluşturmak için şablonları işleyebilirler.
6. `Yanıt Oluşturma:` Controller, HTML, JSON veya XML gibi çeşitli biçimlerde olabilen bir yanıt oluşturur. Yanıt, uygulama tarafından oluşturulan dinamik içeriği, yanıt başlıklarını ve çerezleri içerebilir.
7. `HTTP Sunucusu Yanıtı:` Oluşturulan yanıt, HTTP sunucusuna geri gönderilir, ardından kullanıcının web tarayıcısına iletilir. Yanıt, istenen içeriğin yanı sıra herhangi ek HTTP başlıkları, çerezler veya durum kodları içerebilir.

***
### Controllers - Returning a Response | Denetleyici - Yanıt Döndürme 
+ Symfony uygulamasında her istek controllers tarafından işlenir.
+ Controller, `src/controller` klasöründe bulunur.
> `HelloController.php` adında bir dosya oluşturulsun.

##### 1. Namespace Tanımlama
+ Tüm `src` klasöründeki kodlar bir `namespace` ile tanımlanmalıdır.
+ Hangi namespace kullanılacağı `composer.json` dosyasında yapılandırılmıştır.

###### composer.json
~~~~~~~
"autoload": {
    "psr-4": {
        "App\\": "src/"
    }
}
~~~~~~~
+ Controller dosyası için namespace `App\Controller` olmalıdır. Controller dosyasında namespace tanımlanır.

###### HelloController.php
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;

class HelloController
{
    public function index(): Response
    {
        return new Response('Hello');
    }
}
~~~~~~~

##### 2. Yönlendirme (Routing) Tanımlama
+ Controller'in çalışması için routing yapılandırması gerekmektedir. Routing, hangi URL'in hangi controller eylemine yönlendirileceğini belirler. Bu yapılandırma `config/routes.yaml` dosyasında yapılır.
~~~~~~~
hello:
    path: /
    controller: App\Controller\HelloController::index
~~~~~~~

##### 3. Projeyi Çalıştırma
+ Değişiklik kaydedilip tarayıcıda ana sayfa refresh edilince ekranda `Hello` metni gözükür.
  - PHP class'ı dosya adı class adı ile aynı olmalıdır.
  - Bir dosyada birden fazla class tanımlanmamalıdır.
  - PHP'nin en son sürümlerinde, her fonksiyon veya method'un dönüş türü belirtilmelidir.
  
###### Mevcut Yönlendirmeleri Görüntüleme
> Uygulamanda tanımlı mevcut yönlendirmeleri ve hangi controller'in çağrıldığını görmek için `symfony console debug:router` komutu kullanılır. Bu komut, uygulamada tanımlı tüm yönlendirmeleri ve controller'ı listeler.

***
### Routing Using PHP 8 Attributes
+ Controller'da yazılan aksiyonun istek atabilmek için bir rota tanımlama yapılması gerekmektedir. Rotaları, `routes.yaml` dosyasında tanımlanır. Ancak bunun için daha daha iyi bir yol bulunmaktadır. Böylece ya anotasyonları ya da PHP 8'den itibaren yerleşik olarak gelen özellikleri (attributes) kullanılabilir.

#### Özelliklerle (Attributes) Rota Tanımlama
+ Özellikler ve anotasyonlar aynı şekilde çalışır. Method'a meta veri sağlarlar ve Symfony tarafından okunup anlaşılırlar.
+ Özellikler PHP'nin yerleşik bir özelliği olduğundan, özellikle PHP 8'den itibaren `routes.yaml` dosyasında tanımlamak yerine, rota eşleştirildikten sonra çalışacak aksiyonun hemen üzerine bir özellik eklenir.
 
###### Özellik Ekleyerek Rota Tanımlama
+ `config/routes.yaml` dosyasındaki route yapılacak controller'ların satırları yoruma alınması gerektiği unutulmamalıdır.
+ Yukarıdaki örnekte oluşturulan `HelloController`'daki index method'unun üzerine eklenecek özellik aşağıdaki gibidir:
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class HelloController
{
    #[Route('/', name: 'app_index')]
    public function index(): Response
    {
        return new Response('Hello');
    }
~~~~~~~
> Burada `Route` özelliği kullanılarak yolu ve rota adını tanımlanır. `Route` class'ını da içe aktarılması gerekir.

##### Ekstra Aksiyonlar Ekleme
+ Aynı controller'a daha fazla aksiyon eklensin. Örneğin, `showOne` adında bir method eklensin ve bu method bir parametre alsın.
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class HelloController
{
    private array $messages = ['Hello', 'Hi', 'Bye!'];

    #[Route('/', name: 'app_index')]
    public function index(): Response
    {
        return new Response(implode(', ', $this->messages));
    }

    #[Route('/messages/{id}', name: 'app_show_one')]
    public function showOne(int $id): Response
    {
        if (!isset($this->messages[$id])) {
            return new Response('Message not found.', 404);
        }

        return new Response($this->messages[$id]);
    }
}
~~~~~~~
> + Burada `showOne` method'unda bir `id` parametresi alınır ve bu parametreye göre bir mesaj döndürülür. Yolu `Route` özelliği ile tanımlanır ve parametre yolun içine `{id}` olarak eklenir.
> + Mevcut rotaları ve hangi controller'ın çağrıldığını görmek için `symfony console debug:router` komutu çağrılarak bakılabilir.

***
### Route Parameter Requirements / Optional Parameters | Rota Parametre Gereksinimleri / İsteğe Bağlı Parametreler

#### Parametre Gereksinimleri
+ ID parametresi bir sayısal değer olmalıdır. PHP'de array'ler varsayılan olarak sıfırdan başlayarak sayılarla indekslenir. Bu nedenle, bu parametrenin türünü tam sayı olarak belirtilmelidir. Ancak, `messages/hello` gibi bir URL yazıldığında bir hata alınır çünkü array indeksleri sayısal olmalıdır. `Bu durumu önceden ele alınması gerekmektedir.`
+ Bu tür sorunları ele almak için rota tanımına `requirements` adı verilen bir ek parametre eklenebilir. Böylece, her rota parametresi için beklenen türü belirleyen bir düzenli ifade (regex) tanımlanmasına olanak tanır. Örneğin şu şekilde yapılabilir:
~~~~~~~
#[Route('/messages/{id}', name: 'app_show_one', requirements: ['id' => '\d+'])]
public function show(int $id): Response
{
    if (!isset($this->messages[$id])) {
        return new Response('Message not found.', 404);
    }

    return new Response($this->messages[$id]);
}
~~~~~~~
> `requirements` ile `id` parametresi yalnızca sayısal değerleri kabul eder. Dolayısıyla `messages/hello` gibi bir istek yapıldığında `404 hatası` döner, çünkü belirtilen rota bulunamaz.

#### İsteğe Bağlı Parametreler
+ Bir isteğe bağlı parametre eklemek de mümkündür. Örneğin, index aksiyonuna kaç mesajın gösterileceğini belirleyen bir `limit` parametresi eklenebilir. Bu parametre varsayılan olarak 3 alınabilir.
~~~~~~~
#[Route('/{limit?3}', name: 'app_index', requirements: ['limit' => '\d+'])]
#[Route('/{limit<\d+>?3}', name: 'app_index')] şeklinde de yazılabilir.
public function index(int $limit = 3): Response
{
    $messages = array_slice($this->messages, 0, $limit);
    return new Response(implode(', ', $messages));
}
~~~~~~~
> URL'de bir `limit` parametresi belirtilmezse varsayılan olarak 3 mesaj döndürülecektir. Aksi halde, belirtilen sayı kadar mesaj döndürülür.

##### Rotaların Sırası ve Öncelik
+ Bazı durumlarda, rotaların yolları çok benzer olabilir. Örneğin, aşağıdaki iki rota ele alınsın:
~~~~~~~
#[Route('/blog/{slug}', name: 'blog_show')]
public function show(string $slug): Response
{
    // ...
}

#[Route('/blog/static', name: 'blog_static')]
public function static(): Response
{
    // ...
}
~~~~~~~
> Burada, `/blog/static` rotası, `/blog/{slug}` rotası ile de eşleşebilir çünkü `{slug}` için herhangi bir gereksinim belirtilmemiştir. Bu durumda, `/blog/static` rotası beklenenden farklı bir şekilde eşleşebilir. Bu durumda yapılması gereken şey rota tanımlamalarına `priority` parametresinin eklenmesidir.
~~~~~~~
#[Route('/blog/static', name: 'blog_static', priority: 1)]
public function static(): Response
{
    // ...
}

#[Route('/blog/{slug}', name: 'blog_show')]
public function show(string $slug): Response
{
    // ...
}
~~~~~~~
> Böylece `priority` değeri sıfırdan büyük olan rotalar önce eşleştirilecektir.

***
### Twig Templates | Twig Şablonları
+ Symfony'de controller eylemlerinden direkt olarak string olarak yanıt döndürmek basit olabilir ama karmaşık HTML çıktısı için sürdürülebilir değildir.
+ HTML veya diğer çıktı formatlarını daha uygun bir şekilde oluşturmak için şablonlar kullanılmalıdır. Symfony, şablon motoru olarak Twig kullanır.

#### Twig'in Kurulumu
+ Aşağıdaki komut, Twig'i kurar ve gerekli ayar dosyalarını ekleyerek Symfony uygulamasını Twig kullanımı için yapılandırır.
~~~~~~~
composer require twig
~~~~~~~

#### Twig Şablonlarının Oluşturulması ve Kullanılması
+ Şablonlar varsayılan olarak templates dizininde saklanır. Her controller genellikle templates içinde kendi dizinine sahiptir ve her eylemin ilgili bir şablon dosyası vardır.

##### 1. AbstractController'ı Genişletmek
+ Controller, Twig şablonlarını işlemek için render yöntemini sağlayan AbstractController'ı genişletecek şekilde güncellenmelidir.
~~~~~~~
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class HelloController extends AbstractController
{
    private array $messages = ['Hello', 'Hi', 'Bye!'];

    #[Route('/messages/{id}', name: 'app_show_one', requirements: ['id' => '\d+'])]
    public function show(int $id): Response
    {
        if (!isset($this->messages[$id])) {
            return new Response('Message not found.', 404);
        }

        return $this->render('hello/show_one.html.twig', [
            'message' => $this->messages[$id]
        ]);
    }

    #[Route('/{limit?3}', name: 'app_index', requirements: ['limit' => '\d+'])]
    public function index(int $limit = 3): Response
    {
        $messages = array_slice($this->messages, 0, $limit);
        return $this->render('hello/index.html.twig', [
            'messages' => $messages
        ]);
    }
}
~~~~~~~

##### 2. Şablon Dosyaları Oluşturma
+ `templates` dizini içinde hello adında bir klasör oluşturulsun. Ardından iki Twig dosyası oluşturulsun: `show_one.html.twig` ve `index.html.twig`

###### show_one.html.twig
~~~~~~~
<div>
    <b>{{ message }}</b>
</div>
~~~~~~~

###### index.html.twig
~~~~~~~
<div>
    <ul>
        {% for message in messages %}
            <li>{{ message }}</li>
        {% endfor %}
    </ul>
</div>
~~~~~~~

#### Render Yönteminin Kullanımı
+ `render` method'u, belirtilen Twig şablonunu render ederek bir `Response` nesnesi oluşturur. İki parametre alır:
 1. Twig şablon dosyasının yolu
 2. Şablona geçilecek veri dizisi
~~~~~~~
return $this->render('hello/show_one.html.twig', [
    'message' => $this->messages[$id]
]);
~~~~~~~

***
### Twig Template Inheritance | Twig Şablonları Kalıtımı
+ Twig şablon motoru, Symfony'de HTML çıktısı oluştururken tekrar eden kodlardan kaçınılmasını sağlayan güçlü bir araçtır. Şablon mirası ve layout kullanarak, sayfanın genel yapısını merkezi bir yerden yönetilebilir ve her şablonda tekrar eden kod yazmaktan kaçınılmasına yardımcı olur.

##### Temel Şablon (Base Template)
+ Öncelikle, her sayfada ortak olan HTML yapısını içeren bir temel şablon oluşturulması gerekmektedir. Symfony, kurulum sırasında bir `base.html.twig` dosyası oluşturulur. Bu dosya genellikle şablonların ana yapısını içerir. Örneğin:
~~~~~~~
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Welcome!{% endblock %}</title>
    {# Stylesheets can be included here #}
    {% block stylesheets %}{% endblock %}
</head>
<body>
    {# Common header can be included here #}
    {% block header %}{% endblock %}

    {# Main content of the page will be inserted here #}
    {% block body %}{% endblock %}

    {# Scripts can be included here #}
    {% block javascripts %}{% endblock %}
</body>
</html>
~~~~~~~
> Diğer tüm şablonların miras alacağı temel yapıyı sağlar. `block` ve `endblock` arasında tanımlanan bölümler, alt şablonlar tarafından doldurulabilir.

##### Alt Şablonların Oluşturulması
+ Alt şablonlar, temel şablonu miras alır ve belirli bloklar içinde özel içerik sağlar. Örneğin, `index.html.twig` ve `show_one.html.twig` şablonları güncellenirse:

###### index.html.twig
~~~~~~~
{% extends 'base.html.twig' %}

{% block title %}All Messages{% endblock %}

{% block body %}
    <h1>Messages</h1>
    <ul>
        {% for message in messages %}
            <li>{{ message }}</li>
        {% endfor %}
    </ul>
{% endblock %}
~~~~~~~

###### index.html.twig
~~~~~~~
{% extends 'base.html.twig' %}

{% block title %}Message Details{% endblock %}

{% block body %}
    <h1>Message Details</h1>
    <p><strong>{{ message }}</strong></p>
{% endblock %}
~~~~~~~

#### Şablon Mirası ve Kullanımı
+ Yukarıdaki şablonlarda görüldüğü gibi, her şablon `base.html.twig` şablonunu genişletir (extends). Belirli bölümler (block) tanımlanır ve bu blokların içeriği alt şablonlarda doldurulur. Bu sayede, sayfa başlığı (title), ana içerik (body) gibi belirli alanları her şablonda özelleştirebilir.

###### Controller Güncellemeleri
~~~~~~~
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class HelloController extends AbstractController
{
    private array $messages = ['Hello', 'Hi', 'Bye!'];

    #[Route('/messages/{id}', name: 'app_show_one', requirements: ['id' => '\d+'])]
    public function show(int $id): Response
    {
        if (!isset($this->messages[$id])) {
            return new Response('Message not found.', 404);
        }

        return $this->render('hello/show_one.html.twig', [
            'message' => $this->messages[$id]
        ]);
    }

    #[Route('/{limit?3}', name: 'app_index', requirements: ['limit' => '\d+'])]
    public function index(int $limit = 3): Response
    {
        $messages = array_slice($this->messages, 0, $limit);
        return $this->render('hello/index.html.twig', [
            'messages' => $messages
        ]);
    }
}
~~~~~~~

***
### Twig Control Structure (if/for) | Twig Kontrol Yapıları

#### Mevcut Problemin İncelenmesi
+ Controller'daki index fonksiyonu içinde bulunan index şablonunu render etmeye çalışırken bu şablona bir array veriyi geçirilir. Bu dizi direkt olarak render edilemez çünkü bu bir hata oluşturur. Bu nedenle, array'i bir dizeye dönüştürmek için PHP `implode` fonksiyonunu kullanılır.
+ Ancak, bu tür işlemler sunum katmanının (Twig şablonları) işi olup, controller'ların işi olmamalıdır. `Controller'ın yapması gereken tek şey verileri almak, gerekirse değiştirmek ve ardından bu verileri şablon motoruna (Twig) geçirmektir.` Sunumun nasıl yapılacağını Twig şablonları belirlemelidir.
+ Dolayısıyla controller sadece verileri geçirmelidir ve Twig şablonları içinde bu verilerin nasıl render edileceğine karar verilmelidir. Yani `implode` fonksiyonunu kullanmak yerine, array verisi olduğu gibi geçirilmelidir.
~~~~~~~
public function index()
{
    // Data preparing
    $messages = array_slice($this->messages, 0, 3);

    return $this->render('hello/index.html.twig', [
        'messages' => $messages,
    ]);
}
~~~~~~~
> Bu veri array'ini direkt olarak şablona geçirilir.

#### Twig'de Döngüler
+ Twig şablonları içinde bir array'i render etmek için `for` döngüsünü kullanılabilir. Aşağıda `for` döngüsünün örneği verilmiştir.
~~~~~~~
{% for message in messages %}
    <div>{{ message }}</div>
{% endfor %}
~~~~~~~
> `messages` array'inde her elemanı iterasyona alır ve her bir `message` elemanını bir `<div>` içinde render eder. Döngünün kapatılması unutulmamalıdır, aksi takdirde hata alınır.

#### Koşullu Render
+ Boş bir array durumunda, sayfanın tamamen boş olmaması için bazı mesajlar göstermek istenebilir. Bu durumları `if` yapısı ile kontrol edilebilir.
~~~~~~~
{% if messages|length > 0 %}
    {% for message in messages %}
        <div>{{ message }}</div>
    {% endfor %}
{% else %}
    <div>There's nothing here yet!div>
{% endif %}
~~~~~~~
> Eğer `messages` array'inin uzunluğu 0'dan büyükse array'i iterasyona alır ve her bir elemanı render eder. Aksi takdirde `There's nothing here yet!` mesajını gösterir.

***
### Twig Filters & Functions | Twig Filtreleri ve Fonksiyonları
+ Twig filtreleri ve fonksiyonları, Twig şablon motorunda verileri işlemeyi ve dönüştürmeyi kolaylaştırır.

#### Twig Filtreleri
+ Twig filtreleri, veri değişkenlerine uygulanan işlemlerdir. Bir filtre, pipe operatörü (`|`) kullanılarak bir veri değişkenine uygulanır ve bu işlem sonucunda yeni bir değer üretir. `Orijinal değeri değiştirmez, sadece yeni bir değer oluşturur.`
~~~~~~~
{% if messages|length > 0 %}
    {% for message in messages %}
        <div>{{ message }}</div>
    {% endfor %}
{% else %}
    <div>There's nothing here yet!</div>
{% endif %}
~~~~~~~
> + Burada `message` array'inin uzunluğunu kontrol edilir. `length` filtresi, array'in uzunluğunu döndürerek yeni bir sayısal değer oluşturur.
> + Filtreler genellikle argüman kabul ederler, ancak bazıları argümansız çalışır. [Tüm filtrelerin bir listesi Twig dokümantasyonunda bulunabilir.](https://twig.symfony.com/doc/3.x/filters/index.html#filters)

##### `slice` Filtresi Kullanımı
+ Controller içinde veri işleme işlemlerinden kaçınmak ve bunun yerine şablon içinde veri işlemi yapmak istediğinde, `array_slice` fonksiyonunu controller'da kullanmak yerine, Twig'in `slice` filtresini kullanılabilir.

    - Öncelikle controller'da sadece ham veri ve limit değişkeni şablona geçirilir.
    ###### HelloController.php
    ~~~~~~~
    public function index()
    {
        $messages = $this->messages; // Remove array_slice
        $limit = 3; // Add limit variable
    
        return $this->render('hello/index.html.twig', [
            'messages' => $messages,
            'limit' => $limit,
        ]);
    }
    ~~~~~~~

    - Sonrasında, Twig şablonunda `slice` filtresini kullanarak veri dilimlenir:
    ###### index.html.twig
    ~~~~~~~
    {% for message in messages|slice(0, limit) %}
        <div>{{ message }}</div>
    {% endfor %}
    ~~~~~~~
    > Bu örnekte `messages` array'i `slice` filtresi ile işlenir. `slice` filtresi, array'in  belirtilen başlangıç noktası ve limit kadar elemanını alır. `limit` değeri de controller'dan şablona geçirilen bir değişkendir.

#### Twig Fonksiyonları
+ Twig içinde fonksiyonlar da kullanabilir. Örneğin, tarih işlemleri için `date` fonksiyonu kullanışlıdır. Twig fonksiyonları ile verileri farklı formatlara dönüştürebilir, tarih işlemleri yapabilir ve diğer faydalı işlemleri gerçekleştirebilir.
    - `date` fonksiyonu kullanarak tarih formatlarını değiştirebilir:
    ~~~~~~~
    {{ "now"|date("Y-m-d") }}
    ~~~~~~~
    > Mevcut tarihi `Y-m-d` formatında görüntüler.
    
    - Bir başka örnek, belirli bir tarih formatında tarih oluşturmak:
    ~~~~~~~
    {{ date("now") }}
    ~~~~~~~
    > Geçerli tarihi `Y-m-d H:i:s` formatında oluşturur.

***
### Twig Functions - Including Partial Templates | Twig Fonksiyonları - Kısmi Şablonlar Dahil
+ `include` fonksiyonu, başka şablonların parçalarını mevcut Twig şablonunun içine dahil edilmesini sağlar.

#### Messages Array'ini Değiştirmek
+ `messages` array'inin her mesajın oluşturulma tarihini içerecek şekilde değiştirilsin.

###### İşte güncellenmiş array'in genel bir görünümü:
~~~~~~~
$messages = [
    [
        'message' => 'First message',
        'created' => '2021-05-15',
    ],
    [
        'message' => 'Second message',
        'created' => '2022-04-20',
    ],
    // Add as many messages as needed
];
~~~~~~~

#### Twig Şablonunu Güncellemek
+ Güncellenmiş `message` array'i ile şablon bozulur çünkü artık iç içe array'ler alınır. Bu yeni yapıyı işlemek için Twig şablonun güncellenmesi gerekir.

##### 1. Array Elemanlarına Erişmek
+ Twig’de array elemanlarına erişmek için nokta notasyonunu kullanabilir, bu da PHP kare parantez sözdiziminden daha kullanışlıdır.
~~~~~~~
{{ message.message }}
{{ message.created }}
~~~~~~~

##### 2. Mesajları Tarihleriyle Birlikte Görüntülemek
+ Mesajları oluşturulma tarihleriyle birlikte görüntülemek için şablon güncellenir. Ayrıca, tarihleri formatlamak ve karşılaştırmak için Twig’de date fonksiyonunu kullanarak koşullu render işlemi uygulanır.
~~~~~~~
{% for message in messages %}
    <div>
        <div>{{ message.message }}</div>
        <div style="color:gray;">
            {% if message.created|date("Y-m-d") < "now"|date_modify("-1 year") %}
                Older than 1 year
            {% else %}
                {{ message.created|date("d-m-Y") }}
            {% endif %}
        </div>
    </div>
{% endfor %}
~~~~~~~
> Bir mesajın oluşturulma tarihinin bir yıldan eski olup olmadığını kontrol edilir. Eğer öyleyse, `Older than 1 year` ifadesini görüntüler; aksi halde tarihi `gün-ay-yıl` formatında gösterir.

#### `include` Fonksiyonunu Kullanmak
+ Mantığı (örneğin tarih formatlama) birden fazla şablonda tekrarlamamak için `include` fonksiyonu kullanılabilir.

##### 1. Kısmi Şablon Oluşturmak
+ Aynı dizinde `_message.html.twig` adında yeni bir dosya oluşturulur.

###### _message.html.twig
~~~~~~~
<div style="color:gray;">
    {% if message.created|date("Y-m-d") < "now"|date_modify("-1 year") %}
        Older than 1 year
    {% else %}
        {{ message.created|date("d-m-Y") }}
    {% endif %}
</div>
~~~~~~~

##### 2. Kısmi Şablonu Dahil Etmek
+ Ana şablonda `include` fonksiyonunu kullanarak kısmi şablon dahil edilir.

###### base.html.twig
~~~~~~~
{% for message in messages %}
    <div>
        <div>{{ message.message }}</div>
        {{ include('hello/_message.html.twig', { message: message }) }}
    </div>
{% endfor %}
~~~~~~~
> Bu kurulum, tarih formatlama mantığını farklı şablonlarda tekrar kullanılmasına olanak tanır, tutarlılık sağlar ve kod tekrarını azaltır.

##### 3. Kısmi Şablona Veri Geçmek
+ Bir şablonu dahil ederken, ona veri geçirilebilir. Bu, kısmi şablonun daha esnek ve yeniden kullanılabilir olmasını sağlar.
~~~~~~~
{{ include('hello/_message.html.twig', { message: message }) }}
~~~~~~~
> Bu satır, `message` değişkenini `_message.html.twig` şablonuna aktarır.

***
### Generating Links to Routes | Rotalara Bağlantı Oluşturma
+ rota (route) isimlerini kullanarak daha sürdürülebilir ve esnek kodlar yazılmasını sağlar.

#### Basit Link Oluşturma
+ Mesajların listesini görüntülerken, her mesaj için ayrı bir sayfa oluşturmak istenebilir. İlk olarak, doğrudan bir `a` elementi eklenip yol belirtilsin.
~~~~~~~
{% for key, message in messages %}
    <div>
        <a href="/messages/{{ key }}">Show Message</a>
    </div>
{% endfor %}
~~~~~~~
> Bu yöntemle, her mesaj için doğru yolu oluşturabilir ancak bu yöntem doğrudan yolları (URLs) kullanır ve bu da uygulamanın ölçeklenebilirliği ve bakımı açısından zorluklara yol açabilir.

#### Symfony Console Kullanarak Rotaları İncelemek
+ Terminalden aşağıdaki komut ile mevcut rotalar incelenir ve bu mevcut rotaların isimlerini ve yolların öğrenmenilmesini sağlar.
~~~~~~~
symfony console debug:router
~~~~~~~

+ Uygulamadaki tüm rotalar ve onların isim listesi:
~~~~~~~
 -------------------------- -------- -------- ------ -----------------------------------
  Name                       Method   Scheme   Host   Path
 -------------------------- -------- -------- ------ -----------------------------------
  app_show_one               GET      ANY      ANY    /messages/{id}
  app_message_list           GET      ANY      ANY    /messages
 -------------------------- -------- -------- ------ -----------------------------------
~~~~~~~
> `app_show_one` rotasının `/messages/{id}` yolunda bir mesajı göstermek için kullanıldığını görülür.

#### Twig Path Fonksiyonu Kullanarak Dinamik Linkler Oluşturm
+ Doğrudan yol belirtmek yerine, Twig'in `path` fonksiyonunu kullanarak rota isimleri üzerinden dinamik yollar oluşturulabilir. Bu, rotaların değiştirilmesi durumunda kodu güncellemeyi kolaylaştırır.

+ Yukarıdaki `a` elementi aşağıdaki gibi güncellenir.
~~~~~~~
{% for key, message in messages %}
    <div>
        <a href="{{ path('app_show_one', {id: key}) }}">Show Message</a>
    </div>
{% endfor %}
~~~~~~~
> `path` fonksiyonu, `app_show_one` rotası için dinamik bir URL oluşturur ve `{id: key}` parametresiyle birlikte id'yi geçirir.

#### Daha Esnek ve Bakımı Kolay Kodlar
+ Uygulamanın ölçeklenebilirliği ve bakımı açısından büyük avantajlar sağlar. Rota yolları değiştiğinde, sadece rota tanımını güncellemek yeterli olacaktır, tüm şablon dosyalarda URL'leri tek tek değiştirmeye gerek kalmayacaktır.
+ `Symfony'de link oluştururken doğrudan URL'ler kullanmak yerine rota isimlerini ve Twig path fonksiyonunu kullanmak, daha esnek ve sürdürülebilir bir yaklaşım sağlar. Bu yöntemle, uygulamanın gelecekteki değişikliklere daha kolay uyum sağlamasını garantilenmiş olunur.`

***
### Symfony Maker (Generating Boring Code)
+ Symfony framework'ü, belirli kalıplara dayalı olarak geliştirme yapılmasını sağlar. Örneğin, bir denetleyici (controller) oluştururken her seferinde aynı adımları izlenir; class oluşturulur, abstractController class genişletirilir, rotaları tanımlanır vb.
+ Bu süreçler bazen zaman alıcı olabilir ve tekrarlayan işler haline gelebilir. Neyse ki Symfony, bu tür işleri otomatikleştirmek için kullanabilecek bir araç sunar: `Maker Bundle.`

##### Maker Bundle'ı Kurma
+ Öncelikle, Maker Bundle'ı Symfony uyglamaya eklenmesi gerekmektedir. Bu araç sadece geliştirme ortamında kullanılacağı için, `--dev` bayrağı ile yüklenmelidir. Terminali açıp aşağıdaki komut çalıştırılmalıdır.
~~~~~~~
composer require symfony/maker-bundle --dev
~~~~~~~

##### Kullanılabilir Komutları Görüntüleme
+ Maker Bundle ile neler yapılabileceğini görmek için aşağıdaki komutu çalıştırarak mevcut komutları listelenir.
~~~~~~~
symfony console list make
~~~~~~~
> `make` ile başlayan tüm komutları listeleyecektir. Bu komutlar arasında controllers, varlıklar (entities), formlar ve daha fazlasını oluşturmak için kullanılabilecek komutlar bulunur.

##### Bir Denetleyici (Controller) Oluşturma
+ Örneğin, yeni bir controller oluşturmak için aşağıdaki komut kullanabilir.
~~~~~~~
symfony console make:controller
~~~~~~~

+ Bu komutu çalıştırdıktan sonra, komut bazı sorular sorar ve verilen cevaplara göre istenilen denetleyiciyi oluşturulur. Örneğin bir `MicroPost` uygulaması oluşturulsun, yani bir tür Twitter klonu gibi. Bu yüzden yeni controller `MicroPostController` olarak adlandırılmalıdır.
+ `Symfony'de denetleyici adları tekil olarak adlandırılmalıdır (örneğin, PizzasController yerine PizzaController).`
+ Komut çalıştırıldıktan sonra gelen soruya `MicroPostController` yanıtı verilsin.
~~~~~~~
symfony console make:controller MicroPostController
~~~~~~~
> Bu işlem, `MicroPostController` adında bir controller class'ı ve ona bağlı bir şablon dosyası oluşturur.

##### Oluşturulan Kodları İnceleme
+ Terminali kapatıp oluşturulan dosyalara bakaldığında:

###### MicroPostController.php
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micro/post', name: 'app_micro_post')]
    public function index(): Response
    {
        return $this->render('micro_post/index.html.twig', [
            'controller_name' => 'MicroPostController',
        ]);
    }
}
~~~~~~~

###### micro_post/index.html.twig:
~~~~~~~
{% extends 'base.html.twig' %}

{% block title %}MicroPost{% endblock %}

{% block body %}
    <h1>Welcome to the MicroPost page!</h1>
{% endblock %}
~~~~~~~
> Symfony, controller'ın doğru namespace ve gerekli bileşenleri içerecek şekilde oluşturur. Ayrıca, bir rota (route) tanımlar ve ilgili şablon dosyasını oluşturur.

##### URL'i Test Etme
+ Oluşturulan URL'i ziyaret ederek sonucu kontrol edilir. Tarayıcıda `http://localhost/micro/post` adresini ziyaret edilerek sayfanın düzgün bir şekilde görüntülendiği doğrulanır.
+ Ardından, URL'i daha okunabilir hale getirmek için `/micro/post` yolu `/micro-post` olarak değiştirilir.

###### MicroPostController.php
~~~~~~~
#[Route('/micro-post', name: 'app_micro_post')]
~~~~~~~
> Yeniden tarayıcıdan `http://localhost/micro-post` adresi ziyaret edilerek değişiklik kontrol edilir.

***
### Symfony Profiler (Debugging Project)
+ Symfony Profiler, Symfony geliştirme sürecinde vazgeçilmez bir araçtır.
+ Profiler, uygulamanın performansı ve davranışı hakkında detaylı bilgi toplar. `Ancak bu araç hiçbir zaman canlı ortamlarda kullanılmamalıdır.` Bu, ciddi güvenlik açıklarına yol açabilir ve uygulamayı yavaşlatabilir. Bu nedenle, Profiler sadece geliştirme ortamında kurulmalıdır.

##### Profiler Kurulumu
+ Profiler'ı Symfony uygulamada eklemek için terminali açıp aşağıdaki komut çalıştırılmalıdır.
~~~~~~~
composer require symfony/profiler-pack --dev
~~~~~~~
> Bu komut, Profiler paketini geliştirme ortamına ekler. Symfony'nin varsayılan ayarları, Profiler'ın sorunsuz çalışmasını sağlar. Kurulum tamamlandıktan sonra, uygulamanın yeniden başlatılması gerekebilir.

##### Profiler'ın Kullanımı
+ Profiler kurulduktan sonra, uygulama yerel sunucuda çalıştırılır ve bir sayfa yenilenir. `Sayfanın alt kısmında Profiler bar'ı görünmelidir.`

##### Profiler Bar'ı İnceleme
+ Profiler bar'ı, aşağıdaki bilgileri gösterir:
 1. `HTTP Durum Kodu:` Bu, yapılan isteğin HTTP durum kodunu gösterir.
 2. `Rota ve Denetleyici Bilgisi:` Hangi rota ve denetleyici işleminin çalıştığını gösterir.
 3. `Performans Bilgileri:` Sayfanın ne kadar sürede oluşturulduğunu ve bu sürecin detaylarını gösterir.
 4. `Bellek Kullanımı:` İsteğin ne kadar bellek kullandığını gösterir.
 5. `Şablon Bilgileri:` Hangi şablonların kullanıldığını ve kaç kez kullanıldığını gösterir.
 6. `Sunucu Bilgileri:` PHP sürümü, yüklü uzantılar vb. bilgileri gösterir.
 7. `Uygulama Bilgileri:` Hangi ortamda çalışıldığını, hata ayıklamanın açık olup olmadığını gösterir.

##### Profiler Detaylarına Bakma
+ Profiler bar'ındaki herhangi bir öğeye tıklayarak detaylı bilgiye ulaşabilir. Örneğin:
  - `Request/Response Bilgileri:` GET ve POST parametreleri, yüklenen dosyalar, başlıklar vb. bilgileri içerir.
  - `Performans Bilgileri:` Hangi işlemlerin ne kadar süre aldığını zaman çizelgesiyle gösterir.
  - `Rota Bilgileri:` Hangi rotanın eşleştiğini ve diğer rotaların nasıl değerlendirildiğini gösterir.
  - `Twig Şablonları:` Hangi şablonların kullanıldığını, kaç kez kullanıldığını gösterir.
  - `Konfigürasyon Bilgileri:` Symfony uygulamanın konfigürasyon bilgilerini gösterir.

##### Geçmiş İstekleri İnceleme
+ Profiler, önceki isteklerin bilgilerini de saklar. Sağ üst köşedeki "Last 10" bölümüne tıklayarak son yapılan isteklerin listesini görebilir ve her bir isteğin detaylarına bakılabilir.

##### Profiler'ı Kapatma
+ Profiler'ı kapatmak için URL'den `_profiler` kısmını kaldırabilir veya bar'ın sağ alt köşesindeki düğmeye tıklayarak normal sayfaya dönülebilir.
