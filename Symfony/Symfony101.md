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
4. `Yönlendirme:` İçsel olarak, Symfony uygulaması her gelen isteği nasıl ele alacağını belirlemek için yönlendirme adı verilen bir mekanizma kullanır. Yönlendirme, isteğin URL'sini belirli bir denetleyici eylemine eşler.
5. `Denetleyici:` Yönlendirme sistemi, istek için uygun denetleyici eylemini belirlediğinde, denetleyici, isteği işlemek ve bir yanıt oluşturmakla sorumludur. Denetleyiciler, uygulamanın ana operatörleri olarak hizmet verir ve veritabanından veri almak, form gönderimlerini işlemek ve HTML çıktısı oluşturmak için şablonları işleyebilirler.
6. `Yanıt Oluşturma:` Denetleyici, HTML, JSON veya XML gibi çeşitli biçimlerde olabilen bir yanıt oluşturur. Yanıt, uygulama tarafından oluşturulan dinamik içeriği, yanıt başlıklarını ve çerezleri içerebilir.
7. `HTTP Sunucusu Yanıtı:` Oluşturulan yanıt, HTTP sunucusuna geri gönderilir, ardından kullanıcının web tarayıcısına iletilir. Yanıt, istenen içeriğin yanı sıra herhangi ek HTTP başlıkları, çerezler veya durum kodları içerebilir.

***
### Controllers - Returning a Response | Denetleyici - Yanıt Döndürme 
+ Symfony uygulamasında her isteğin controllers tarafından işlenir.
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
+ Controller dosyası için namespace `App\Controller` olmalıdır. Controller dosyasında namespace'i tanımlanır:
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
+ Controller'in çalışması için routing yapılandırmak gerekir. Routing, hangi URL'nin hangi controller eylemine yönlendirileceğini belirler. Bu yapılandırma `config/routes.yaml` dosyasında yapılır.
~~~~~~~
hello:
    path: /
    controller: App\Controller\HelloController::index
~~~~~~~

##### 3. Projeyi Çalıştırma
+ Değişiklik kaydedilip tarayıcıda ana sayfa refresh edilince ekranda `Hello` metnini gözükür.
  - PHP class'ı dosya adı class adı ile aynı olmalıdır.
  - Bir dosyada birden fazla class tanımlanmamalıdır.
  - PHP'nin en son sürümlerinde, her fonksiyon veya method'un dönüş türü belirtilmelidir.
  
###### Mevcut Yönlendirmeleri Görüntüleme
> Uygulamanda tanımlı mevcut yönlendirmeleri ve hangi controller'in çağrıldığını görmek için `symfony console debug:router` komutunu çalıştırılır. Bu komut, uygulamada tanımlı tüm yönlendirmeleri ve controller'ı listeler:

***
### Route Parameter Requirements / Optional Parameters | Rota Parametre Gereksinimleri / İsteğe Bağlı Parametreler
+
~~~~~~~
~~~~~~~

***

### Twig Templates | Twig Şablonları
+
~~~~~~~
~~~~~~~

***
### Twig Template Inheritance | Twig Şablonları Kalıtımı
+
~~~~~~~
~~~~~~~

***
### Twig Control Structure (if/for) | Twig Kontrol Yapıları
+
~~~~~~~
~~~~~~~

***
### Twig Filters & Functions | Twig Filtreleri ve Fonksiyonları
+
~~~~~~~
~~~~~~~

***
### Twig Functions - Including Partial Templates | Twig Fonksiyonları - Kısmi Şablonlar Dahil
+
~~~~~~~
~~~~~~~

***
### Generating Links to Routes | Rotalara Bağlantı Oluşturma
+
~~~~~~~
~~~~~~~

***
### Symfony Maker (Generating Boring Code)
+
~~~~~~~
~~~~~~~

***
### Symfony Profiler (Debugging Project)
+
~~~~~~~
~~~~~~~


###
+
~~~~~~~
~~~~~~~

***
