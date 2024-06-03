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
~~~~~~~
~~~~~~~

##### Composer Kullanarak Autoloading

###### 1. `Composer Kurulumu:` Öncelikle Composer'ı kurulur.
~~~~~~~https://www.youtube.com/
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

> Autoloading, PHP projelerinin yönetimini kolaylaştırır ve kodun daha modüler ve bakımı kolay olmasını sağlar. Özellikle Composer ile birlikte kullanıldığında, güçlü ve esnek bir autoloading yapısı elde edilir.

***
### PHP'de Coding Standards nedir?

***
### PSR (PHP Standard Recommendation) nedir?

***
### Composer & dependency management (bağımlılık yönetimi ve proje yapılandırması) nedir?

***
### Autoloading using composer (Composer kullanarak autoloading) 
