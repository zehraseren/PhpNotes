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
+ Bir ifadenin önüne konulduğunda, o ifadede meydana gelen hata mesajlarının görüntülenmesini engeller.
+ Özellikle hata mesajlarının kullanıcıya gösterilmesini istemediğiniz durumlarda kullanışlı olabilir. `Ancak dikkatli kullanılmadığında, hata yönetimi ve hata ayıklama süreçlerini zorlaştırabilir.`
##### Örnekler
  + Dosya Okuma
~~~~~~~
$file = @file('non_existing_file.txt');

if ($file === false) {
    echo 'Dosya okunamadı.';
}
~~~~~~~
> `non_existing_file.txt` dosyası mevcut değilse normalde bir hata mesajı alınır, ancak `@ operatörü` sayesinde bu hata mesajı bastırılır ve sadece false değeri döner.

  + Database Bağlantısı
~~~~~~~
$connection = @mysqli_connect('localhost', 'user', 'password', 'database');

if(!connection) {
  echo "Database connection failed.";
}
~~~~~~~
> Database bağlantısı başarısız olursa normalde hata mesajı çıkacaktır. `@ operatörü` bu hatayı bastırarak sadece if koşuluyla başarısızlığı kontrol etmenizi sağlar.

##### Dikkat edilmesi gerekenler
+ `Performans:` @ operatörü kullanıldığında, hataları kontrol etmek için fazladan işlem yapar ve bu performansı olumsuz etkileyebilir.
  
+ `Hata Ayıklama:` Hataların bastırılması, koddaki sorunları tespit etmeyi zorlaştırabilir. Dolayısıyla, geliştirici ortamında veya hata ayıklama sırasında `@` operatöründen kaçınılmalıdır.
  
+ `Hata Yönetimi:` Daha iyi bir hata yönetimi için özel hata işleme mekanizmaları (try-catch blokları, özel hata işleme fonksiyonları gibi) kullanmak genellikle daha iyi bir yaklaşımdır.
~~~~~~~
try {
    $file = new SplFileObject('non_existing_file.txt');
} catch (RuntimeException $e) {
    echo 'Dosya okunamadı: ' . $e->getMessage();
}
~~~~~~~

6. Increment/Decrement Operators | ```++ --```
----
+ `++$x` | Ön Artırma: Değişkenin değerini önce bir artırır, sonra değeri kullanır.

+ `$x++` | Sonra Artırma: Değişkenin mevcut değerini kullanır, sonra bir artırır.

+ `--$x` | Ön Artırma: Değişkenin değerini önce bir azaltır, sonra değeri kullanır.

+ `$x--` | Sonra Artırma: Değişkenin mevcut değerini kullanır, sonra bir azaltır.

7. Logical (Mantıksal) Operators | ```&& || ! and or xor```
----
+ Mantıksal işlemler yapmak için kullanılır ve boolean sonuçlar döner.
+ `&&` | Ve: İki koşulun da doğru olup olmadığını kontrol eder.
~~~~~~~
$x && $y
~~~~~~~

+ `and` | Ve: `&&` ile aynıdır, ancak öncelik farklıdır.
~~~~~~~
$x and $y
~~~~~~~

##### Farkı
+ `&&` operatörü, yüksek öncelikli bir mantıksal `AND` operatörüdür. Genellikle diğer operatörlerden önce değerlendirilir.
~~~~~~~
$x = true && false;
~~~~~~~
> Output: false
> + `true && false` ifadesi önce değerlendirilir ve sonuç `false` olur. Dolayısıyla `$a` değişkeni `false` değerini alır.

+ `and` operaötürü, düşük öncelikli bir mantıksal `AND` operatörüdür. Atama operatörü `=` gibi diğer düşük öncelikli operatörlerden sonra değerlendirilir.
~~~~~~~
$x = true and false;
~~~~~~~
> Output: true
> + Yukarıdaki örnekte, atama işlemi `=` önce değerlendirilir, bu nedenle `$a` önce `true` olarak atanır. Daha sonra true and false ifadesi değerlendirilir, ancak bu sonuç atama işleminden sonra geldiği için `$a değeri true olarak kalır`.

+ `||` | Veya: İki koşuldan birinin doğru olup olmadığını kontrol eder.
~~~~~~~
$x || $y
~~~~~~~

+ `or` | Veya: `||` ile aynıdır, ancak öncelik farklıdır.
~~~~~~~
$x or $y
~~~~~~~

##### Farkı
+ `||` operatörü, yüksek öncelikli bir mantıksal OR operatörüdür. Genellikle diğer operatörlerden önce değerlendirilir.
~~~~~~~
$x = true || false;
~~~~~~~
> Output: true
> + `true || false` ifadesi önce değerlendirilir ve sonuç `true` olur. Dolayısıyla `$a` değişkeni `true` değerini alır

+ `or` operatörü, yüksek öncelikli bir mantıksal OR operatörüdür. Atama operatörü `=` gibi diğer düşük öncelikli operatörlerden sonra değerlendirilir.
~~~~~~~
$x = true or false;
~~~~~~~
> Output: true
> + Yukarıdaki örnekte, atama işlemi `=` önce değerlendirilir, bu nedenle `$a` önce `true` olarak atanır. Daha sonra true and false ifadesi değerlendirilir, ancak bu sonuç atama işleminden sonra geldiği için `$a değeri true olarak kalır`.

+ `!` | Değil: Bir koşulun doğru olmadığını kontrol eder.
~~~~~~~
!$x
~~~~~~~

+ `xor` | Özel Veya: İki koşuldan yalnızca birinin doğru olup olmadığını kontrol eder.
~~~~~~~
$x xor $y
~~~~~~~

