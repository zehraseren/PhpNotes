***
### Functions nedir?
***
+ Belirli bir görevi yerine getiren kod bloklarıdır. Fonksiyonlar, kodun modüler ve yeniden kullanılabilir hale getirir.
+ Bir fonksiyon tanımlandıktan sonra, istenilen yerde ve istenilen kadar çağrılabilir.
+ Fonksiyonlar, yerleşik (built-in) fonksiyonlar ve kullanıcı tanımlı (user-defined) fonksiyonlar olarak ikiye ayrılır.
~~~~~~~
function greeting($name) {
  echo "Hello, $name!";
}

greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra!

##### Özellikleri
+ `Parametreler:` Girdileri işlemek için parametreler alabilir. Parametreler, virgülle ayrılır ve parantez içinde belirtilir.
+ `Geri Dönüş Değeri (Return Value):` `return` anahtar kelimesi ile bir değer döndürebilir.
+ `Yerel ve Global Değişkenler:` Fonksiyon içinde tanımlanan değişkenler yerel (local) değişkenlerdir ve sadece o fonksiyon içinde geçerlidir. Global değişkenlere fonksiyon içinden erişmek için `global` key kelimesi kullanılır.

#### Geri Dönüş Değeri (Return Value)
+ `return` ifadesi kullanılarak bir değer döndürebilir. `return` ifadesi aynı zamanda fonksiyonun çalışmasını sonlandırır.
~~~~~~~
function sum($x + $y) {
  return $x + $y;
}

$result = sum(7, 9);
echo $result;
~~~~~~~
> Output: 16

#### Varsayılan Parametre Değerleri
+ Fonksiyon parametrelerine varsayılan değerler atanabilir. Bu durumda, fonksiyon çağrılırken bu parametreler atlanabilir.
~~~~~~~
function greeting($name = "Visitor") {
  echo "Hello, $name!";
}

greeting();
greeting("Zehra");
~~~~~~~
> Output: Hello, Visitor!

> Output: Hello, Zehra!

#### Değişken Sayıda Parametreler
+ Bir fonksiyonun değişken sayıda parametre almasını sağlamak için `... (splat operator)` kullanılabilir.
~~~~~~~
function sum(...$numbers) {
  return array_sum($numbers);
}

echo sum(1, 2, 3, 4);
~~~~~~~
> Output: 10

#### Anonim Fonksiyonlar ve Kapatmalar (Closures)
+ Anonim fonksiyonlar, adlandırılmamış fonksiyonlardır ve genellikle değişkenlere atanabilir veya diğer fonksiyonlara argüman olarak geçilebilir.
+ Kapatmalar ise, anonim fonksiyonların kapsamındaki değişkenlere erişimini sağlayan fonksiyonlardır.
> Bu, fonksiyonların durumu (state) saklayabileceği ve daha sonra bu durumu değiştirebileceği anlamına gelir. Bu özellik, daha karmaşık ve dinamik fonksiyonlar oluşturmak için oldukça kullanışlıdır.

###### Anonim Fonksiyon
~~~~~~~
$array = [1 ,2 , 3, 4, 5];

$squaredArray = array_map(function($n) {
  return $n * $n;
}, $array);
~~~~~~~
> Output: Array ( [0] => 1 [1] => 4 [2] => 9 [3] => 16 [4] => 25 )
> + `array_map` fonksiyonunu kullanarak bir array'in her elemanına anonim bir fonksiyon uygular ve elemanlarının karesini alır.

###### Kapatma (Closure)
~~~~~~~
function createCounter() {
    $count = 0;
    return function() use (&$count) {
        return ++$count;
    };
}

$counter = createCounter();
echo $counter() . "\n";
echo $counter() . "\n";
echo $counter() . "\n";
~~~~~~~
> Output: 1

> Output: 2

> Output: 3

> + `createCounter` fonksiyonu, bir sayacı döndürür. `use (&$count)` ifadesi ile `$count` değişkeni referans olarak anonim fonksiyona geçirilir, böylece her çağrıldığında `$count` değeri artırılır ve güncellenir.

#### Recursive Fonksiyonlar
+ Recursive (özyinelemeli) fonksiyonlar, kendi kendini çağıran fonksiyonlardır. Bu tür fonksiyonlar, genellikle belirli bir problemi tekrarlayan alt problemlere ayırmak için kullanılır.
~~~~~~~
function factorial($n) {
  if ($n <= 1) {
    return 1;
  }
  return $n * factorial($n - 1);
}

echo "5! = " . factorial(5) . "\n";
~~~~~~~
> Output: 120

***
### Where to define functions (fonksiyonlar nerede tanımlanır)?
***
+ Fonksiyonları nerede tanımlanması gerektiği, uygulamanın yapısına ve gereksinimlerine bağlıdır.
+ Fonksiyonların tanımlanma yeri, kodun okunabilirliğini, yeniden kullanılabilirliğini ve bakımını doğrudan etkiler.
----
1. Global Scope (Küresel Kapsam)
+ Fonksiyonlar genellikle küresel kapsamda, yani betiğin en üst seviyesinde tanımlanır. Bu, fonksiyonların betiğin her yerinden erişilebilir olmasını sağlar.
~~~~~~~
function greeting($name) {
    echo "Hello, $name!";
}

greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra!

  - Avantajlar:
      - `Kolay Erişim:` Fonksiyonlar betiğin her yerinden çağrılabilir.
      - `Basitlik:` Küçük betiklerde veya basit uygulamalarda kullanışlıdır.
  
  - Dezavantajlar:
      - `Ad Çakışmaları:` Büyük projelerde fonksiyon adlarının çakışması riski vardır.
      - `Modülerlik Eksikliği:` Fonksiyonlar belirli bir modüle veya class'a ait değildir.

----
2. Dosya Dahil Etme | `include, require`
+ Fonksiyonları ayrı bir dosyada tanımlayıp, bu dosyayı gerektiği yerde betiğe dahil edilebilir. Bu, kodu modüler hale getirir ve tekrar kullanılabilirliği artırır.
###### 1. functions.php
~~~~~~~
function greeting($name) {
    echo "Hello, $name!";
}
~~~~~~~

###### 2. main.php
~~~~~~~
include 'functions.php';
greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra!

  - Avantajlar:
      - `Modülerlik:` Fonksiyonlar ayrı dosyalarda düzenlenebilir.
      - `Yeniden Kullanılabilirlik:` Aynı fonksiyonlar farklı betiklerde kullanılabilir.
  
  - Dezavantajlar:
      - `Dosya Yönetimi:` Çok sayıda dosya olduğunda yönetim zorlaşabilir.
      - `Performans:` Büyük projelerde dosya dahil etme işlemi performansı etkileyebilir.

----
3. Class'lar İçinde (Nesne Yönelimli Programlama | OOP)
+ Fonksiyonlar, nesne yönelimli programlama (OOP) yaklaşımı ile class'lar içinde method olarak tanımlanabilir. Bu, kodun daha organize ve bakımının kolay olmasını sağlar.
~~~~~~~
class Greeter {
  public function greeting($name) {
    echo "Hello, $name!";
  }
}

$greeter = new Greeter();
$greeter -> greeting("Zehra");
~~~~~~~
> Output: Hello, Zehra!

  - Avantajlar:
      - `Modülerlik ve Organizasyon:` Kod daha düzenli ve modüler hale gelir.
      - `Kapsülleme:` Fonksiyonlar class'lar içinde kapsüllenebilir.
      - `Yeniden Kullanılabilirlik:` Class'lar ve method'lar farklı yerlerde tekrar kullanılabilir.
  
  - Dezavantajlar:
      - `Karmaşıklık:` Basit projelerde gereksiz karmaşıklık yaratabilir.
      - `Öğrenme Eğrisi:` OOP yaklaşımını öğrenmek zaman alabilir.

----
4. Anonim Fonksiyonlar ve Kapatmalar (Closures)
+ Anonim fonksiyonlar ve kapatmalar, özellikle geri çağırma (callback) fonksiyonları veya bir fonksiyonun sadece belirli bir bağlamda kullanılması gerektiğinde kullanışlıdır.
~~~~~~~
$greet = function($name) {
  echo "Hello, $name!";
}

$greet("Zehra");
~~~~~~~
> Output: Hello, Zehra!

  - Avantajlar:
      - `Kapsam Kapsama (Scope):` Anonim fonksiyonlar, tanımlandıkları yerde kullanılabilir.
      - `Esneklik:` Fonksiyonları dinamik olarak oluşturma ve kullanma imkanı verir.
  
  - Dezavantajlar:
      - `Karmaşıklık:` Anonim fonksiyonlar, özellikle uzun ve karmaşık olduğunda kodun okunabilirliğini zorlaştırabilir.
      - `Yeniden Kullanılabilirlik:` Anonim fonksiyonlar, adlandırılmış fonksiyonlar kadar kolay tekrar kullanılamaz.
----
#### Fonksiyonları nerede tanımlanacağına karar verirken aşağıdaki faktörleri göz önünde bulundurmalıdır:
+ `Projenin Büyüklüğü ve Karmaşıklığı:` Büyük ve karmaşık projelerde, fonksiyonların modüler olması önemlidir.
+ `Kodun Okunabilirliği ve Bakımı:` Kodunuzu düzenli ve anlaşılır tutmak için fonksiyonları uygun yerlerde tanımlayın.
+ `Yeniden Okunabilirlik:` Fonksiyonların farklı betiklerde veya projelerde tekrar kullanılmasını sağlamak için modüler yapıyı tercih edin.
> Genel olarak, küçük ve basit projelerde fonksiyonları küresel kapsamda (global scope) tanımlamak yeterli olabilir. Ancak, daha büyük ve karmaşık projelerde fonksiyonları class'lar içinde method olarak tanımlamak (OOP içinde) veya ayrı dosyalarda tutmak (include | require), kodunuzu daha organize ve yönetilebilir hale getirecektir.

