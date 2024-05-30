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
+ Adı olmayan ve doğrudan bir değişkene atanarak kullanılabilen fonksiyonlardır.
+ PHP'de anonim fonksiyonlar, function anahtar kelimesi kullanılarak tanımlanır ve genellikle callable veya closure olarak adlandırılır. `Bu fonksiyonlar, diğer fonksiyonlara argüman olarak geçirilebilir veya bir değişken olarak saklanabilir.`

#### Anonim Fonksiyonların Temel Kullanımı
+ Anonim fonksiyonlar, normal bir fonksiyon gibi tanımlanır ancak adları yoktur. Bir değişkene atanarak çağrılabilirler.

1. Temel Anonim Fonksiyon Tanımlama ve Kullanma
~~~~~~~
$greeting = function($name) {
    return "Hello, $name!";
};

echo $greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra!
> + `$greeting` değişkenine anonim bir fonksiyon atanmıştır. Bu fonksiyon, `$greeting` değişkeni kullanılarak çağrılır.

2. Anonim Fonksiyonu Argüman Olarak Kullanma
~~~~~~~
function funcConstructor($func, $value) {
    return $func($value);
}

$square = function($num) {
    return $num * $num;
};

echo funcConstructor($square, 4);
~~~~~~~
> Output: 16
> + `funcConstructor` fonksiyonu, bir anonim fonksiyonu argüman olarak alır ve bu fonksiyonu kullanarak işlem yapar.

3. Kapsam Kullanımı (use keyword ile)
+ Anonim fonksiyonlar kendi kapsamları dışında tanımlanmış değişkenlere doğrudan erişemezler. Ancak `use` keyword'ü ile bu değişkenleri fonksiyon içine dahil edebilirler.
~~~~~~~
$message = "World";

$greeting = function($name) use ($message) {
    return "Hello, $name! How are you $message?";
};

echo $greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra! How are you World?
> + `$message` değişkeni `use` keyword'ü ile anonim fonksiyonun kapsamına dahil edilmiştir.

+ Dışarıdaki değişkenleri anonim fonksiyonlar içinde kullanarak daha dinamik ve esnek kod yazmaya olanak tanır. Dış değişkenler fonksiyon içine dahil edilerek, fonksiyonun dışarıdaki duruma göre farklı sonuçlar üretmesi sağlanır.
+ `use` keyword, `özellikle kapanış (closure) fonksiyonlar` içinde dış değişkenlere erişmek istediğinizde kullanışlıdır. Böylece, fonksiyonun tanımlandığı yerdeki değişkenleri fonksiyon içinde kullanabilirsiniz. Bu mekanizma, fonksiyonel programlama paradigmalarına uygun bir şekilde, fonksiyonlarınızı daha esnek ve yeniden kullanılabilir hale getirir.

#### Closure Class
+ Anonim fonksiyonlar, PHP'de `Closure Class` olarak adlandırılan bir class'ın örnekleridir. `Closure class, anonim fonksiyonların daha fazla kontrol edilmesini sağlar.`
~~~~~~~
$greeting = function($name) {
    return "Hello, $name!";
};

if ($greeting instanceof Closure) {
    echo "This is a Closure object.";
}
~~~~~~~
> + `$greeting` anonim fonksiyonunun bir Closure nesnesi olup olmadığı kontrol edilir.

#### Kullanım Alanları
1. Callback Fonksiyonlar
+ Anonim fonksiyonlar, özellikle callback fonksiyonları olarak kullanışlıdır.
~~~~~~~
$numbers = [1, 2, 3, 4, 5];

$squared = array_map(function($num) {
    return $num * $num;
}, $numbers);

print_r($squared);
~~~~~~~
> Output: Array ( [0] => 1 [1] => 4 [2] => 9 [3] => 16 [4] => 25 )
> + `array_map()`, bir array üzerinde işlem yapmak ve her eleman için belirli bir işlemi uygulayarak yeni bir array oluşturmak için kullanışlıdır. Anonim fonksiyonlar, bu tür işlemlerde esneklik sağlar ve kodun okunabilirliğini artırır.

2. Dinamik İşlemler
+ Dinamik olarak oluşturulan işlemler veya koşullara bağlı olarak değişken fonksiyonel davranışlar için kullanılabilir.
~~~~~~~
$operation = "+";
$calculate = function($x, $y) use ($operation) {
    switch ($operation) {
        case "+": return $x + $y;
        case "-": return $x - $y;
        default: return 0;
    }
};
echo $calculate(10, 5);
~~~~~~~
> Output: 15

3. Geçici İşlevler
+ Sadece belirli bir işlem için geçici olarak tanımlanması gereken işlevlerde kullanılır.

#### Güvenlik ve Performans
+ Anonim fonksiyonlar, PHP'de güçlü ve esnek bir araçtır, ancak kullanımları dikkat gerektirir. Özellikle, değişkenlerin kapsamını yönetirken dikkatli olunmalı ve bellek yönetimi konusuna dikkat edilmelidir.

