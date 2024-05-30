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
#### Callable Data Type
+  `callable` türü, çağrılabilir bir fonksiyon, yöntem veya bir closure anlamına gelir. Bir değişken veya ifade `callable` olarak tanımlandığında, bu onun bir fonksiyon olarak çağrılabileceği anlamına gelir.
+  Callable türü, fonksiyonlar, anonim fonksiyonlar, closure'lar, class yöntemleri ve hatta class'ın belirli bir örneği ile birlikte kullanabilecek yöntemleri içerir.

###### Örnekler
1. Fonksiyon İsmi
~~~~~~~
function selamla() {
    echo "Hello, World!";
}

$func = 'hello';
if (is_callable($func)) {
    $func();!
}
~~~~~~~
> Output: Hello, World!

2. Anonim Fonksiyon (Closure)
~~~~~~~
$salute = function() {
    echo "Hello, Anonymous Function!";
};

if (is_callable($salute)) {
    $salute();
}
~~~~~~~
> Output: Hello, Anonymous Function!

3. Class Yöntemi
~~~~~~~
class Greeter {
    public function salute() {
        echo "Hello, Class Management!";
    }
}

$object = new Greeter();
$method = [$object, 'salute'];

if (is_callable($method)) {
    $method();
}
~~~~~~~
> Output: Hello, Class Management!

4. Statik Class Yöntemi
~~~~~~~
class Greeter {
    public static function salute() {
        echo "Hello, Static Class Management!";
    }
}

$method = ['Greeter', 'salute'];

if (is_callable($method)) {
    $method();
}
~~~~~~~
> Output: Hello, Static Class Management!

#### Callback Functions
+ Callback fonksiyonları, bir işlevin argümanı olarak geçirilen ve bu işlev içinde çağrılan fonksiyonlardır. Callback'ler, programın belirli bir noktasında bir fonksiyonun çalıştırılmasını sağlamak için kullanılır.

###### Örnekler
1. Temel Callback Fonksiyonu
~~~~~~~
function funcBuild($callback) {
    if (is_callable($callback)) {
        $callback();
    }
}

function hello() {
    echo "Hello, Callback!";
}

funcBuild('hello');
~~~~~~~
> Output: Hello, Callback!

2. Anonim Fonksiyon Callback
~~~~~~~
function funcBuild($callback) {
    if (is_callable($callback)) {
        $callback();
    }
}

funcBuild(function() {
    echo "Hello, Anonymous Callback!";
});
~~~~~~~
> Output: Hello, Anonymous Callback!

3. Parametreli Callback
~~~~~~~
function funcBuild($callback, $param) {
    if (is_callable($callback)) {
        $callback($param);
    }
}

function hello($name) {
    echo "Merhaba, $name!";
}

funcBuild('hello', 'Zehra');
~~~~~~~
> Output: Hello, Zehra!

4. Class Yönetimi Callback
~~~~~~~
function funcBuild($callback, $param) {
    if (is_callable($callback)) {
        $callback($param);
    }
}

class Greeter {
    public function hello($name) {
        echo "Hello, $name!";
    }
}

$object = new Greeter();
funcBuild([$object, 'hello'], 'Zehra');
~~~~~~~
> Output: Hello, Zehra!

##### Kullanım Alanları
1. Array Fonksiyonları
+ Callback fonksiyonları, `array_map`, `array_filter`, `array_walk` gibi array işleme fonksiyonlarında sıkça kullanılır.
~~~~~~~
$numbers = [1, 2, 3, 4, 5];

$square = function($num) {
    return $num * $num;
};

$squared = array_map($square, $numbers);
print_r($squared);
~~~~~~~
> Output: Array ( [0] => 1 [1] => 4 [2] => 9 [3] => 16 [4] => 25 )

2. Event Handling (Olay Yönetimi)
+ Callback'ler, olay tabanlı programlama ve event handler'lar oluşturmak için kullanılır.
~~~~~~~
function clickButton($callback) {
    // The called function when click the button
    $callback();
}