*** 
### Declaring functions conditionally (şartlı fonksiyon tanımlama) nedir?
***
+ Fonksiyonları şartlı olarak (conditionally) tanımlamak, belirli bir koşulun gerçekleşmesine bağlı olarak fonksiyon tanımlamak anlamına gelir.
+ Aynı fonksiyon adının tekrar tanımlanmasını önlemek veya belirli bir duruma bağlı olarak farklı fonksiyonlar tanımlamak için kullanılabilir.
+  `if, else, elseif` gibi kontrol yapılarını kullanarak yapılabilir. Böylece belirli bir koşul sağlandığında veya sağlanmadığında fonksiyonun tanımlanmasını sağlar.

##### Örnekler
1. Basit Şartlı Tanımlama
~~~~~~~
$condition = true;

if($condition) {
  function hello() {
    echo "Hello World!";
  }
}

if (function_exists)('hello')) {
  hello();
}
~~~~~~~
> Output: Hello World!
> + `$condition` değişkeni `true` olduğu için, `hello` fonksiyonu tanımlanır. Eğer koşul `false` olsaydı, `hello` fonksiyonu tanımlanmazdı.

2. Birden Fazla Şartlı Tanımlama
~~~~~~~
$condition = false;

if($condition) {
  function hi() {
    echo "Hi!";
  }
} else {
  function hi() {
    echo "Hello!";
  }
}

hi();
~~~~~~~
> Output: Hello!
> + `hi` fonksiyonu, `condition` değişkenine bağlı olarak farklı şekilde tanımlanır. `$condition` değişkeni `false` olduğu için `Hello!` çıktısını verir.

3. Dinamik Fonksiyon Tanımlama
~~~~~~~
$condition = mt_rand(0, 1) == 1;

if ($condition) {
    function dynamicFunction() {
        echo "Condition true!";
    }
} else {
    function dynamicFunction() {
        echo "Condition false!";
    }
}

dynamicFunction();
~~~~~~~
> Output: Condition true! or Condition false!
> + `dynamicFunction` fonksiyonu, rastgele oluşturulan `$conditon` değerine bağlı olarak farklı şekillerde tanımlanır.

> + `mt_rand() fonksiyonu`, belirtilen iki sayı arasındaki rastgele bir tamsayıyı üretir. Bu fonksiyon, daha hızlı ve daha iyi rastgele sayı üreten bir algoritma kullanır.
> + `mt_rand(0, 1):` Bu fonksiyon, 0 ile 1 arasında rastgele bir tamsayı döndürür. Yani, sonuç ya 0 ya da 1 olur.
> + `== 1:` Elde edilen rastgele sayı 1'e eşit mi diye kontrol edilir.
> + Kontrol sonucunda elde edilen boolean değer (true ya da false) $kosul değişkenine atanır.

#### `function_exists` ile Fonksiyon Varlığını Kontrol Etme
+ Bir fonksiyonun zaten tanımlı olup olmadığını kontrol etmek için `function_exists` fonksiyonu kullanılabilir. `Böylece aynı fonksiyon adının tekrar tanımlanmasını önlenir.`
~~~~~~~
if (!function_exists('hello')) {
    function hello() {
        echo "Hello World!";
    }
}

merhaba();
~~~~~~~
> Output: Hello World!
> + Eğer `hello` fonksiyonu daha önce tanımlanmamışsa, bu tanımlama yapılır.

##### Kullanım Alanları
+ Şartlı fonksiyon tanımlamanın bazı yaygın kullanım alanları şunlardır:
  - `Backward Compatibility (Geriye Dönük Uyumluluk):` Eski sürümlerde tanımlı olmayan fonksiyonları sadece ihtiyaç duyulduğunda tanımlamak.
  - `Özelleştirilmiş Fonksiyonlar:` Uygulamanın çalışma ortamına veya belirli parametrelere göre farklı fonksiyon davranışları sağlamak.
  - `Ad Çakışmalarını Önleme:` Aynı fonksiyon adının birden fazla yerde tanımlanmasını önlemek.

> PHP'de şartlı fonksiyon tanımlama, uygulamanın dinamik ve esnek olmasını sağlar. Fonksiyonları belirli koşullara bağlı olarak tanımlayarak kodunuzun modülerliğini ve bakımını artırabilir. Bu yöntem, özellikle büyük ve karmaşık projelerde kodunuzu daha yönetilebilir ve esnek hale getirmek için oldukça kullanışlıdır.

***
### Inner functions (iç içe fonksiyolar) nedir?
***
+ Bir fonksiyonun içinde tanımlanan başka bir fonksiyonu ifade eder.
+ İç fonksiyonlar, yalnızca onları tanımlayan dış fonksiyon çalıştırıldığında tanımlanır ve sadece bu dış fonksiyonun kapsamı içinde kullanılabilir.
+ Belirli bir görev veya işlem için gereken işlevselliği kapsüllemek ve kodunuzu daha modüler hale getirmek için kullanışlı olabilir.
~~~~~~~
function externalFunction() {
    echo "External function worked.\n";

    function innerFunction() {
        echo "Inner function worked.\n";
    }
}

