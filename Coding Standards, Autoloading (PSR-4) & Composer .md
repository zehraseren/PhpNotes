### Autoloading nedir?
+ Class dosyalarının ihtiyaç duyulduğunda otomatik olarak yüklenmesini sağlayan bir mekanizmadır.
+ Geliştiricilerin manuel olarak `require` veya `include` komutlarını kullanarak dosya yükleme zahmetinden kurtarır ve kodun daha temiz ve bakımı daha kolay hale gelmesini sağlar.
+ Autoloading, özellikle büyük projelerde ve modern PHP uygulamalarında oldukça yaygındır.

#### Autoloading'in Temel Prensibi
+ Bir class, interface veya trait kullanıldığında, PHP'nin otomatik olarak ilgili dosyayı bulup yüklemesini sağlar. Bu, `class'ın ilk kullanıldığı anda gerçekleşir.`
+ Autoloading için PHP'de kullanılan bazı mekanizmalar şunlardır:
  - `__autoload Fonksiyonu:` Eski ve artık tavsiye edilmeyen bir yöntemdir.
  - `spl_autoload_register Fonksiyonu:` Daha esnek ve modern bir yaklaşımdır.
  - `Composer:` PHP'nin en popüler paket yönetim sistemi olan Composer, autoloading'i kolaylaştıran bir araçtır.

1. `__autoload` Fonksiyonu
+ PHP 5'te tanıtılmıştır ancak artık kullanılmaması önerilmektedir. Çünkü `spl_autoload_register` fonksiyonuna kıyasla daha sınırlıdır.
~~~~~~~
<?php
function __autoload($className) {
    require_once $className . '.php';
}

$myClass = new MyClass();
?>
~~~~~~~

2. `spl_autoload_register` Fonksiyonu
+ Autoloading işlemi için daha esnek ve güçlü bir yöntem sunar. Birden fazla autoloader fonksiyonu kaydedilmesine olanak tanır.
~~~~~~~
<?php
spl_autoload_register(function ($className) {
    $file = __DIR__ . '/' . str_replace('\\', '/', $className) . '.php';
    if (file_exists($file)) {
        require_once $file;
    }
});

// Kullanım
$myClass = new MyClass();
~~~~~~~
> + spl_autoload_register:
>   - `spl_autoload_register` fonksiyonu, class'ları otomatik olarak yüklemek için bir autoloader fonksiyonu kaydeder.
>   - Bu fonksiyon, bir class ismi verildiğinde çağrılır ve sınıfın dosyasını bulup yükler.
> + Anonim fonksiyon, autoloader olarak kaydedilir. Bu fonksiyon, yüklenmesi gereken class'ın (`$className`) adını alır.
> + Dosya Yolunu Belirleme:
>   - `__DIR__`, bu betiğin bulunduğu dizinin tam yolunu verir.
>   - `str_replace('\\', '/', $className)`, class adındaki ters eğik çizgileri (`\`) normal eğik çizgilere (`/`) çevirir. Bu, class adlarının dizin yollarına uygun hale getirilmesini sağlar.
>   - Class adının sonuna `.php` eklenir, böylece dosya yolu oluşturulur. Örneğin, `MyClass` class'ı için bu yol `__DIR__/MyClass.php` olacaktır.
> + Dosyanın Varlığını Kontrol Etme ve Yükleme:
>   - `file_exists($file)`, belirtilen dosyanın var olup olmadığını kontrol eder.
>   - Dosya varsa, `require_once $file` ile dosya yüklenir. `require_once, dosyanın yalnızca bir kez yüklenmesini sağlar.`
> + Class'ın Kullanılması:
>   - `MyClass` class'ından bir nesne oluşturulmak istendiğinde, PHP otomatik olarak yukarıda tanımlanan autoload fonksiyonunu çağırır.
>   - Autoload fonksiyonu, `MyClass` class'ı için `__DIR__/MyClass.php` dosyasını arar ve yükler.
>   - Eğer `MyClass.php` dosyası belirtilen dizinde mevcutsa, dosya yüklenir ve class tanımı kullanıma hazır hale gelir.

3. Composer ile Autoloading
+ Composer, PHP'nin popüler bir paket yönetim sistemidir ve autoloading için PSR-4 standartlarını kullanarak yapılandırmayı otomatik hale getirir.

##### Composer Kullanarak Autoloading

###### 1. `Composer Kurulumu:` Öncelikle Composer'ı kurulur.
~~~~~~~
composer init
~~~~~~~

###### 2. `composer.json Dosyasını Yapılandırma:` Proje kök dizininde composer.json dosyası oluşturulur ve autoload bölümü eklenir.
~~~~~~~
{
    "autoload": {
        "psr-4": {
            "MyNamespace\\": "src/"
        }
    }
}
~~~~~~~

###### 3. `Composer Autoload Oluşturma:` Autoload dosyasını oluşturmak için aşağıdaki komutu çalıştırılır.
~~~~~~~
composer dump-autoload
~~~~~~~

###### 4. `Kullanım:` Projenin giriş dosyasına `vendor/autoload.php` dosyası dahil edilmelidir..
~~~~~~~
<?php
require 'vendor/autoload.php';

use MyNamespace\MyClass;

$myClass = new MyClass();
~~~~~~~
#### PSR-4 Standardı
+ PSR-4, PHP-FIG (PHP Framework Interoperability Group | Framework Birlikte Çalışabilirlik Grubu) tarafından belirlenen bir autoloading standardıdır.
+ PSR-4 standardı, class isimlerini dosya yapısıyla eşleştirir. Örneğin:
  - `Namespace:` MyNamespace\SubNamespace
  - `Class Adı:` MyClass
  - `Dosya Yolu:` src/SubNamespace/MyClass.php
~~~~~~~
<?php
// src/MyNamespace/MyClass.php
namespace MyNamespace;

class MyClass {
    public function sayHello() {
        echo "Hello, World!";
    }
}
~~~~~~~

~~~~~~~
<?php
// index.php
require 'vendor/autoload.php';

use MyNamespace\MyClass;

$myClass = new MyClass();
$myClass->sayHello();
~~~~~~~
> Output: Hello, World!

#### Composer Autoloading'in Avantajları
+ `Kolaylık ve Verimlilik:` Dosyaları manuel olarak yükleme ihtiyacını ortadan kaldırır.
+ `Standartlara Uygunluk:` PSR-4 gibi standartlarla uyumlu çalışır.
+ `Hata Azaltma:` Yanlış dosya yüklemeleri veya isim çakışmaları gibi hataları azaltır.
+ `Kolay Yönetim:` Bağımlılıkları ve sınıfları merkezi bir yerden yönetir.

> Autoloading, PHP projelerinin yönetimini kolaylaştırır ve kodun daha modüler ve bakımı kolay olmasını sağlar. Özellikle Composer ile birlikte kullanıldığında, güçlü ve esnek bir autoloading yapısı elde edilir.

***
### PSR (PHP Standard Recommendation) nedir?
+ PHP'de PSR (PHP Standard Recommendation | PHP Standart Önerisi), PHP-FIG (PHP Framework Interoperability Group) tarafından oluşturulan ve PHP projelerinde standartları belirleyen bir dizi öneridir.
+ PSR'ler, PHP projelerinin uyumlu ve tutarlı olmasını sağlamaya yardımcı olur, böylece farklı projeler ve kütüphaneler arasında daha kolay entegrasyon ve işbirliği sağlanır.
+ PSR'ler, kod yazma standartlarından autoloading kurallarına kadar geniş bir yelpazede standartlar sunar.

#### Önemli PSR Standartları
1. PSR-1: Basic Coding Standard
+ Temel kodlama standartlarını tanımlar ve kodun okunabilirliğini artırmayı amaçlar.
  - `Dosya biçimi:` PHP dosyaları ya yalnızca PHP kodu içermeli ya da PHP ve HTML kodunu birlikte içerebilir.
  - `Adlandırma kuralları:` Class isimleri, büyük harfle başlamalıdır.
  - `Dosya isimlendirme:` Bir dosya, yalnızca bir class, interface veya trait tanımlamalıdır.

2. PSR-2: Coding Style Guide
+ PSR-1 üzerine inşa edilir ve daha ayrıntılı kodlama stil kuralları sunar.
   - `Indentation:` 4 boşluk kullanılmalı, sekme (tab) kullanılmamalıdır.
   - `Line Length:` Satır uzunluğu 120 karakteri geçmemelidir.
   - `Namespace ve Use Declarations:` Namespace deklarasyonları dosyanın en üstünde olmalı, ardından boş bir satır bırakılmalı ve use deklarasyonları gelmelidir.
   - `Control Structures:` If, elseif, else, switch ve diğer kontrol yapılarının kullanımı belirli kurallara tabi olmalıdır.
~~~~~~~
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;

class Foo implements FooInterface
{
    public function sampleFunction($arg1, $arg2 = null)
    {
        if ($arg1 === $arg2) {
            throw new \Exception('Arguments cannot be equal');
        }

        return true;
    }
}
?>
~~~~~~~

3. PSR-3: Logger Interface
+ PHP'deki loglama işlemleri için bir interface standardı tanımlar. Loglama sistemleri için bir standart API sağlar.
~~~~~~~
<?php
use Psr\Log\LoggerInterface;

class MyLogger implements LoggerInterface
{
    // All necessary methods must be defined here
}
?>
~~~~~~~

4. PSR-4: Autoloading Standard
+ Class'ların otomatik olarak yüklenmesi için bir standart sağlar. Bu, dosya ve class isimlendirmesi için belirli kuralları içerir ve Composer gibi araçlarla yaygın olarak kullanılır.
   - `Namespace:` Vendor\Package
   - `Dosya Yolu:` src/Vendor/Package/ClassName.php
~~~~~~~
{
    "autoload": {
        "psr-4": {
            "Vendor\\Package\\": "src/"
        }
    }
}
~~~~~~~

~~~~~~~
<?php
// src/Vendor/Package/ClassName.php
namespace Vendor\Package;

class ClassName
{
    // Class content
}
?>
~~~~~~~

5. PSR-7: HTTP Message Interface
+ HTTP mesajlarının (istek ve yanıtlar) nasıl temsil edilmesi gerektiğini tanımlar. Bu, web uygulamaları ve API'ler için standart bir iletişim formatı sağlar.
~~~~~~~
<?php
use Psr\Http\Message\RequestInterface;
use Psr\Http\Message\ResponseInterface;

class MyController
{
    public function handle(RequestInterface $request): ResponseInterface
    {
        // Request and response operations are performed here
    }
}
?>
~~~~~~~

#### Diğer Önemli PSR'ler
+ `PSR-6:` Caching Interface
+ `PSR-11:` Container Interface
+ `PSR-12:` Extended Coding Style Guide (PSR-2'nin genişletilmiş versiyonu)

#### PSR'lerin Önemi
+ `Uyumluluk:` Farklı projeler ve kütüphaneler arasında uyumluluğu artırır.
+ `Kod Kalitesi:` Standartlar, kodun kalitesini ve okunabilirliğini artırır.
+ `İşbirliği:` Takımlar arası işbirliğini kolaylaştırır ve kodun anlaşılmasını sağlar.
+ `Bakım Kolaylığı:` Standartlara uygun yazılmış kodlar daha kolay bakımı yapılabilir.

> PSR'ler, PHP projelerinde standartları belirleyen ve uyumluluğu artıran önerilerdir. Kodlama standartlarından autoloading'e, loglama arayüzlerinden HTTP mesajlarına kadar geniş bir yelpazede kurallar sunar. Bu standartlar, PHP geliştiricilerinin daha tutarlı, okunabilir ve bakımı kolay kodlar yazmasına yardımcı olur. PSR'leri takip ederek, projelerinizde yüksek kalite ve uyumluluk sağlayabilirsiniz.

***
### Composer & dependency management (bağımlılık yönetimi ve proje yapılandırması) nedir?
+ PHP'de Composer, bağımlılık yönetimi ve proje yapılandırması için kullanılan güçlü bir araçtır.
+ Composer, projelerin ihtiyaç duyduğu kütüphaneleri ve bu kütüphanelerin bağımlılıklarını kolayca yönetilmesini sağlar. Composer ayrıca otomatik yükleme (autoloading) için de standart bir yöntem sunar.
+ Composer, bağımlılıkların sürümlerini yönetir ve gerektiğinde güncellemeleri gerçekleştirir.

#### Composer Kurulumu
1. İndirme ve Yükleme:
~~~~~~~
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
~~~~~~~
> Bu komutlar, Composer'ı indirir ve çalıştırılabilir bir dosya oluşturur.

2. Composer'ı Küresel Olarak Kurma:
~~~~~~~
mv composer.phar /usr/local/bin/composer
~~~~~~~
> Bu komut, Composer'ı küresel olarak erişilebilir hale getirir.

#### Bağımlılık Güncelleme
+ Bağımlılıkları güncellemek için `composer update` komutunu kullanılır. Bu komut, `composer.json` dosyasındaki bağımlılıkları en son sürümlerine günceller ve `composer.lock` dosyasını yeniden oluşturur.

#### Composer ile Sık Kullanılan Komutlar
+ `composer install:` composer.lock dosyasına göre bağımlılıkları yükler.
+ `composer update:` composer.json dosyasına göre bağımlılıkları günceller.
+ `composer require:` Yeni bir bağımlılık ekler.
+ `composer remove:` Var olan bir bağımlılığı kaldırır.
+ `composer dump-autoload:` Autoload dosyasını yeniden oluşturur.

#### Composer'ın Avantajları
+ `Kolay Bağımlılık Yönetimi:` Tüm bağımlılıkları ve sürümlerini kolayca yönetir.
+ `Otomatik Yükleme:` Class'ların otomatik olarak yüklenmesini sağlar.
+ `Versiyon Yönetimi:` Belirli sürümleri kullanarak projede tutarlılığı korur.
+ `Geniş Paket Desteği:` Packagist üzerinden binlerce paket ve kütüphane desteği sunar.

> Composer, PHP projelerinde bağımlılık yönetimini ve otomatik yükleme işlemlerini kolaylaştıran güçlü bir araçtır. Projelerin ihtiyaç duyduğu kütüphaneleri ve bağımlılıkları yönetmek, sürümleri kontrol etmek ve class'ları otomatik olarak yüklemek için Composer'ı kullanabilir. Composer, büyük ve karmaşık projelerde kodun düzenlenmesini ve bakımını kolaylaştırır, bu da geliştirme sürecini daha verimli hale getirir.
