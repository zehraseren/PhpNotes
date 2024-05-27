***
### Operatörler nelerdir?
***
+ PHP'de çeşitli operatörler vardır ve bu operatörler, farklı işlemleri gerçekleştirmek için kullanılır. 

1. Aritmetic Operators | ```+ - * / % **```
----
+ Matematiksel işlemler yapmak için kullanılır.
  
+ `+` | Toplama: İki değeri toplar.
~~~~~~~
$x + $y
~~~~~~~
  
+ `-` | Çıkarma: Bir değeri diğerinden çıkarır.
~~~~~~~
$x - $y
~~~~~~~
  
+ `*` | Çarpma: İki değeri çarpar.
~~~~~~~
$x * $y
~~~~~~~
  
+ `/` | Bölme: Bir değeri diğerine böler.
~~~~~~~
$x / $y
~~~~~~~
  
+ `%` | Modülüs: Bir değerin diğerine bölümünden kalanı verir.
~~~~~~~
$x + $y
~~~~~~~
  
+ `**` | Üs Alma: Bir değerin üssünü alır.
~~~~~~~
$x + $y
~~~~~~~

2. Assigment (Atama) Operators | ```= += -= *= /= %= **=```
----
+ `=` | Atama: Sağdaki değeri sola atar.
~~~~~~~
$x = $y
~~~~~~~

+ `+=` | Toplamlı Atama: Değişkenin mevcut değerine bir değer ekler ve sonucu değişkene atar.
~~~~~~~
$x += $y
~~~~~~~

+ `-=` | Çıkarmalı Atama: Değişkenin mevcut değerinden bir değer çıkarır ve sonucu değişkene atar.
~~~~~~~
$x -= $y
~~~~~~~

+ `*=` | Çarpmalı Atama: Değişkenin mevcut değeriyle bir değeri çarpar ve sonucu değişkene atar.
~~~~~~~
$x *= $y
~~~~~~~

+ `/=` | Bölmeli Atama: Değişkenin mevcut değerini bir değere böler ve sonucu değişkene atar.
~~~~~~~
$x /= $y
~~~~~~~

+ `%=` | Modülüs Atama: Değişkenin mevcut değerini bir değere bölüp kalanını değişkene atar.
~~~~~~~
$x %= $y
~~~~~~~

3. String Operators | ```. .*=```
----
+ Esas olarak stringlerin birleştirilmesi ve atamaları için kullanılır.

+ `.` | Concatenation (Birleştirme): İki veya daha fazla stringi birleştirmek için kullanılır.
~~~~~~~
$str1 = "Hello";
$str2 = " World";

echo $result = $str1.$str2;
~~~~~~~
> Output: Hello World

+ `.=` | Concatenation Assigment(Birleştirme ve Atama): Bir stringe başka bir stringi eklemek için kullanılır ve sonucu tekrar aynı değişkene atar.
~~~~~~~
$str = "Hello";
echo $str .= " World";
~~~~~~~
> Output: Hello World

+  Bu iki operatör, PHP'de string manipülasyonu için en yaygın kullanılan operatörlerdir. Ayrıca, PHP string'ler üzerinde birçok yerleşik fonksiyon sağlar, ancak bunlar operatörler değil fonksiyonlar olarak sınıflandırılır.
  
  - Concatenation
~~~~~~~
$greeting = "Hello";
$target = " World!";

$fullGreeting = $greeting.$target;
echo $fullGreeting; 
~~~~~~~
> Output: Hello World!

  - Concatenation Assigment
~~~~~~~
$fullGreeting .= " How are you?";
echo $fullGreeting;
~~~~~~~
> Output: Hello World! How are you?

4. Comprasion (Karşılaştırma) Operators | ```== === != <> !== < > <= >= <=> ?? ?:```
----
+ İki değeri karşılaştırmak için kullanılır ve boolean (doğru veya yanlış) sonuçlar döner.

+ `==` | Eşittir: İki değerin eşit olup olmadığını kontrol eder.
~~~~~~~
$x == $y;
~~~~~~~