8. Bitsel (Bit düzeyi) Operators | ```& | ^ ~ << >>```
----
+ `&` | And: Her iki operandın da ilgili bitlerinin 1 olduğu bitlerde 1 döner.
~~~~~~~
$x = 5; // 5 = 0101
$y = 3; // 3 = 0011
$z = $x & $y; // 0001

echo $z;
~~~~~~~
> Output: 1

+ `|` | Or: Her iki operandın da ilgili bitlerinden en az biri 1 ise, o bit 1 döner.
~~~~~~~
$x = 5; // 5 = 0101
$y = 3; // 3 = 0011
$z = $x | $y; // 0111

echo $z;
~~~~~~~
> Output: 7

+ `^` | Xor: Yalnızca operandların ilgili bitlerinden sadece biri 1 ise, o bit 1 döner.
~~~~~~~
$x = 5; // 5 = 0101
$y = 3; // 3 = 0011
$z = $x ^ $y; // 0110

echo $z;
~~~~~~~
> Output: 6

+ `~` | Not: Tüm bitleri ters çevirir (1'leri 0, 0'ları 1 yapar). İkili tam sayıların (signed integers) işaret bitine dikkat edilmelidir
~~~~~~~
$x = 5; // 5 = 0000 0101
$z = ~$y; // 1111 1010 (signed int olduğu için -6))

echo $z;
~~~~~~~
> Output: -6

+ `<<` | Sol Kaydırma: Bütün bitleri belirtilen sayı kadar sola kaydırır. Sağdan boşalan bitler 0 ile doldurulur.
~~~~~~~
$x = 5; // 0000 0101
$z = $y << 1; // 0000 1010

echo $z;
~~~~~~~
> Output: 10

+ `>>` | Sağ Kaydırma: Bütün bitleri belirtilen sayı kadar sağa kaydırır. Soldan boşalan bitler işaret biti (pozitifse 0, negatifse 1) ile doldurulur.
~~~~~~~
$x = 5; // 0000 0101
$z = $y >> 1; // 0000 0010

echo $z;
~~~~~~~
> Output: 2

9. Array Operators | ```+ == === != <> !==```
----
+ `+` | Birleştirme: İki diziyi birleştirir.
~~~~~~~
$x + $y
~~~~~~~

+ `==` | Eşittir: İki dizinin aynı key-değer çiftlerine sahip olup olmadığını kontrol eder.
~~~~~~~
$x == $y
~~~~~~~

+ `===` | Özdeş: İki dizinin aynı key-değer çiftlerine ve aynı sıraya sahip olup olmadığını kontrol eder.
~~~~~~~
$x === $y
~~~~~~~

+ `!= | <>` | Eşit Değil: İki dizinin aynı key-değer çiftlerine sahip olmadığını kontrol eder.
~~~~~~~
$x != $y
~~~~~~~

+ `!==` | Eşit Değil: İki dizinin aynı key-değer çiftlerine ve aynı sıraya sahip olmadığını kontrol eder.
~~~~~~~
$x + $y
~~~~~~~

10. Execution (Yürütme) Operators | ``` `` ```
----
+ Bir kabuk komutunu çalıştırmak için kullanılır. `shell_exec` fonksiyonuna benzer şekilde çalışır ve bir komut satırı komutunu çalıştırır, ardından bu komutun çıktısını döner.
~~~~~~~
$output = `ls -l`;
echo "<pre>$output</pre>";
~~~~~~~

11. Type Operators | ```instanceof```
----
+ Genellikle belirli bir değerin veya değişkenin türünü (type) kontrol etmek veya belirlemek için kullanılan operatörler ve fonksiyonlar kümesini ifade eder.
+ PHP'de doğrudan "type operator" olarak adlandırılan bir operatör yoktur, ancak tür kontrolü ve tür dönüşümü yapmak için kullanılan bazı operatörler ve fonksiyonlar vardır.
    - is_int(): Bir değişkenin tamsayı (integer) olup olmadığını kontrol eder.
    - is_float(): Bir değişkenin kayan nokta (float) sayı olup olmadığını kontrol eder.
    - is_string(): Bir değişkenin dize (string) olup olmadığını kontrol eder.
    - is_bool(): Bir değişkenin boolean (true/false) olup olmadığını kontrol eder.
    - is_array(): Bir değişkenin dizi (array) olup olmadığını kontrol eder.
    - is_object(): Bir değişkenin nesne (object) olup olmadığını kontrol eder.
    - is_null(): Bir değişkenin null olup olmadığını kontrol eder.
    - is_resource(): Bir değişkenin kaynak (resource) olup olmadığını kontrol eder.
    - is_callable(): Bir değişkenin çağrılabilir (callable) olup olmadığını kontrol eder.

12. Nullsafe Operators | `?->`
----
+ Bir nesne zincirinde null kontrollerini daha kolay ve temiz bir şekilde yapılmasına olanak tanır.
+ Bir nesne veya alt nesne null olduğunda hataya yol açmadan işlemi güvenli bir şekilde sonlandırır ve null döner.
+ `Nullsafe operatörü, bir nesne zincirinde herhangi bir noktada null değeri olup olmadığını kontrol eder. Eğer nullsa, işlemi durdurur ve null döner. Aksi halde, zincirin geri kalanını yürütür.`
~~~~~~~
$person = null;
$name = $person?->getName();

echo $name;
~~~~~~~
> Output: null
> + `$person `değişkeni null olduğu için getName() metodu çağrılmadan null döner ve herhangi bir hata meydana gelmez.