// Sadece dış fonksiyon çağrıldığında iç fonksiyon tanımlanır.
externalFunction();

// İç fonksiyonu çağırmak artık mümkündür.
innerFunction();
~~~~~~~
> Output: External function worked.

> Output: Inner function worked.

> + `innerFunction` yalnızca `externalFunction` çağrıldıktan sonra tanımlanır ve kullanılabilir hale gelir. 

#### İç Fonksiyonların Özellikleri ve Kullanım Alanları
##### Özellikler
  1. `Kapsam (Scope):` İç fonksiyonlar, dış fonksiyon çalıştırıldığında tanımlanır ve dış fonksiyonun kapsamında bulunur.
  2. `Tekrar Kullanılabilirlik:` İç fonksiyonlar sadece dış fonksiyon çağrıldıktan sonra kullanılabilir hale geldiği için, kodun belirli bir bölümünde tanımlı olan işlevleri izole eder.
  3. `Kapsülleme:` Kodun belirli bir bölümünü kapsüllemek ve dış fonksiyonun dışında kullanılmasını önlemek için kullanılır.
     
##### Kullanım Alanları
  1. `Modülerlik:` Büyük fonksiyonları daha küçük ve yönetilebilir parçalara böler.
  2. `Yalnızca Belirli Kapsamda Kullanım:` Belirli bir fonksiyonun yalnızca bir başka fonksiyon tarafından kullanılmasını sağlar.
  3. `Geçici Fonksiyonlar:` Belirli bir işlemin yapılması sırasında geçici olarak gerekli olan fonksiyonlardır.

##### Örnekler
1. İç Fonksiyon ile Hesaplama
~~~~~~~
function extFunc($value) {
    function inFunc($x) {
        return $x * 2;
    }

    echo "Result: " . inFunc($value) . "\n";
}

extFunc(5); 
~~~~~~~
> Output: 10
> + `inFunc` sadece `extFunc` içinde kullanılır ve `$value` değişkenini ikiyle çarpar.

2. Birden Fazla İç Fonksiyon
~~~~~~~
function calculator($value1, $value2, $operation) {
    function sum($x, $y) {
        return $x + $y;
    }

    function ext($x, $y) {
        return $x - $y;
    }

    if ($operation == "sum") {
        return sum($value1, $value2);
    } elseif ($operation == "ext") {
        return ext($value1, $value2);
    } else {
        return "Invalid operation";
    }
}

echo calculator(10, 5, "sum") . "\n";
echo calculator(10, 5, "ext") . "\n";
~~~~~~~
> Output: 15

> Output: 5

> + `calculator` fonksiyonu içinde `sum` ve `ext` adında iki iç fonksiyon tanımlanır ve `$operation` parametresine göre uygun fonksiyon çağrılır.

##### Dikkat Edilmesi Gerekenler
  - `Performans:` İç fonksiyonlar, dış fonksiyon her çağrıldığında yeniden tanımlanır, bu nedenle sıkça çağrılan fonksiyonlarda performans sorunlarına yol açabilir.
  - `Kapsam Hatası:` İç fonksiyonlar yalnızca dış fonksiyonun çalıştırılması sonrası tanımlanır, bu nedenle uygun şekilde kullanılmadığında kapsam hatalarına neden olabilir.

> İç fonksiyonlar, belirli bir işlevi izole etmek ve kapsüllemek için güçlü bir araçtır. Kodu daha modüler ve yönetilebilir hale getirmek için kullanabilir. Ancak, performans ve kapsam konularına dikkat etmek önemlidir. Uygun şekilde kullanıldığında, iç fonksiyonlar kodu daha düzenli ve okunabilir hale getirir.

***
### Functions with the same name (aynı isimde fonksiyonlar) nedir?
***
+ Aynı isimde birden fazla fonksiyon tanımlamak mümkün değildir.
+ Bir fonksiyon bir kez tanımlandıktan sonra, aynı isimle başka bir fonksiyon tanımlamaya çalışılırsa, bir hata oluşur. `Dolayısıyla fonksiyon isimlerinin benzersiz olması gerekir.`
+ Eğer aynı isimde iki fonksiyon tanımlamaya çalışılırsa alınacak hata aşağıdaki gibidir:
  > Fatal error: Cannot redeclare function_name() in /path/to/file.php on line X

#### Çözüm Yolları
1. Fonksiyon İsimlerinin Benzersiz Olmasını Sağlamak
+ Farklı fonksiyonlar tanımlarken, isimlerinin benzersiz olmasına dikkat edilmelidir. Fonksiyon isimlerinin anlamlı ve spesifik olması, ad çakışmalarını önlemeye yardımcı olabilir.
~~~~~~~
function salute() {
    echo "Hello!";
}

function saluteTheWorld() {
    echo "Hello Wprld!";
}
~~~~~~~