clickButton(function() {
    echo "Button clicked!";
});
~~~~~~~
> Output: Button clicked!

3. Karmaşık İşlem ve Akış Kontrolü
+ Karmaşık işlem akışlarında belirli işlemleri dinamik olarak belirlemek için kullanılır.
~~~~~~~
function selectOprt($operation, $x, $y) {
    if (is_callable($operation)) {
        return $operation($x, $y);
    }
    return null;
}

$sum = function($x, $y) {
    return $x + $y;
};

$result = selectOprt($sum, 10, 5);
echo $result;
~~~~~~~
> Output: 15

> Callable ve callback fonksiyonları, PHP'de esnek ve dinamik programlama yapılmasını sağlar. Bu özellikler, kodun daha modüler ve yeniden kullanılabilir hale getirir.

***
### Closure vs callable (kapatılabilir vs çağrılabilir)
***
##### Closure
+ Closure'lar, PHP'de fonksiyonların adını belirtmeden anonim olarak tanımlanabilen, ve genellikle bir değişkene atanarak kullanılan fonksiyonlardır.
+ Closure'lar, tanımlandıkları yerin kapsamındaki değişkenlere erişebilirler. Bu özellik, closure'ların tanımlandıkları bağlamdan veri alabilmesini sağlar.

#### Özellik ve Kullanım
+ `Anonim Fonksiyonlar:` Closure'lar anonim fonksiyonlar olarak da bilinirler ve bir değişkene atanabilirler.
+ `Kapsam Kullanımı:` `use` keyword ile dış değişkenlere erişim sağlanabilir.
+ `Closure Class:` PHP'de closure'lar Closure Class'ının bir örneğidir.

~~~~~~~
$message = "Hello, world!";

$closure = function() use ($message) {
    echo $message;
};

$closure();
~~~~~~~
> Output: Hello, world!

##### Callable
+ Callable, PHP'de çağrılabilir bir fonksiyonu veya yöntemi ifade eden bir veri türüdür.
+ Callable bir değişken veya ifade, bir fonksiyon, bir class'ın statik method'u, bir class'ın örneği ile kullanılan bir method, veya bir Closure olabilir.

#### Özellik ve Kullanım
+ `Fonksiyon İsmi:` Bir fonksiyonun adı callable olabilir.
+ `Anonim Fonksiyonlar:` Closure'lar da callable türündedir.
+ `Class Yöntemleri:` Bir class'ın yöntemi callable olabilir.
+ `Statik Yöntemler:` Bir class'ın statik yöntemi de callable olabilir.

~~~~~~~
function greet() {
    echo "Hello!";
}

$callable = 'greet';

if (is_callable($callable)) {
    $callable();
}
~~~~~~~
> Output: Hello!

~~~~~~~
class Greeter {
    public static function sayHello() {
        echo "Hello from Greeter!";
    }
}

$callableStatic = ['Greeter', 'sayHello'];

if (is_callable($callableStatic)) {
    $callableStatic();
}
~~~~~~~
> Output: Hello from Greeter!
> + `greet` fonksiyonu ve `Greeter::sayHello` statik method'u callable türünde değişkenler olarak kullanılmıştır.

#### Closure vs Callable
+ Tanımlama
    - `Closure:` Anonim fonksiyonlar olarak tanımlanır ve bir değişkene atanabilir.
    - `Callable:` Fonksiyon, closure, class method'u veya statik method olabilir.

+ Erişim
    - `Closure:` `use` keyword ile dış değişkenlere erişebilir.
    - `Callable:` Bir fonksiyonun veya method'un çağrılabilir olup olmadığını kontrol etmek için kullanılır.

+ Class
    - `Closure:` Closure class'ın bir örneğidir.
    - `Callable:` Callable türü, fonksiyonların ve yöntemlerin genel çağrılabilirliğini ifade eder.

+ Kullanım Alanı
    - `Closure:` Anonim fonksiyonlar, kapsama bağlı değişkenlere erişim, dinamik fonksiyon tanımlama
    - `Callable:` Dinamik fonksiyon çağrıları, callback mekanizmaları, genel amaçlı çağrılabilir fonksiyon referansları