> Anonim fonksiyonlar, adı olmayan ve değişken olarak saklanabilen fonksiyonlardır. Callback mekanizmaları, dinamik işlemler ve geçici işlevler gibi çeşitli alanlarda kullanılır. `use` keyword ile dış değişkenlere erişim sağlayarak kapsamlarını genişletebilirler. Anonim fonksiyonlar, PHP'de fonksiyonel programlama yaklaşımını destekleyen önemli bir özelliktir.

***
### Closures & accessing variables from the parent scope (kapanış ve ana kapsamdaki değişkenlere erişim) nedir?
***
+ `Closures:` PHP'de anonim fonksiyonların gelişmiş bir versiyonudur. Bir closure, fonksiyonun tanımlandığı bağlamdaki değişkenlere ve parametrelere erişim sağlayabilir. Anonim fonksiyonlar, closure'lar olarak da bilinir ve PHP'de closure class örnekleri olarak implement edilirler.
+ `Accessing Variables from the Parent Scope:` Closure'lar, `use` keyword ile tanımlandıkları bağlamdaki (parent scope) değişkenlere erişebilir. Bu, closure'ların dış değişkenleri kullanmasını ve bu değişkenleri fonksiyon içine dahil etmesini sağlar.

##### Örnekler

1. Temel Closure ve use Keyword
~~~~~~~
$message = "World";

$greeting = function($name) use ($message) {
    return "Hello, $name! How are you $message?";
};

echo $greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra! How are you World?
> + `$message` değişkeni `use` keyword ile closure'a dahil edilmiştir. Closure, `$message` değişkenini fonksiyon içinde kullanır.

2. Birden Fazla Değişkeni `use` ile Dahil Etme
~~~~~~~
$hi = "Hello";
$world = "World";

$salute = function($name) use ($hi, $world) {
    return "$hi, $name! How are you $world?";
};

echo $salute("Zehra");
~~~~~~~
> Output: Hello, Zehra! How are you World?
> + `$salute` ve `$world` değişkenleri closure'a `use` keyword ile dahil edilmiştir.

3. Değişkenlerin Referans Olarak Dahil Edilmesi
+ Değişkenleri referans olarak dahil etmek, closure'ın bu değişkenleri değiştirmesine olanak tanır.
~~~~~~~
$num = 1;

$increase = function() use (&$num) {
    $num++;
};

$increase();
echo $num;
$increase();
echo $num;
~~~~~~~
> Output: 2 

> Output: 3

> + `$num` değişkeni referans olarak closure'a dahil edilmiştir, bu nedenle closure içindeki değişiklikler dışarıdaki değişkeni etkiler.

4. Closure ve Global Değişkenler
+ Closure'lar, `use` keyword kullanılmadan doğrudan global değişkenlere erişemezler. Ancak global keyword ile global değişkenlere erişim sağlanabilir.
~~~~~~~
$globalMsg = "Hello World";

$salute = function() {
    global $globalMsg;
    return $globalMsg;
};

echo $salute();
~~~~~~~
> Output: Hello World
> + `global` keyword ile closure içinde global bir değişkene erişilmiştir.

#### Closure'ların Kullanım Alanları
1. Callback Fonksiyonlar
+ Closure'lar, callback fonksiyonları olarak sıkça kullanılır.
~~~~~~~
$numbers = [1, 2, 3, 4, 5];

$square = array_map(function($num) {
    return $num * $num;
}, $numbers);

print_r($square);
~~~~~~~
> Output: Array ( [0] => 1 [1] => 4 [2] => 9 [3] => 16 [4] => 25 )

2. Dinamik ve Geçici İşlemler
+ Dinamik olarak tanımlanan ve belirli işlemler için kullanılan fonksiyonlar.
~~~~~~~
$operation = "+";
$calculate = function($x, $y) use ($operation) {
    switch ($operation) {
        case "+": return $x + $y;
        case "-": return $x - $y;
        default: return 0;
    }
};

echo $calculate(10, 5);
~~~~~~~
> Output: 15

3. Kapsamı Sınırlı Fonksiyonlar
+ Sadece belirli bir işlem veya görev için gerekli olan, genellikle tek kullanımlık fonksiyonlar.
~~~~~~~
function filter($array, $callback) {
    $result = [];
    foreach ($array as $value) {
        if ($callback($value)) {
            $result[] = $value;
        }
    }
    return $result;
}

$numbers = [1, 2, 3, 4, 5, 6];
$evenNumbers = filter($numbers, function($num) {
    return $num % 2 == 0;
});

print_r($evenNumbers);
~~~~~~~
> Output: Array ( [0] => 2 [1] => 4 [2] => 6 )

> Closure'lar, PHP'de dinamik ve esnek programlama yapısı sağlamak için güçlü bir araçtır. Dış değişkenleri kullanabilme yeteneği, fonksiyonel programlama yaklaşımlarını destekler ve daha modüler kod yazmayı mümkün kılar.

***
### Callable data type & callback functions (çağrılabilir data tipleri ve geri çağrılan fonksiyonlar) nedir?
***

***
### Closure vs callable (kapatılabilir vs çağrılabilir) nedir?
***

***
### Arrow functions nedir?
***