2. Namespace Kullanımı
+ İsim alanları (namespaces) kullanarak, aynı isimde fonksiyonları farklı isim alanları içinde tanımlanabilir. Bu, özellikle büyük projelerde veya kütüphanelerde ad çakışmalarını önlemeye yardımcı olur.
~~~~~~~
namespace FirstNamespace {
    function salute() {
        echo "Hello from the First Namespace!";
    }
}

namespace SecondNamespace {
    function salute() {
        echo "Hello from the Second Namespace!";
    }
}

FirstNamespace\salute();
SecondNamespace\salute();
~~~~~~~
> Output: Hello from the First Namespace!

> Output: Hello from the Second Namespace!

3. Class İçinde Method Olarak Tanımlamak
+ Fonksiyonları class içinde method olarak tanımlamak da isim çakışmalarını önler. Her class içinde aynı isimde farklı method'lar tanımlanabilir.
~~~~~~~
class FirstClass {
    public function salute() {
        echo "Hello from the First Class!";
    }
}

class SecondClass { {
    public function salute() {
        echo "Hello from the Second Namespace!";
    }
}

$first = new FirstClass();
$first->salute();

$second = new SecondClass();
$second->salute();
~~~~~~~
> Output: Hello from the First Class!

> Output: Hello from the Second Class!

> Bu durumu önlemek için fonksiyon isimleri benzersiz yapmalı, gerekirse namespace veya class'lar kullanarak kod daha modüler ve yönetilebilir hale getirilmelidir. Bu yöntemler, özellikle büyük projelerde kodun bakımını ve anlaşılabilirliğini artıracaktır.

***
### Return types & strict types (dönüş tipleri ve katı tip kontrolü) nedir?
***
+ Fonksiyonların dönüş tipleri (return types) ve katı tip kontrolü (strict types) kodu daha güvenilir ve hataya dayanıklı hale getirir.
+ Fonksiyonların belirli veri türlerini döndürmesini ve belirli türdeki argümanlarla çağrılmasını sağlamaya yardımcı olur.

#### Return Types (Dönüş Tipleri)
+ Fonksiyonun döndürmesi gereken veri türünü belirtir.
+ Eğer fonksiyon belirtilen türde bir değer döndürmezse bir hata oluşur.
~~~~~~~
function sum(int $x, int $y): int {
  return $x + $y;
}

echo sum(5, 3);
~~~~~~~
> Output: 8
> + `sum` fonksiyonunun dönüş türü `int` olarak belirtilmiştir. Fonksiyonun döndürdüğü değer bir tamsayı olmalıdır.

##### Desteklenen Dönüş Tipleri
+ int
+ float
+ bool
+ string
+ array
+ iterable
+ mixed (PHP 8.0 ve sonrasında)
+ object
~~~~~~~
class Person {
  public $name;

