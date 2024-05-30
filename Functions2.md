***
### Variable functions (değişken fonksiyonlar) nedir?
***
+ Bir değişkenin değerinin bir fonksiyon adı olduğu ve bu değişken kullanılarak fonksiyonun çağrılabildiği bir özelliktir.
+ Değişken fonksiyonlar, dinamik fonksiyon çağrıları yapmayı mümkün kılar ve fonksiyonların adlarının değişkenlerle belirtilebilmesini sağlar.

#### Değişken Fonksiyonların Kullanımı
+ Bir değişkenin değeri bir fonksiyon adı olduğunda, bu değişkenin sonuna parantez ekleyerek o fonksiyonu çağrılabilir.

1. Temel Kullanım
~~~~~~~
function hello() {
    echo "Hello, World!";
}

$func = "hello";
$func(); 
~~~~~~~
> Output: Hello, World!
> + `$fonksiyon` değişkeninin değeri `hello` fonksiyonunun adı olarak belirlenmiştir.
> + `$func()` ifadesi, `hello` fonksiyonunu çağırır.

2. Parametrelerle Kullanım
~~~~~~~
function salute($name) {
    echo "Hello, $name!";
}

$func = "salute";
$func("Zehra");
~~~~~~~
> Output: Hello, Zehra!
> + `salute` fonksiyonuna parametre geçerek çağırılmıştır.

3. Class Yöntemleriyle Kullanım
~~~~~~~
class Greeter {
    public function greeting($name) {
        echo "Hello, $name!";
    }
}

$object = new Greeter();
$method = "greeting";
$object->$method("Zehra");
~~~~~~~
> Output: Hello, Zehra!
> + `$method` değişkeni bir class'ın yöntemini belirtir ve `$object->$method("Zehra");` ifadesi `Greeter` class'ının `greeting` yöntemini çağırır.

#### Değişken Fonksiyonların Kullanım Alanları
+ Dinamik fonksiyon çağrılarının gerektiği durumlarda kullanışlıdır.

1. Fonksiyon Adlarını Dinamik Olarak Belirlemek
+ Özellikle kullanıcı girdilerine veya belirli koşullara göre farklı fonksiyonların çağrılması gerektiğinde kullanılır.
~~~~~~~
function sum($x, $y) {
    return $x + $y;
}

function ext($x, $y) {
    return $x - $y;
}

$operation = "sum"; // This value can be set dynamically.
echo $operation(10, 5);
~~~~~~~
> Output: 15

2. Callback Fonksiyonlar ve Yönlendirme
+ Callback (geri çağırım) fonksiyonları veya yönlendirme sistemlerinde kullanılır.
~~~~~~~
$callback = function() {
    echo "This is a callback function.";
};

function run($fn) {
    $fn();
}

run($callback);
~~~~~~~
> Output: This is a callback function.

#### Potansiyel Sorunlar ve Dikkat Edilmesi Gerekenler
+ `Güvenlik:` Değişken fonksiyon adları kullanıcı girdilerinden türetiliyorsa, kötü niyetli fonksiyon çağrılarını engellemek için dikkatli olmak gerekir. Kullanıcı girdilerini doğrulamak ve sanitize etmek önemlidir.
+ `Hata Ayıklama:` Değişken fonksiyonlar kullanıldığında, fonksiyon adlarının dinamik olması hata ayıklamayı zorlaştırabilir. Kodun doğru çalıştığından emin olmak için kapsamlı testler yapılmalıdır.

> Değişken fonksiyonlar, PHP'de fonksiyon adlarını dinamik olarak belirlemenin ve çağırmanın esnek bir yolunu sunar. Bu özellik, kodunuzu daha dinamik ve uyarlanabilir hale getirir, ancak güvenlik ve hata ayıklama konularına dikkat edilmesi gerekir.

***
### Anonymous functions (anonim fonksiyonlar) nedir?
***
+

#### Anonim Fonksiyonların Temel Kullanımı
+
1. 
~~~~~~~
~~~~~~~
> Output:
> +

2. 
~~~~~~~
~~~~~~~
> Output:
> +

3. 
~~~~~~~
~~~~~~~
> Output:
> +

#### Closure Class
+
~~~~~~~
~~~~~~~
> +

#### Kullanım Alanları
1. Callback Fonksiyonlar
+
~~~~~~~
~~~~~~~
> Output:

2. Dinamik İşlemler
+
~~~~~~~
~~~~~~~
> Output:

3. Geçici İşlevler
+

#### Güvenlik ve Performans
+ 

> 

***
### Closures & accessing variables from the parent scope (kapatma ve ana kapsamdaki değişkenlere erişim) nedir?
***

***
### Callable data type & callback functions (çağrılabilir data tipleri ve geri çağrılan fonksiyonlar) nedir?
***

***
### Closure vs callable (kapatılabilir vs çağrılabilir) nedir?
***

***
### Arrow functions nedir?
***