+ Örnek
    - `Closure:` $closure = function() use ($var) { ... };
    - `Callable:` $callable = 'functionName'; or $callable = [$object, 'methodName'];

***
### Arrow functions nedir?
***
+ Arrow functions, PHP 7.4 ile tanıtılan ve daha kısa ve okunabilir bir şekilde anonim fonksiyonlar (closure'lar) yazmaya olanak tanıyan bir özelliktir.
+ Arrow functions, `fn` keyword ile tanımlanır ve dış kapsamdan (parent scope) değişkenlere otomatik olarak erişebilir. Bu özellik, `use` keyword'ünü kullanmadan dış değişkenlere erişimi sağlar.

#### Özellikler ve Kullanım
+ `Kısa Söz Dizimi:` Daha kısa ve öz bir sözdizimine sahiptir. Tek satırlık fonksiyon tanımlamaları için idealdir.
+ `Otomatik Erişim:` Tanımlandıkları yerin kapsamındaki değişkenlere otomatik olarak erişebilir. `use` keyword'ü gerektirmez.
+ `Tek Satırlık Gövde:` Arrow functions'ın gövdesi tek bir ifade olmalıdır ve bu ifade otomatik olarak döndürülür (`return` keyword'ü gerektirmez).

###### Örnekler

1. Temel Kullanım Örneği
~~~~~~~
$multiple = 2;
$multiplication = fn($num) => $num * $multiple;

echo $multiplication(5);
~~~~~~~
> Output: 10
> + `$multiplication` arrow function'ı `$multiple` değişkenine otomatik olarak erişir ve verilen sayıyı bu değişkenle çarpar.

2. Daha Karmaşık Bir Örnek
~~~~~~~
$num1 = 10;
$num2 = 20;

$sum = fn($x, $y) => $x + $y + $num1 + $num2;

echo $sum(5, 5);
~~~~~~~
> Output: 40
> + `$sum` arrow function'ı, dış kapsamdan `$num1` ve `$num2` değişkenlerine otomatik olarak erişir ve verilen parametrelerle birlikte bu değişkenleri toplar.

##### Arrow Functions ile `array_map` Kullanımı
+ Arrow functions, array'ler üzerinde işlem yapmak için kullanılan fonksiyonlarla (örneğin array_map) çok kullanışlıdır.
~~~~~~~
$numbers = [1, 2, 3, 4, 5];
$multiple = 2;

$result = array_map(fn($num) => $num * $multiple, $numbers);

print_r($result);
~~~~~~~
> Output: Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 [4] => 10 )
> +

##### Arrow Functions ile `usort` Kullanımı:
+ Arrow functions, kullanıcı tanımlı sıralama işlevleri için de kullanılabilir.
~~~~~~~
$names = ["Ali", "Zehra", "Ahmet", "Ayşe"];

usort($names, fn($x, $y) => strlen($x) <=> strlen($y));

print_r($isimler);
~~~~~~~
> Output: Array ( [0] => Ali [1] => Ayşe [2] => Zehra [3] => Ahmet )
> + `usort` fonksiyonu, array elemanlarını uzunluklarına göre sıralamak için bir arrow function kullanır.

#### Arrow Functions vs. Anonymous Functions (Closure'lar)
+ Tanımlama
    - `Arrow Functions:` fn($param) => $expression
    - `Anonymous Functions:` function($param) use ($variable) { return $expression; }

+ Gövde
    - `Arrow Functions:` Tek bir ifade
    - `Anonymous Functions:` Birden fazla ifade veya komut içerebilir

+ `use` Keyword
    - `Arrow Functions:` Gerektirmez, otomatik erişim sağlar
    - `Anonymous Functions:` Dış değişkenlere erişim için `use` keyword'ü gerektirir

+ Kapsamdan Değişken Erişimi
    - `Arrow Functions:` Otomatik olarak erişebilir
    - `Anonymous Functions:` `use` keyword'ü ile manuel olarak erişim sağlar
