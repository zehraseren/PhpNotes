***
### Variable scope (değişkenlerin kapmsamı) nedir?
***
+ Bir değişkenin hangi kod blokları içinde geçerli ve erişilebilir olduğunu belirler.
+ PHP'de değişken kapsamı dört ana kategoriye ayrılır: global, local, static ve function parameters (fonksiyon parametreleri)

----
1. Global scope (global kapsam) nedir?
+ Bir değişken, fonksiyonların veya class'ların dışında tanımlandığında global kapsamda olur.
+ Bu değişkenler, PHP dosyasının herhangi bir yerinden erişilebilir. Ancak, bir global değişkene bir fonksiyon içinden doğrudan erişmek için `global` key veya `$GLOBALS` kullanılmalıdır.
  
###### Global Key
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
+ Bir değişkenin değerini bir fonksiyon çağrıları arasında korumasını sağlamak için static key kullanılır.
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
### Using global keyword to access variables in the global scope nedir?
***
+ 

#### Global Key Kullanımı
+
#####
~~~~~~~
~~~~~~~
> Output:
> +

##### $Globals Array Kullanımı
~~~~~~~
~~~~~~~
> Output:
> +

#### Global Key vs. $Globals Array
+ `:`
+ `:`

***
### How are global variables stored & GLOBALS superglobal nedir?
***

***
### Static variables nedir?
***
+
~~~~~~~
~~~~~~~
> Output:
> +







































