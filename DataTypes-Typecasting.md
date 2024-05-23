***
### Data types nedir?
***
+ PHP'de data types, değişkenlerde saklanan verilerin türlerini tanımlar. PHP, geniş bir yelpazede veri tiplerini destekler ve dinamik olarak tip belirlemeyi kullanır. İşte PHP'deki temel veri tipleri:
1. Scalar (Skaler) Data Types
+ Integer (Tam Sayı)
  - Tam sayıları temsil eder.
  - 32-bit sistemlerde ```-2,147,483,648``` ile ```2,147,483,647```, 64-bit sistemlerde ise çok daha büyük bir aralıkta değer alabilir.
~~~~~~~
$age = 30;
$negativeNumber = -10;
~~~~~~~

+ Float (Kayan Nokta Sayı)
  - Ondalıklı sayıları temsil eder.
~~~~~~~
$price = 19.99;
$pi = 3.14159;
~~~~~~~

+ String (Dizi)
  - Karakter dizilerini temsil eder.
  - Çift tırnak ```"``` veya ```'``` içinde yazılır.
~~~~~~~
#greeting = "Hello, World!";
$name = "Zehra";
~~~~~~~

+ Boolean (Mantıksal)
  - İki değer alabilir: ```true``` veya ```false```.
~~~~~~~
$isAdmin = true;
$isLoggedIn = false;
~~~~~~~

~~~~~~~
$greeting = "Little Ailurophile";

echo $greeting."<br/>";
var_dump($greeting);
~~~~~~~
> ```var_dump``` fonksiyonu, bir değişkenin türü ve değeri hakkında bilgi verir. Yukarıdaki örnekte ```$greeting``` değişkeninin türü string, 18 karakter uzunluğunda ve "Little Ailurophile" değerine sahiptir.

2. Compund (Bileşik) Data Types
+ Array (Dizi)
  - Birden fazla değeri tek bir değişkende saklamak için kullanılabilir.
  - İndeksli veya anahtar-değer çiftleri olarak saklanabilir.
~~~~~~~
$numbers = array(1, 2, 3, 4, 5);
$person = array("name" => "Zehra", "age" => 29);
~~~~~~~

+ Object (Nesne)
  - Class'lara dayalı olarak oluşturulan veri tipidir.
  - Bir class, properties ve method'lar içerir.
~~~~~~~
class Car {
  public $make;
  public $model;

  public function __construct($make, $model) {
    $this->make = $make;
    $this->model = $model;
  }

  public function display() {
    echo "Car: ". $this->make." ".$this->model;
  }
}

$car = new Car("Volvo", "XC90");
$car->display();
~~~~~~~

+ Callable (Çağırılabilir)
  - Bir değişkenin veya bir ifadenin çağırılabilir bir şey (yani bir fonksiyon, yöntem veya anonim fonksiyon) olduğunu belirtir.
  - Bir değişkenin fonksiyon gibi kullanılabiliceği anlamına gelir.

##### Örnekler
- Anonim Fonksiyon (Anonymous Function)
~~~~~~~
$func = function($name) {
  return "Hello, $name!";
}

echo $func("World");
~~~~~~~
> Output: Hello, World!

- İsimli Fonksiyon (Named Function)
~~~~~~~
function greet($name) {
  return "Hello $name!";
}

$func = "greet";
echo $func("World");
~~~~~~~
> Output: Hello, World!

