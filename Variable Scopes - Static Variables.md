***
### Variable scope (değişkenlerin kapmsamı) nedir?
***
+ Bir değişkenin hangi kod blokları içinde geçerli ve erişilebilir olduğunu belirler.
+ PHP'de değişken kapsamı dört ana kategoriye ayrılır: global, local, static ve function parameters (fonksiyon parametreleri)

----
1. Global scope (global kapsam) nedir?
+ Bir değişken, fonksiyonların veya class'ların dışında tanımlandığında global kapsamda olur.
+ Bu değişkenler, PHP dosyasının herhangi bir yerinden erişilebilir. Ancak, bir global değişkene bir fonksiyon içinden doğrudan erişmek için `global` keyword'ü veya `$GLOBALS` kullanılmalıdır.
  
###### Global Keyword
~~~~~~~
$y = 20; // $y in global scope

function bar() {
    global $y; // $y accesses the global variable
    echo $y;
}

bar();
~~~~~~~
> Output: 20

###### $GLOBALS
~~~~~~~
$y = 20; // $y in global scope

function bar() {
    echo $GLOBALS['y'];
}

bar();
~~~~~~~
> Output: 20

----
2. Local scope (yerel kapsam) nedir?
+ Bir değişken, bir fonksiyon veya blok içinde tanımlandığında yerel kapsamda olur.
+ `Bu değişkenler sadece tanımlandıkları fonksiyon veya blok içinde geçerlidir ve dışında erişilemezler.`
~~~~~~~
function foo() {
    $x = 10; // $x in local scope
    echo $x; // Output: 10
}

foo();
echo $x;
~~~~~~~
> Output: Error: Undefined variable $x

##### Local ve Global Scope
~~~~~~~
$inGlobal = "I'm a global variable.";

function testScopes() {
    $inLocal = "I'm a local variable.";
    echo $inLocal;             // Output: Ben yerel bir değişkenim.
    echo $GLOBALS['inGlobal']; // Output: I'm a global variable.
}

testScopes();
echo $inGlobal;
echo $inLocal;
~~~~~~~
> Output: I'm a global variable.

> Output: Error: Undefined variable $inLocal

----
3. Static scope (statik kapsam) nedir?
+ Bir değişkenin değerini bir fonksiyon çağrıları arasında korumasını sağlamak için static keyword'ü kullanılır.
+ `Statik değişkenler, fonksiyon her çağrıldığında yeniden başlatılmaz ve değerlerini korurlar.`
~~~~~~~
function counter() {
    static $count = 0; // $count static variable
    $count++;
    echo $count;
}

counter();
counter();
counter();
~~~~~~~
> Output: 1

> Output: 2

> Output: 3

----
4. Function Parameters (fonksiyon parametreleri) nedir?
+ Fonksiyon tanımında belirtilen ve fonksiyon çağrıldığında değer atanan değişkenlerdir.
+ `Bu parametreler sadece fonksiyon içinde geçerlidir.`
~~~~~~~
function greet($name) {
    echo "Hello, $name!";
}

greet("Zehra");
~~~~~~~
> Output: Hello, Zehra!

***
### Using global keyword to access variables in the global scope (global kapsmadaki değişkenlere erişmek için global keyword'ü kullanımı) nedir?
***

##### Global Keyword Kullanımı
+ Global değişkenlere fonksiyon içinden erişmek için `global` keyword'ü kullanarak o değişkeni fonksiyon içinde de global kapsamda kullanıma açılabilir.

1. Global Değişken Tanımlama ve Erişim
~~~~~~~
$message = "Hello, World!"; // defined variable in global scope

function printMessage() {
    global $message; // global keyword to use the global variable
    echo $message;
}

printMessage();
~~~~~~~
> Output: Hello, World!

 2. Global Değişkeni Güncelleme
~~~~~~~
$count = 0; // defined variable in global scope

function incrementCount() {
    global $count; // global keyword to use the global variable
    $count++; // update variable
}

incrementCount();
echo $count;
incrementCount();
echo $count;
~~~~~~~
> Output: 1

> Output: 2

##### $Globals Array Kullanımı
+ Global keyword yerine `$GLOBALS süper global array`'i de kullanılabilir. `$GLOBALS`, tüm global değişkenleri bir array olarak tutar ve global değişkenlere erişim sağlar.

1. $GLOBALS Kullanımı
~~~~~~~
$message = "Hello, World!"; // defined variable in global scope

function printMessage() {
    echo $GLOBALS['message']; // access with $GLOBALS super global array
}

printMessage();
~~~~~~~
> Output: Hello, World!

2. $GLOBALS Kullanarak Değişkeni Güncelleme
~~~~~~~
$count = 0; // defined variable in global scope

function incrementCount() {
    $GLOBALS['count']++; // update with $GLOBALS super global array
}

incrementCount();
echo $count;
incrementCount();
echo $count;
~~~~~~~
> Output: 1

> Output: 2

#### $GLOBALS Süper Global Array'inin Avantajları ve Dezavantajları

##### Avantajları
+ `Evrensel Erişim:` Betiğin herhangi bir yerinden global değişkenlere erişim sağlar, bu da kodun okunabilirliğini ve yönetilebilirliğini artırır.
+ `DEğişken Adlarıyla Erişim:` Değişken adlarıyla erişim imkanı sağlar, bu da dinamik değişken erişimini kolaylaştırır.