  public function __construct($name) {
    $this->name = $name;
  }
}

function newPerson(string $name): Person {
  return new Person($name);
}

$person = newPerson("Zehra");
echo $person->name;
~~~~~~~
> Output: Zehra
> + `newPerson` fonksiyonunun dönüş tipi `Person` sınıfından bir nesne olarak belirtilmiştir.
> + `Person` adında bir class tanımlandı ve bu class'ın `name` özelliğini kurucu method (__construct) aracılığıyla başlatıldı.
> + `newPerson` fonksiyonu, verilen bir isimle yeni bir `Person` nesnesi oluşturuldu ve bu nesne döndürüldü.
> + Sonrasında, oluşturulan `Person` nesnesinin `name` özelliği ekrana yazdırılır.

+ void
~~~~~~~
function greet(): void {
  echo "Hello!";
}
~~~~~~~
> Output: Hello!
> `void` dönüş tipi, fonksiyonun hiçbir değer döndürmediğini belirtir.
> Fonksiyon, işlemlerini tamamladığında otomatik olarak `null` döndürse bile, void dönüş tipi ile çelişmez.

#### Strict Types (Katı Tip Kontrolü)
+ Fonksiyonların belirli türdeki argümanları ve dönüş türlerini katı bir şekilde kontrol etmeyi sağlar.
+ Katı tip kontrolü etkinleştirildiğinde, tip uyumsuzluğu durumunda bir hata fırlatılır.
+ `Katı tip kontrolünü etkinleştirmek için dosyanın en üstüne declare(strict_types=1); satırını eklenmelidir.`
~~~~~~~
declare(strict_types=1);

function multiply(float $x, float $y): float {
    return $x * $y;
}

echo multiply(2.5, 4.2);
~~~~~~~
> Output: 10.5
> + `multiply` fonksiyonu sadece `float` türünde argümanlar kabul eder ve `float` türünde bir değer döndürür.
> + Katı tip kontrolü etkin olduğu için, fonksiyon sadece belirtilen türdeki değerlerle çalışır. Yani `katı tip kontrolü etkinleştirildiğinde fonksiyon argümanları ve dönüş değeri belirtilen türde olmalıdır.`
~~~~~~~
declare(strict_types=1);

function sum(int $xy int $b): int {
    return $x + $y;
}

echo sum(5, 3);
echo sum(5, "3");
~~~~~~~
> Output: 8

> Output: Error: Uncaught TypeError

> + `sum` fonksiyonu yalnızca `int` türünde argümanlar kabul eder. "3" gibi bir string argüman geçildiğinde hata oluşur.

##### Katı Tip Kontrolü ile Esnek Tip Kontrolü Arasındaki Fark
1. Esnek Tip Kontrolü (Default Behavior):
+ Varsayılan olarak esnek tip kontrolü yapar. Bu, türlerin otomatik olarak dönüştürülebildiği anlamına gelir.
~~~~~~~
function sum(int $xy int $b): int {
    return$x + $y;
}

echo sum(5, "3");
~~~~~~~
> Output: 8
> + "3" string'i otomatik olarak `int` türüne dönüştürülür ve hata oluşmaz.

2. Katı Tip Kontrolü:
+ Katı tip kontrolü etkinleştirildiğinde, tür dönüşümü yapılmaz ve uyumsuz türler hata fırlatır.
~~~~~~~
declare(strict_types=1);

function sum(int $xy int $b): int {
    return $x + $y;
}

echo sum(5, "3");
~~~~~~~
> Output: Error: Uncaught TypeError
> + "3" string'i, `int` türüne dönüştürülemez ve bir hata fırlatılır.

> Dönüş türleri ve katı tip kontrolü kullanmak, kodun daha güvenilir, hataya dayanıklı ve anlaşılır olmasını sağlar. Dönüş türleri, fonksiyonların döndürmesi gereken veri türünü belirlerken, katı tip kontrolü fonksiyon argümanlarının ve dönüş türlerinin belirtilen türde olmasını garanti eder. Bu özellikler, özellikle büyük ve karmaşık projelerde kodun bakımını ve güvenilirliğini artırır.

***
### Nullable types nedir?
***
+ Bir değişkenin belirtilen veri türünü ya da `null` değerini alabileceğini ifade eder. Yani bir fonksiyonun argümanı ya da dönüş değeri için belirli bir türdeki değeri ya da null değerini kabul etmesini sağlar.

#### Nullable Types Nasıl Kullanılır?
+ Nullable types kullanmak için tür tanımının önüne `?` işareti eklenir. Bu, değişkenin belirtilen türde bir değer ya da null olabileceğini belirtir.

##### Nullable Argüman
~~~~~~~
function hello(?string $name) {
    if ($name === null) {
        echo "Hello, guest!\n";
    } else {
        echo "Hello, $name!\n";
    }
}

hello("Zehra");
hello(null);
~~~~~~~
> Output: Hello, Zehra!

> Output: Hello, guest!

> + `hello` fonksiyonu string türünde bir argüman ya da null kabul eder. `null` değeri geçirildiğinde farklı bir mesaj yazdırır.

##### Nullable Dönüş Değeri
~~~~~~~
function findable(int $id): ?string {
    $data = [1 => "One", 2 => "Two", 3 => "Three"];
    
    return $data[$id] ?? null;
}

echo findable(2);
echo findable(5);
~~~~~~~
> Output: Two

> Output: The return value is null

> + `findable` fonksiyonu bir string ya da null döndürebilir. `id` değeri veritabanında bulunamazsa null döner.

#### Nullable Types Kullanım Alanları
+ `Opsiyonel Parametreler:` Bir fonksiyonun belirli bir parametreyi opsiyonel hale getirmek ve yoksa `null` değerini kabul etmek için.
+ `Database Sorguları:` Bir database sorgusunun sonucunda bulunmayan kayıtlar için `null` döndürebilir.
+ `İsteğe Bağlı Dönüş Değerleri:` Bir fonksiyonun belirli bir değeri döndüremediği durumlarda `null` döndürebilir.

#### Katı Tip Kontrolü ile Kullanımı
+ Katı tip kontrolü etkinleştirildiğinde, `nullable` types hala çalışır ve belirtilen türde ya da `null` değerlerini kabul eder.
~~~~~~~
declare(strict_types=1);

function strictTypesNullable(?int $value): ?int {
    return $value;
}

echo strictTypesNullable(5);
echo strictTypesNullable(null);
~~~~~~~
> Output: 5

> Output: Return value null

> + `strictTypesNullable` fonksiyonu hem `int` türünde hem de `null` değerleri kabul eder ve döndürebilir.

> `nullable types`, fonksiyonların esnekliğini artırır ve daha güvenilir kod yazılmasına yardımcı olur. Nullable types, belirli bir türdeki değerlerin yanı sıra `null` değerini de kabul edebilme yeteneği sağlar. Bu özellik, özellikle opsiyonel parametreler ve isteğe bağlı dönüş değerleri gibi durumlar için çok kullanışlıdır. Katı tip kontrolü ile birlikte kullanıldığında, kodun doğruluğunu ve güvenilirliğini artırır.

***
### Union types (type hinting multiple types) nedir?
***
+ PHP 8.0 ile birlikte, bir değişkenin birden fazla türü kabul edebileceğini belirtmek için union types özelliği tanıtıldı.
+ Union types, bir değişkenin birden fazla belirli türden birini alabileceğini belirtmek için kullanılır.` Bu, fonksiyonların daha esnek ve dinamik olmasını sağlar.`
+ Union types tanımlamak için, türleri dikey çubuk `|` ile ayırarak belirtilir.

##### Fonksiyon Argümanında Union Types
~~~~~~~
function operation(int|float $num1, int|float $num2): int|float {
    return $num1 + $num2;
}

echo operation(5, 3);
echo operation(5.5, 3.2);
echo operation(5, 3.2);
~~~~~~~
> Output: 8

> Output: 8.7

> Output: 8.2

> + `operation` fonksiyonu hem `int` hem de `float` türünde argümanlar kabul edebilir ve dönebilir.

##### Fonksiyon Dönüş Değerinde Union Types
~~~~~~~
function getData(int $id): int|string {
    $data = [1 => "One", 2 => "Two", 3 => "Three"];
    
    return $data[$id] ?? $id;
}

echo getData(2);
echo getData(4);
~~~~~~~
> Output: Two

> Output: 4

> + `getData` fonksiyonu hem `int` hem de `string` türünde değerler döndürebilir.
> + `return $data[$id] ?? $id;`: `$id` anahtarının `$getData` dizisinde olup olmadığını kontrol eder. `$id` dizide varsa karşılık gelen değeri döndürür; yoksa `$id` kendisini döndürür.
> + `??` operatörü, PHP'de "null birleşim" operatörü olarak bilinir. Sol taraf null ise sağ tarafı döndürür. Ancak, dizide olmayan bir anahtarı kullanmaya çalışmak null döndüreceği için burada uygun bir kullanım sağlar.

#### Union Types ile Nullable Types
+ Union types, nullable types ile birlikte de kullanılabilir. Bu durumda, null değeri de kabul edilebilir bir tür olarak belirtilir.
~~~~~~~
function operation(?int|float $value): ?float {
    return $value ? $value * 2 : null;
}

echo operation(5);
echo operation(5.5);
echo operation(null);
~~~~~~~
> Output: 10

> Output: 11

> Output: Return value null

> + `operation` fonksiyonu `int`, `float` ve `null` değerlerini kabul edebilir ve döndürebilir.

#### Union Types Kullanım Alanları
+ `Esneklik:` Bir fonksiyonun birden fazla türde argüman kabul edebilmesini ve dönebilmesini sağlar.
+ `Gerçek Dünya Modelleri:` Gerçek dünya senaryolarında, bir değer birden fazla türde olabileceğinden, union types kullanarak bu tür senaryoları daha iyi modelleyebilirsiniz.
+ `Geriye Dönük Uyum:` Eski kodlarla uyumluluğu sürdürmek ve yeni tür eklemeleri yapmak için union types kullanabilirsiniz.

#### Katı Tip Kontrolü ile Kullanımı
+ Union types, katı tip kontrolü ile birlikte de çalışır ve tür uyumsuzlukları durumunda PHP'nin hata fırlatmasını sağlar.
~~~~~~~
declare(strict_types=1);

function sum(int|float $x, int|float $y): int|float {
    return $x + $y;
}

echo sum(5, 3.5);
echo sum(5, "3");

~~~~~~~
> Output: 8.5

> Output: Error: Uncaught TypeError

> + `sum` fonksiyonu `int` ve `float` türlerini kabul eder. "3" gibi bir string argüman geçildiğinde, katı tip kontrolü etkin olduğu için hata fırlatılır.

> Union types, fonksiyonların ve değişkenlerin daha esnek ve dinamik olmasını sağlayan güçlü bir özelliktir. Bir değişkenin birden fazla belirli türden birini alabileceğini belirtir ve kodun daha anlaşılır, esnek ve hataya dayanıklı olmasına yardımcı olur. Union types, özellikle büyük projelerde ve karmaşık veri modellerinde kullanışlıdır ve katı tip kontrolü ile birlikte kullanıldığında kodun doğruluğunu artırır.

***
### Mixed Types nedir?
***
+ PHP 8.0 ile birlikte tanıtılan `mixed types`, bir değişkenin birden fazla türde değer alabileceğini belirtmek için kullanılır.
+ Bir değişkenin aşağıdaki türlerden herhangi biri olabileceğini ifade eder:
    - int
    - float
    - string
    - bool
    - array
    - object
    - callable
    - resource
    - null.
    - `mixed types`, özellikle dinamik veri yapıları veya çeşitli türlerde veri döndüren fonksiyonlar için kullanışlıdır.
+ Hem fonksiyon argümanlarında hem de dönüş değerlerinde kullanılabilir.

#### Fonksiyon Argümanında Mixed Types
~~~~~~~
function print(mixed $data): void {
    if (is_array($data)) {
        echo implode(", ", $data);
    } else {
        echo $data;
    }
}

print("Hello World!");
print([1, 2, 3]); 
~~~~~~~
> Output: Hello World!

> Output: 1, 2, 3

> + `print` fonksiyonu mixed türünde bir argüman kabul eder ve argümanın türüne göre farklı işlemler yapar.

#### Fonksiyon Dönüş Değerinde Mixed Types
~~~~~~~
function getData(int $id): mixed {
    $data = [1 => "One", 2 => 2.5, 3 => true];
    
    return $data[$id] ?? null;
}

echo getData(1);
echo getData(2);
echo getData(3);
~~~~~~~
> Output: One

> Output: 2.5

> Output: 1

> + `getData` fonksiyonu `mixed types` türünde bir değer döndürebilir. Dönüş değeri, veritabanında bulunan farklı türlerdeki verilere göre değişir.

#### Mixed Types Kullanım Alanları
+ `Dinammik Data Yapıları:` Dinamik olarak değişen data türlerini işlemek için.
+ `Çeşitli Türlerde Veri Döndüren Fonksiyonlar:` Aynı fonksiyonun farklı türlerde veri döndürebileceği durumlarda.
+ `Geriye Dönük Uyum:` Eski kodlarla uyumluluğu sürdürmek için.

#### Katı Tip Kontrolü ile Kullanımı
+ `mixed types`, katı tip kontrolü ile birlikte de kullanılabilir. Katı tip kontrolü etkin olduğunda, mixed types belirtilen tüm türleri kabul eder ve tür uyumsuzluklarında hata fırlatılmaz.
~~~~~~~
declare(strict_types=1);

function getValue(): mixed {
    return 42;
}

echo getValue();
~~~~~~~
> + `getValue` fonksiyonu `mixed types` bir değer döndürebilir.

~~~~~~~
function addValue(mixed $value): void {
    if (is_string($value)) {
        echo strtoupper($value);
    } else {
        var_dump($value);
    }
}

addValue("test");
addValue([1, 2, 3]);
~~~~~~~
> Output: TEST

> Output: Array(3) { [0]=> int(1) [1]=> int(2) [2]=> int(3) }

> + `addValue` fonksiyonu mixed types olarak bir argüman kabul eder. Katı tip kontrolü etkin olduğunda bile, mixed types belirtilen tüm türleri kabul eder.

#### Mixed Types Avantajları ve Dezavantajları

##### Avantajları
+ `Esneklik:` Mixed types, fonksiyonların ve değişkenlerin çeşitli türlerdeki verilerle çalışabilmesini sağlar. 
+ `Basitlik:` Farklı türdeki verileri işleyen fonksiyonları yazarken kodu basitleştirir.
+ `Uyumluluk:` Eski kodlarla uyumlu olmayı sürdürür ve genişletilebilirlik sağlar.

##### Dezavantajları
+ `Tip Güvenliği Azalması:` Mixed types kullanıldığında, belirli bir tür beklentisi olmadan çalışmak zor olabilir dolayısıyla hata ayıklamayı zorlaştırabilir.
+ `Dokümantasyon ve Anlaşılabilirlik:` Fonksiyonların döndürdüğü veya kabul ettiği türlerin açıkça belirtilmemesi, kodun anlaşılabilirliğini azaltabilir

> `mixed types`, PHP'de bir değişkenin birden fazla türde değer alabileceğini ifade eder ve fonksiyonların daha esnek olmasını sağlar. Ancak, bu esneklik tip güvenliğini azaltabilir, bu yüzden dikkatli kullanılmalıdır. Özellikle dinamik veri yapıları ve çeşitli türlerde veri döndüren fonksiyonlar için kullanışlıdır. Katı tip kontrolü ile birlikte kullanıldığında bile, mixed types geniş bir data türü yelpazesini kabul eder.

#### Union types vs Mixed Types 
+ Union types ve mixed types, PHP'de bir değişkenin birden fazla türde olabileceğini belirtmek için kullanılan iki farklı yaklaşımdır, ancak aralarında önemli farklar vardır.
  
+ Tip Güvenliği
  - `Union Types:` Daha fazla tip güvenliği sağlar çünkü hangi türlerin kabul edildiği ve döndürüldüğü açıkça belirtilir. Bu, yanlış türdeki data'ların kullanılmasını engeller.
  - `Mixed Types:` Daha az tip güvenliği sağlar çünkü herhangi bir türdeki data kabul edilir. Bu, tip uyumsuzluklarının tespit edilmesini zorlaştırabilir.

+ Kullanım Amacı
  - `Union Types:` Belirli birkaç türde data kabul eden ve döndüren fonksiyonlar ve değişkenler için idealdir. Tiplerin net olarak belirlenmesi gerektiğinde kullanılır.
  - `Mixed Types:` Çok çeşitli türlerde data kabul eden ve döndüren fonksiyonlar ve değişkenler için kullanışlıdır. Dinamik ve esnek data yapıları ile çalışırken tercih edilir.

+ Okunabilirlik ve Anlaşılabilirlik
  - `Union Types:` Kodun okunabilirliğini ve anlaşılabilirliğini artırır çünkü hangi türlerin kabul edildiği ve döndürüldüğü nettir.
  - `Mixed Types:` Kodun anlaşılmasını zorlaştırabilir çünkü bir değişkenin veya fonksiyonun hangi türde veri ile çalıştığı belirsiz olabilir.