- Sınıf Yönetimi (Class Method)
~~~~~~~
class Greeter {
  public static function greet($name) {
    return ("Hello, $name!)";
  }
}

$func = ["Greeter", "greet"];
echo call_user_func($func, "World");
~~~~~~~
> Hello, World!
> + ```Greeter``` adında bir class tanımlanmıştır.
> + ```public static function greet($name) {...}``` ifadesi, class içinde statik bir yöntem tanımlar, bir ad alır ve ```Hello, [ad]!``` mesajını döner.
> + ```$func = ["Greeter", "greet"];``` ifadesi bir callable bir değişkenidir.
>   - ```$func``` değişkeni bir array olarak tanımlanmıştır.
>   - ```Greeter``` sınıfın adı, ```greet``` ise sınıfın içinde çağırılacak yöntemdir.
> + ```call_user_func``` fonksiyonu kullanılarak callable değişkeni çağırılır.
>   - ```call_user_func($func, "World");``` ifadesi, ```$func``` içinde tanımlanan yöntem olan ```Greeter::greet``` yöntemini çağırır ve ```World``` argümanını bu yönteme geçirir.

+ Iterable (Yinelenebilir)
  - ```itrable```, üzerinde yineleme (iteration) yapılabilecek bir veri tipidir. Bir diziyi (array) veya ```Traversable``` arayüzünü (interface) implemente eden bir class'ı içerebilir.

##### Örnekler
- Dizi (Array)
~~~~~~~
function printıterable(iterable #myIterable) {
  foreach($myIterable as $item) {
    echo $item;
  }
}

$array = ["a", "b", "c"];
printIterable($array);
~~~~~~~
> Output: abc

~~~~~~~
$companies = [1, 2, 3, 0.5, -9.2, "A", "B", true];
print_r($companies);
~~~~~~~
> Yukarıdaki örnekte ```print``` yerine ```print_r``` kullanılmıştır. Arasındaki fark nedir?
> + Basit Metin ve Değişkenler:
>   - ```print``` düz metin ve değişkenlerin "string" temsilini yazdırmak için uygundur.
>   - ```print_r``` ise bu tür basit değerleri yazdırmak için gereksiz detay ekler.
> + Diziler ve Nesneler:
>   - ```print``` sadece "array" veya "object" gibi basit bir bilgi verir.
>   - ```print_r``` dizilerin ve nesnelerin içeriğini ve yapısını detaylı bir şekilde gösterir.

- Iterator Arayüzü Kullanan Sınıf
~~~~~~~
class MyIterator implements Iterator {
  private $items = [];
  private $index = 0;

  public function __construct($items) {
    $this-> $items;
  }

  public function current() {
    return $this->item[$this->index];
  }

  public function key() {
    return $this->index;
  }

  public function next() {
    $this->index++;
  }

  public function rewind() {
    $this->index = 0;
  }