##### Dezavantajları
+ `Okunabilirlik:` Kodun okunabilirliğini azaltabilir çünkü `$GLOBALS` array'i ile hangi değişkenlerin kullanıldığını izlemek zor olabilir.
+ `Yan Etki Riski:` `$GLOBALS` array'ini kullanarak global değişkenleri değiştirmek, beklenmedik yan etkilere neden olabilir, bu yüzden dikkatli kullanılmalıdır.

#### Global Keyword vs. $Globals Array
+ `Kapsam ve Kapsama Alma:`
    - `global` keyword, bir fonksiyon içinde belirli değişkenleri global kapsamda kullanılabilir hale getirir.
    - `$GLOBALS` ise tüm global değişkenlere doğrudan erişim sağlar.

+ `Kullanım Kolaylığı:`
    - `global` keyword, sadece belirli değişkenlerin global olarak kullanılmasını sağlar ve bu, kodun okunabilirliğini artırabilir.
    - `$GLOBALS` ile çalışırken, tüm global değişkenler bir dizi üzerinden erişilebilir.

***
### How are global variables stored & GLOBALS superglobal (global değişkenler nasıl saklanır & GLOBALS superglobal)?
***
+ PHP'de global değişkenler, bir fonksiyon veya class'on dışında tanımlanan ve tüm betik boyunca erişilebilir olan değişkenlerdir. Bu değişkenler, betik çalıştığı sürece bellekte tutulur ve global kapsamda erişilebilir.
+ `$GLOBALS`, PHP'nin süper global array'lerinden biridir ve tüm global değişkenleri içeren bir array'dir. Bu array, betik boyunca herhangi bir yerden global değişkenlere erişim sağlar. `$GLOBALS array'i, her global değişkeni key-value çiftleri şeklinde tutar, key olarak değişkenin adı ve value olarak değişkenin değeri kullanılır.`
+ Global değişkenler, PHP'nin içsel mekanizması tarafından global bir alanda depolanır. Bu alana erişmek için global keyword veya $GLOBALS array'i kullanılır.

***
### Static variables nedir?
***
+ Bir fonksiyon içinde tanımlandığında, o fonksiyonun yaşam süresi boyunca değerini koruyan ve fonksiyon her çağrıldığında aynı değeri hatırlayan değişkenlerdir.
+ Statik değişkenler, fonksiyon çağrıları arasında veri saklamak için kullanılır ve sadece fonksiyonun ilk çağrısında başlatılır. Sonraki çağrılarda, önceki değeri korunur.
+ `Özellikle sayaçlar, durum bilgileri veya fonksiyon çağrıları arasında veri tutmak için kullanışlıdır.`

##### Özellikleri
+ `Başlangıç Değeri:` Statik değişkenler, bir fonksiyon içinde sadece bir kez başlatılır.
+ `Değer Koruma:` Fonksiyon her çağrıldığında değerini korur ve hatırlar.
+ `Yerel Kapsam:` Statik değişkenler, tanımlandıkları fonksiyonun dışında erişilemez.

###### Örnekler
1. Statik Değişken Kullanımı
~~~~~~~
function counter() {
    static $count = 0; // define static variable
    $count++;
    echo $count;
}

counter();
counter();
counter();
~~~~~~~
> Output: 1

> Output: 2

> Output: 3

> + `$count` değişkeni statik olarak tanımlandığı için fonksiyon her çağrıldığında değeri korunur ve artırılır.

2. Yerel ve Statik Değişkenlerin Karşılaştırılması
~~~~~~~
function localVariable() {
    $count = 0; // define local variable
    $count++;
    echo $count;
}

localVariable();
localVariable();
localVariable();
~~~~~~~
> Output: 1

> Output: 1

> Output: 1

> + `$count` yerel bir değişken olarak tanımlandığı için fonksiyon her çağrıldığında sıfırlanır ve değeri yeniden 1 olur.

##### Statik Değişkenlerin Kullanım Alanları
1. Sayaçlar
+ Bir fonksiyonun kaç kez çağrıldığını takip etmek için kullanılabilir.
~~~~~~~
function callCounter() {
    static $count = 0;
    $count++;
    echo "The function has been called $count times.\n";
}

callCounter();
callCounter();
~~~~~~~
> Output: The function has been called 1 times.

> Output: The function has been called 2 times

2. Durum Bilgisi Saklama
+ Bir fonksiyonun önceki durumunu saklamak için kullanılabilir.
~~~~~~~
function fibonacci() {
    static $prev1 = 0, $prev2 = 1;
    $current = $prev1 + $prev2;
    $prev1 = $prev2;
    $prev2 = $current;
    return $current;
}

echo fibonacci();
echo fibonacci();
echo fibonacci();
echo fibonacci();
~~~~~~~
> Output: 1

> Output: 2

> Output: 3

> Output: 5

> + `fibonacci fonksiyonu, Fibonacci array'ini hesaplamak için önceki iki değeri saklamak amacıyla statik değişkenler kullanır.

> Statik değişkenler, fonksiyonların durumsal bilgilerini saklamak veya fonksiyon çağrıları arasında veri tutmak için kullanışlıdır. Bu, fonksiyonların daha verimli ve işlevsel olmasını sağlar.
