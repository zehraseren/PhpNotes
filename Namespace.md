### Namespace (ad alanı) nedir?
***
+ Büyük projelerde kodun düzenlenmesi ve isim çakışmalarının önlenmesi amacıyla kullanılır.
+ PHP 5.3 ile tanıtılan namespace'ler, class'ları, inrerface'leri, fonksiyonları ve sabitleri mantıksal gruplara ayrılmasına olanak tanır. Böylece `özellikle aynı isimde birden fazla class'ın veya fonksiyonun bulunduğu büyük projelerde oldukça faydalıdır.`

#### Namespace Tanımı ve Kullanımı
+ Namespace, `namespace` keyword'ü ile tanımlanır ve genellikle dosyanın en üstünde yer alır.

##### Basit Namespace Örneği
###### MyProject.php
~~~~~~~
<?php
namespace MyProject;

class MyClass {
    public function myMethod() {
        return "Hello from MyClass!";
    }
}

function myFunction() {
    return "Hello from myFunction!";
}

const MY_CONSTANT = 'Hello from MY_CONSTANT!';
?>
~~~~~~~
> Yukarıdaki örnekte, `MyProject` namespace'inde bir class (`MyClass`), bir fonksiyon (`myFunction`) ve bir sabit (`MY_CONSTANT`) tanımlanmıştır.

#### Namespace Kullanımı
+ Namespace tanımladıktan sonra, ilgili class, fonksiyon veya sabite erişmek için tam adını (fully qualified name) kullanmalıdır.

##### Namespace İçindeki Öğelere Erişim
~~~~~~~
<?php
require 'MyProject.php';

use MyProject\MyClass;
use function MyProject\myFunction;
use const MyProject\MY_CONSTANT;

$object = new MyClass();
echo $object->myMethod();

echo myFunction();

echo MY_CONSTANT;
?>
~~~~~~~
> Output: Hello from MyClass!

> Output: Hello from myFunction!

> Output: Hello from MY_CONSTANT!

#### Alt Namespace'ler
+ Namespace'ler alt namespace'lere de sahip olabilir. Bu, daha karmaşık yapılar oluşturmanıza olanak tanır.

###### SubNamespace.php
~~~~~~~
<?php
namespace MyProject\SubNamespace;

class MyClass {
    public function myMethod() {
        return "Hello from SubNamespace MyClass!";
    }
}
?>
~~~~~~~

##### Alt Namespace'lerin Kullanımı
~~~~~~~
<?php
require 'SubNamespace.php';

use MyProject\SubNamespace\MyClass;

$object = new MyClass();
echo $object->myMethod();
?>
~~~~~~~
> Output: Hello from SubNamespace MyClass!

#### Namespace Kullanımının Avantajları
+ `İsim Çakışmalarını Önler:` Farklı kütüphanelerde aynı isimde class veya fonksiyon kullanıldığında çakışma olmaz.
+ `Kodun Düzenlenmesi ve Yönetimi:` Kodun mantıksal modüllere ayrılması, okunabilirliği ve yönetimi kolaylaştırır.
+ `Autoloading ile Entegrasyon:` PSR-4 gibi standartlar, namespace'lerin autoloading ile kolayca yüklenmesini sağlar.

#### PHP'de Namespace ile İlgili Önemli Notlar
+ `Namespace Tanımı:` Namespace tanımı dosyanın en üstünde olmalıdır, önce PHP açılış etiketi ( `<?php`) ve olası declare ifadesi dışında başka bir şey olmamalıdır.
+ `Backslash Kullanımı:` Küresel (global) namespace içindeki öğelere erişirken backslash (`\`) kullanılmalıdır. Örneğin, `\DateTime` gibi.
+ `use Keyword:` Namespace içindeki öğeleri daha kısa isimlerle kullanmak için `use` keyword ile kullanılır.

##### Global Namespace Kullanımı
~~~~~~~
<?php
namespace MyProject;

class DateTime {
    public function getDate() {
        return "MyProject DateTime";
    }
}

// Using the Global PHP DateTime class
$globalDateTime = new \DateTime();
echo $globalDateTime->format('Y-m-d H:i:s');

// Using the DateTime class in MyProject
$myDateTime = new DateTime();
echo $myDateTime->getDate();
?>
~~~~~~~