  public function valid() {
    return isset($this->items[$this->index]);
  }
}

$item = ["a", "b", "c"];
$iterator = new MyIterator($items);
printIterable($iterator);
~~~~~~~
> Output: abc
> + ```MyIterator``` class'ı, ```Iterator``` arabirimini uygular. Bu ```current()```, ```key()```, ```next()```, ```rewind()``` ve ```valid()``` method'larının tanımlanmasını gerektirir.
> + ```$items``` iterator'un üzerinde yinelenecek öğelerin depolandığı dizi, ````$index``` mevcut konumu belirten bir index'tir.
> + ```__construct()```, class'ın kurucu method'udur. ```$items``` adlı bir dizi alır ve class'ın ```$items``` özelliğine bu diziyi atar.
> + ```current()```, mevcut öğeyi döndürür. ```$index``` kullanılarak ```$items``` dizisinden mevcut öğe alınır.
> + ```key()```, mevcut konumu temsil eden anahtarı döndürür. Sadece mevcut index'i döndürür.
> + ```next()```, iterator'deki konumu bir sonraki öğeye ilerletir. ```$index``` değerini artırarak yapılır.
> + ```rewind()```, iterator'ü başlangıç konumuna geri sarar. ```$index``` değerini sıfırlayarak yapılır.
> + ```valid()```, iterator'un geçerli bir öğeye sahip olup olmadığını kontrol eder. Mevcut index'in ```$items``` dizisinde tanımlı olup olmadığını kontrol eder.
> + ```$items``` dizisi ve ```MyIterator``` class'ı kullanılarak bir iterator oluşturulur.
> + ```printItrable()``` işlevi çağırılır ve iterator geçirilir. Bu işlev, iteratörü baştan sona dolaşır ve her öğeyi ekrana yazdırır.

> + ```callable```, dinamik fonksiyon çağrıları, geri çağırma fonksiyonları (callback functions), olay işleyicileri (event handlers) ve fonksiyon referansları için kullanılır.
> + ```iterable```, döngüler ve yineleme işlemleri, veri kümeleri üzerinde gezinme işlemleri ve koleksiyon yönetimi için kullanılır.

3. Special (Özel) Data Types
+ Null
  - Boş bir değeri temsil eder.
  - Bir değişkenin değeri ````NULL``` ise, o değişken tanımlı ama içi boştur.
~~~~~~~
$value = NULL;
~~~~~~~

+ Resource (Kaynak)
  - Harici bir kaynağa referans içerir.
  - Veritabanı bağlantıları, dosya işlemleri gibi işlemlerde kullanılır.
~~~~~~~
$file = fopen("text.txt", "r");
~~~~~~~

***
#### Data Type'ları Belirleme ve Kontrol Etme
***
+ gettype()
  - Bir değişkenin veri tipini döner.
~~~~~~~
$x = 10;
echo gettype($x);
~~~~~~~
> Output: integer

+ is_*
  - Belirli bir veri tipi olup olmadığını kontrol eder.
~~~~~~~
$x = 10.5;

if(is_int($x)) {
  echo "x is a inteher";
} elseif (is_float($x)) {
  echo "x is a float"
} 
~~~~~~~
> Output: x is a float.

***
### Typecasting (veri dönüştürme) nedir?
***
+ Genellikle bir değişkenin başına parantez içinde hedef veri türü yazılarak yapılır.
+ Veri dönüştürme yapılırken dikkat edilmesi gereken hususlar vardır:
  - ```Veri Kaybı:``` Typecasting sırasında veri kaybı olabilir. Örneğin bir ```float``` değeri, ```int```'e dönüştürülürken ondalık kısmı kaybolur.
    ~~~~~~~
    $float = 12.99;
    $int = (int)$float;
    echo $int;
    ~~~~~~~
    > Output: 12
    
  - ```Boole Değerleri:``` Boole tipine dönüştürüldüğünde, ```0```, ```""``` (boş string), ```null```, ```array()``` (boş dizi) ve ```false``` olur. ```Diğer tüm değerler "true" olur.```
    ~~~~~~~
    $val = 0;
    $bool = (bool)$val;
    echo $bool;
    ~~~~~~~
    > Output: false
    
  - ```String Dönüşümleri:``` Bir ```string``` sayısal değere dönüştürülürken, sayısal olmayan karakterler dönüşümde göz ardı edilir.
    ~~~~~~~
    $str = "123abs";
    $int = (int)$str;
    echo $int;
    ~~~~~~~
    > Output: 123
    
  - ```Array ve Object Dönüşümleri:``` Bir ```array```, ```object```'e dönüştürüldüğünde, dizi anahtarları nesne özelliklerine dönüşür.
    ~~~~~~~
    $arr = ["key" => "value"];
    $obj = (object)$arr;
    echo $obj-> key;
    ~~~~~~~
    > Output: value
    > - ```PHP'de bir diziyi nesneye dönüştürmek, dinamik olarak nesne oluşturmanın bir yoludur.``` Bu yöntem, özellikle dizilerden elde edilen verilerle çalışırken yararlıdır. Dizinin anahtarları nesne özelliklerine dönüşür ve dizi değerleri bu özelliklerin değerleri olur. Bu, JSON verilerini işlemek veya API'den gelen verileri kullanmak gibi durumlarda oldukça faydalıdır.

***
### Tip Juggling nedir?
***
+ PHP dinamik bir dil olduğu için, veri türleri arasındaki otomatik olarak dönüşüm yapabilir. Buna ```Tip Juggling``` denir. Örneğin, sayısal bir değeri ```string``` ile toplarken, PHP sayısal değeri otomatik olarak ```string```'e dönüştürür.
+ Değişkenlerin veri türlerini kontrol etmek ve gerektiğinde dönüştürmek, kodun doğru çalışmasını sağlamanın önemli bir parçasıdır. Typecasting kullanırken, veri kaybı ve beklenmeyen sonuçlar gibi olası sorunlara dikkat edilmelidir.
~~~~~~~
$num = 5;
$str = "10;
$result = $num + $str;

echp $result;
~~~~~~~
> Output: 15