+ `===` | Özdeş: İki değerin ve türünün eşit olup olmadığını kontrol eder.
~~~~~~~
$x === $y;
~~~~~~~

+ `!= | <>` | Eşit Değil: İki değerin eşit olmadığını kontrol eder.
~~~~~~~
$x != $y;
~~~~~~~

+ `!==` | Özdeş Değil: İki değerin ve türünün eşit olmadığını kontrol eder.
~~~~~~~
$x !== $y;
~~~~~~~

+ `<` | Küçüktür: Sol değerin sağ değerden küçük olup olmadığını kontrol eder.
~~~~~~~
$x < $y;
~~~~~~~

+ `>` | Büyüktür: Sol değerin sağ değerden büyük olup olmadığını kontrol eder.
~~~~~~~
$x > $y;
~~~~~~~

+ `<=` | Küçük Eşittir: Sol değerin sağ değerden küçük veya eşit olup olmadığını kontrol eder.
~~~~~~~
$x <= $y;
~~~~~~~

+ `>=` | Büyük Eşittir: Sol değerin sağ değerden büyük veya eşit olup olmadığını kontrol eder.
~~~~~~~
$x >= $y;
~~~~~~~

+ `<=>` | Spaceship (Uzay Gemisi): İki değeri karşılaştırmak için kullanılır ve üç farklı durumu temsil eden bir değer döndürür:
    - Eğer sol operand sağ operand'dan küçükse, `-1` döndürür.
    - Eğer sol operand sağ operand'dan büyükse, `1` döndürür.
    - Eğer sol operand sağ operand'a eşitse, `0` döndürür.
~~~~~~~
$x <=> $y;
~~~~~~~

##### <=> operatörünün uygulama alanları nelerdir?
1. Sıralama Fonksiyonları
+ Sıralama algoritmalarında özellikle kullanışlıdır.
+ `usort`, `uasort` ve `uksort` gibi kullanıcı tanımlı sıralama fonksiyonlarında sıkça kullanılır.
~~~~~~~
$array = [3, 2, 5, 1, 4];

usort($array, function($a, $b) {
    return $a <=> $b;
});

print_r($array);
~~~~~~~
> Output: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )

2. Genel Karşılaştırmalar
+ İki değeri karşılaştırmak ve sonucu bir değişkende saklamak gerektiğinde kullanışlıdır.
~~~~~~~
function compare($a, $b) {
    return $a <=> $b;
}

echo compare(3, 4); 
echo compare(4, 3); 
echo compare(4, 4); 
~~~~~~~
> Output: -1

> Output: 1

> Output: 0

##### Kullanım ipuçları
+ `Karmaşık Mantık:` Karmaşık karşılaştırma mantıklarını basitleştirmek için kullanabilir.
+ `Performans:` <=> operatörü, tek bir karşılaştırma işleminde üç farklı durumu kontrol ettiği için performans açısından etkilidir.

+ `??` | Null Birleştirme: Değerin null olup olmadığını kontrol eder ve null değilse değeri döner.
~~~~~~~
$x ?? $y;
~~~~~~~

+ `?:` | Ternary: Koşullu işlemler için kullanılır.
~~~~~~~
$x ? $y : $z
~~~~~~~

5. Error Control Operators | ```@```
----
~~~~~~~
~~~~~~~
> Output:

6. Increment/Decrement Operators | ```++ --```
----
~~~~~~~
~~~~~~~
> Output:

7. Logical (Mantıksal) Operators | ```&& || ! and or xor```
----
~~~~~~~
~~~~~~~
> Output:

8. Bitsel (Bit düzeyi) Operators | ```& | ^ ~ << >>```
----
~~~~~~~
~~~~~~~
> Output:

9. Array Operators | ```+ == === != <> !==```
----
~~~~~~~
~~~~~~~
> Output:

10. Execution (Yürütme) Operators | ``` `` ```
----
~~~~~~~
~~~~~~~
> Output:

11. Type Operators | ```instanceof```
----
~~~~~~~
~~~~~~~
> Output:

12. Nullsafe Operators | ``````
----
~~~~~~~
~~~~~~~
> Output:

