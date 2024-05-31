### 1. array_chunk() Fonksiyonu
+  Bir array'i belirli bir boyuta sahip parçalara böler.
~~~~~~~
array_chunk(array $array, int $size, bool $preserve_keys = false): array
~~~~~~~
  - `$array:` Bölünecek olan orijinal dizi
  - `$size:` Her bir alt dizinin boyutu.
  - `$preserve_keys:` (Opsiyonel) Anahtarların korunup korunmayacağını belirten bir boolean. Varsayılan değeri `false`'tur. `true` olarak ayarlanırsa, orijinal dizinin anahtarları korunur.
  - `Dönen Değer:` Orijinal diziyi alt dizilere bölen çok boyutlu bir dizi döner

###### Temel Kullanım
~~~~~~~
$array = [1, 2, 3, 4, 5, 6];
$chunked_array = array_chunk($array, 2);

print_r($chunked_array);
~~~~~~~
> Output: Array
(
    [0] => Array
        (
            [0] => 1
            [1] => 2
        )
    [1] => Array
        (
            [0] => 3
            [1] => 4
        )
    [2] => Array
        (
            [0] => 5
            [1] => 6
        )
)

###### Preserve Keys
~~~~~~~
$array = ['a' => 1, 'b' => 2, 'c' => 3, 'd' => 4, 'e' => 5];
$chunked_array = array_chunk($array, 2, true);

print_r($chunked_array);
~~~~~~~
> Output: Array
(
    [0] => Array
        (
            [a] => 1
            [b] => 2
        )
    [1] => Array
        (
            [c] => 3
            [d] => 4
        )
    [2] => Array
        (
            [e] => 5
        )
)

----

### 2. array_combine() Fonksiyonu
+ İki farklı diziyi kullanarak bir dizi oluşturmak için kullanılır. Bu fonksiyon, bir array'de key ve diğerinde değerler olarak bulunan verileri birleştirir ve yeni bir array oluşturur.
+ `İki array'in boyutlarının eşit olması gerekir, aksi takdirde hata alınır.`
~~~~~~~
array_combine(array $keys, array $values): array|false
~~~~~~~
  - `$keys:` Key'leri içeren dizi
  - `$values:` Değerleri içeren dizi
  - `Dönen Değer:` Key'leri `$keys` array'inden, değerleri `$values` array'inden alan yeni bir array döner. Eğer `$keys` ve `$values`array'lerinin boyutları eşit değilse `false` döner.

###### Temel Kullanım
~~~~~~~
$keys = ['a', 'b', 'c'];
$values = [1, 2, 3];

$result = array_combine($keys, $values);
print_r($result);
~~~~~~~
> Output: Array
(
    [a] => 1
    [b] => 2
    [c] => 3
)

###### Hatalı Kullanım
~~~~~~~
$keys = ['a', 'b', 'c'];
$values = [1, 2];

$result = array_combine($keys, $values);
var_dump($result);
~~~~~~~
> Output: bool(false)

----

### 3. array_filter() Fonksiyonu
+ Bir array'de belirli bir koşulu sağlayan öğeleri filtrelemek için kullanılır.
+ Bu fonksiyon, bir array'e uygulanan bir callback fonksiyonu aracılığıyla her öğeyi değerlendirir. `Callback fonksiyonu, her öğe için true veya false dönen bir koşul belirtir. Sadece koşulu sağlayan öğeler, sonuç array'inde yer alır.`
~~~~~~~
array_filter(array $array, callable $callback = null, int $flag = 0): array
~~~~~~~
  - `$array:` Filtreleme işlemi yapılacak olan array
  - `$callback:` (Opsiyonel) Her öğe için çağrılacak olan işlev. Eğer belirtilmezse, boş değerler (false, null, 0, '', [] vs.) varsayılan olarak filtrelenir.
  - `$flag:` Filtreleme için kullanılacak ek bir bayrak. Varsayılan olarak 0'dır. Aşağıdaki sabitlerden biri olabilir:
    - `ARRAY_FILTER_USE_KEY:` Callback fonksiyonuna key'lerle birlikte öğeleri iletilir.
    - `ARRAY_FILTER_USE_BOTH:` Callback fonksiyonuna key'ler ve öğeler birlikte öğeleri iletilir.
  - `Dönen Değer:` Filtrelenmiş öğelerden oluşan bir dizi döner.

###### Temel Kullanım
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_filter($array, function($value) {
    return $value % 2 == 0;
});

print_r($result);
~~~~~~~
> Output: Array
(
    [1] => 2
    [3] => 4
)

###### Boş Değerleri Filtreleme
~~~~~~~
$array = [1, '', 0, 'foo', null, 'bar'];

$result = array_filter($array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [3] => foo
    [5] => bar
)

###### Callback Fonksiyonu Kullanımı
+ Key'leri çift olan öğeleri filtreleme
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_filter($array, function($value, $key) {
    return $key % 2 == 0;
}, ARRAY_FILTER_USE_KEY);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [2] => 3
    [4] => 5
)

###### Key ve Öğelerin Kullanımı
+ Çift sayıları ve 'b' key'leri filtreleme
~~~~~~~
$array = ['a' => 1, 'b' => 2, 'c' => 3, 'd' => 4];

$result = array_filter($array, function($value, $key) {
    return $value % 2 == 0 && $key != 'b';
}, ARRAY_FILTER_USE_BOTH);

print_r($result);
~~~~~~~
> Output: Array
(
    [c] => 3
    [d] => 4
)

----

### 4. array_values() Fonksiyonu
+ Bir array'in tüm değerlerini içeren sayısal key'li yeni bir array döndürür.
+ Bu fonksiyon, `orijinal array'deki key'leri korumaz ve array'nin değerlerini sıfırdan başlayan sayısal key'lerle yeniden indeksler.`
~~~~~~~
array_values(array $array): array
~~~~~~~
  - `$array:` Değerlerini almak istenilen array
  - `Dönen Değer:` Orijinal dizinin değerlerini içeren, yeniden indekslenmiş sayısal anahtarlı bir dizi döner.

###### Temel Kullanım
~~~~~~~
$array = ['a' => 1, 'b' => 2, 'c' => 3];
$result = array_values($array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 3
)

###### Karışık Key'lerle Kullanım
~~~~~~~
$array = [10 => 'apple', 20 => 'banana', 30 => 'cherry'];
$result = array_values($array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => apple
    [1] => banana
    [2] => cherry
)

###### Boş Değerin Korunması
~~~~~~~
$array = [1, 2, null, 4, 0, ''];
$result = array_values($array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 
    [3] => 4
    [4] => 0
    [5] => 
)

----

### 5. array_keys() Fonksiyonu
+ Bir array'in tüm key'lerini (veya belirli bir değere sahip key'lerini) içeren bir array döndürmek için kullanılır.
+ Bu fonksiyon, `array içindeki key'leri almanın en hızlı ve kolay yollarından biridir.`
~~~~~~~
array_keys(array $array, mixed $search_value = null, bool $strict = false): array
~~~~~~~
  - `$array:` Anahtarlarını almak istenilen array
  - `$search_value:` (Opsiyonel) Belirli bir değere sahip key'leri almak istenirse bu değer belirmelidir. Belirtilmezse tüm key'ler döner.
  - `$strict:` (Opsiyonel) Arama işleminin katı (strict) olup olmayacağını belirtir. Varsayılan olarak `false`'tur. `true` olarak ayarlanırsa, `===` operatörü kullanılarak katı karşılaştırma yapılır.
  - `Dönen Değer:` Array'in key'lerini içeren bir array döner. Eğer `$search_value` belirtilirse, bu değere sahip key'leri içeren bir array döner.

###### Temel Key'leri Alma
~~~~~~~
$array = ['a' => 1, 'b' => 2, 'c' => 3];
$keys = array_keys($array);

print_r($keys);
~~~~~~~
> Output: Array
(
    [0] => a
    [1] => b
    [2] => c
)

###### Belirli Bir Değere Sahip Key'leri Alma
~~~~~~~
$array = ['a' => 1, 'b' => 2, 'c' => 1];
$keys = array_keys($array, 1);

print_r($keys);
~~~~~~~
> Output: Array
(
    [0] => a
    [1] => c
)

###### Katı Karşılaştırma ile Key'leri Alma
~~~~~~~
$array = ['a' => 1, 'b' => '1', 'c' => 1];
$keys = array_keys($array, 1, true);

print_r($keys);
~~~~~~~
> Output: Array
(
    [0] => a
    [1] => c
)

----

### 6. array_map() Fonksiyonu
+ Bir veya daha fazla array'in her öğesine belirli bir işlevi uygulayarak yeni bir array oluşturur.
+ Bu fonksiyon, `array'ler üzerinde toplu işlemler yapmak için kullanılır ve özellikle fonksiyonel programlama tarzında faydalıdır.`
~~~~~~~
array_map(callable $callback, array $array, array ...$arrays): array
~~~~~~~
  - `$callback:` Array'in her öğesi üzerinde çalıştırılacak olan işlev. Bu işlevin geri çağrısı, array'in her öğesi için çağrılır ve işlevin dönüş değeri, yeni array'in ilgili öğesi olur.
  - `$array:` İşlem yapılacak birinci array
  - `...$array:` (Opsiyonel) İşlem yapılacak ek array'ler
  - `Dönen Değer:` İşlevin uygulanması sonucu oluşan yeni bir array döner.

###### Temel Kullanım
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_map(function($value) {
    return $value * 2;
}, $array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 2
    [1] => 4
    [2] => 6
    [3] => 8
    [4] => 10
)

###### Birden Fazla Array ile Kullanım
~~~~~~~
$array1 = [1, 2, 3];
$array2 = [4, 5, 6];

$result = array_map(function($x, $y) {
    return $x + $y;
}, $array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 5
    [1] => 7
    [2] => 9
)

###### Yerleşik PHP Fonksiyonlarını Kullanma
~~~~~~~
$array = ["apple", "banana", "cherry"];

$result = array_map('strtoupper', $array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => APPLE
    [1] => BANANA
    [2] => CHERRY
)

###### Boş Elemanları Filtreleme
~~~~~~~
$array = [1, null, 3, '', 5];

$result = array_map(function($value) {
    return $value ?: 'default';
}, $array);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => default
    [2] => 3
    [3] => default
    [4] => 5
)

----

### 7. array_merge() Fonksiyonu
+ Bir veya daha fazla array'i birleştirerek yeni bir array oluşturur. 
+ Bu fonksiyon, `array'leri birleştirirken mevcut key'lerin nasıl ele alındığını ve sayısal ve metinsel anahtarların nasıl işlendiğini göz önünde bulundurur.`
~~~~~~~
array_merge(array ...$arrays): array
~~~~~~~
  - `...$array:` Birleştirilecek array'ler. Birden fazla array belirtilebilir.
  - `Dönen Değer:` Birleştirilen array'lerin tüm öğelerini içeren yeni bir array döner.

###### Temel Kullanım
~~~~~~~
$array1 = [1, 2, 3];
$array2 = [4, 5, 6];

$result = array_merge($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 3
    [3] => 4
    [4] => 5
    [5] => 6
)

###### Metinsel Key'lerle Kullanım
~~~~~~~
$array1 = ['a' => 1, 'b' => 2];
$array2 = ['c' => 3, 'd' => 4];

$result = array_merge($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [a] => 1
    [b] => 2
    [c] => 3
    [d] => 4
)

###### Key'lerin Üzerine Yazma
~~~~~~~
$array1 = ['a' => 1, 'b' => 2];
$array2 = ['b' => 3, 'c' => 4];

$result = array_merge($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [a] => 1
    [b] => 3
    [c] => 4
)

###### Sayısal Key'lerin Yeniden İndekslenmesi
~~~~~~~
$array1 = [1 => 'apple', 2 => 'banana'];
$array2 = [3 => 'cherry', 4 => 'date'];

$result = array_merge($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => apple
    [1] => banana
    [2] => cherry
    [3] => date
)

###### Boş Array ile Birleştirme
~~~~~~~
$array1 = [1, 2, 3];
$array2 = [];

$result = array_merge($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 3
)

----

### 8. array_reduce() Fonksiyonu
+ Bir array'i azaltarak tek bir değere indirgemek için kullanılır.
+ Bu işlem, array'in her bir öğesi üzerinde belirtilen bir callback fonksiyonun çalıştırılmasıyla gerçekleştirilir. `Callback fonksiyonu, her adımdaki ara sonuç ve array'in mevcut öğesi üzerinde çalışır.`
~~~~~~~
array_reduce(array $array, callable $callback, mixed $initial = null): mixed
~~~~~~~
  `$array:` İşlem yapılacak array
  `$callback:` Array'in her öğesi üzerinde çalıştırılacak olan geri çağırma işlevi. Bu işlevin imzası şu şekilde olmalıdır: `function($carry, $item)`, burada `$carry` bir önceki geri çağırma işlevinin dönüş değerini taşır ve `$item` array'in mevcut öğesidir.
  `$initial:` (Opsiyonel) Başlangıç değeri. Eğer belirtilmezse, ilk öğe başlangıç değeri olarak kullanılır ve işlem ikinci öğeden başlar.
  `Dönen Değer:` Dizinin indirgenmiş değeri döner.

###### Toplama İşlemi
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_reduce($array, function($carry, $item) {
    return $carry + $item;
}, 0);

echo $result; 15
~~~~~~~
> Output: 

###### Çarpma İşlemi
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_reduce($array, function($carry, $item) {
    return $carry * $item;
}, 1);

echo $result;
~~~~~~~
> Output: 120

###### Array'deki En Büyük Değeri Bulma
~~~~~~~
$array = [1, 9, 3, 7, 5];

$result = array_reduce($array, function($carry, $item) {
    return ($carry > $item) ? $carry : $item;
});

echo $result;
~~~~~~~
> Output: 9

###### Başlangıç Değeri Belirtmeme
~~~~~~~
$array = [1, 2, 3, 4, 5];

$result = array_reduce($array, function($carry, $item) {
    return $carry + $item;
});

echo $result;
~~~~~~~
> Output: 15

###### Dize Birleştirme
~~~~~~~
$array = ['Hello', 'world', 'this', 'is', 'PHP'];

$result = array_reduce($array, function($carry, $item) {
    return $carry . ' ' . $item;
}, '');

echo $result;
~~~~~~~
> Output: " Hello world this is PHP"

----

### 9. array_search() Fonksiyonu
+ Bir array'de belirli bir değeri arar ve bu değeri bulursa ilgili key'i döner.
+ Bu fonksiyon, değeri bulamazsa `false` döner. `array_search() fonksiyonu, büyük küçük harf duyarlılığı ve katı karşılaştırma gibi çeşitli seçenekler sunar.`
~~~~~~~
array_search(mixed $needle, array $haystack, bool $strict = false): int|string|false
~~~~~~~
  - `$needle:` Aranacak değer
  - `$haystack:` Arama yapılacak array
  - `$strict:` (Opsiyonel) Katı karşılaştırma yapılmasını belirtir. Varsayılan olarak `false`'dur. `true` olarak ayarlanırsa, `===` operatörü kullanılarak katı karşılaştırma yapılır.
  - `Dönen Değer:`
    - Eğer aranan değer bulunursa, değerin key'i (int veya string) döner.
    - Eğer aranan değer bulunamazsa, `false` döner.

###### Temel Kullanım
~~~~~~~
$array = [0 => 'apple', 1 => 'banana', 2 => 'cherry'];

$key = array_search('banana', $array);

echo $key;
~~~~~~~
> Output: 1

###### Katı Karşılaştırma ile Kullanım
~~~~~~~
$array = [0 => '10', 1 => 10, 2 => '20'];

$key = array_search(10, $array, true);

echo $key;
~~~~~~~
> Output: 1

###### Değer Bulunamazsa
~~~~~~~
$array = [0 => 'apple', 1 => 'banana', 2 => 'cherry'];

$key = array_search('grape', $array);

var_dump($key);
~~~~~~~
> Output: bool(false)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$array = ['first' => 'apple', 'second' => 'banana', 'third' => 'cherry'];

$key = array_search('cherry', $array);

echo $key;
~~~~~~~
> Output: third

###### Boole Değerinin Katı Karşılaştırma ile Kullanımı
~~~~~~~
$array = [0 => '0', 1 => false, 2 => null];

$key = array_search(false, $array, true);

echo $key;
~~~~~~~
> Output: 1

----

### 10. in_Array() Fonksiyonu
+ Bir array'de belirli bir değerin var olup olmadığını kontrol eder.
+ Bu fonksiyon, değerin array'de bulunması durumunda `true`, bulunmaması durumunda `false` döner. `Katı karşılaştırma seçeneği ile büyük küçük harf duyarlılığı ve veri türü eşleşmesi yapılabilir.`
~~~~~~~
in_array(mixed $needle, array $haystack, bool $strict = false): bool
~~~~~~~
  - `$needle:` Aranacak değer
  - `$haystack:` Arama yapılacak array
  - `$strict:` (Opsiyonel) Katı karşılaştırma yapılmasını belirtir. Varsayılan olarak `false`'dur. `true` olarak ayarlanırsa, `===` operatörü kullanılarak katı karşılaştırma yapılır
  - `Dönen Değer:` Değer array'de bulunursa `true`, bulunamazsa `false` döner.

###### Temel Kullanım
~~~~~~~
$array = ['apple', 'banana', 'cherry'];

$exists = in_array('banana', $array);

var_dump($exists);
~~~~~~~
> Output: bool(true)

###### Değer Bulunamazsa
~~~~~~~
$array = ['apple', 'banana', 'cherry'];

$exists = in_array('grape', $array);

var_dump($exists);
~~~~~~~
> Output: bool(false)

###### Katı Karşılaştırma ile Kullanım
~~~~~~~
$array = ['10', 20, 30];

$exists = in_array(10, $array, true);

var_dump($exists);

$exists = in_array('10', $array, true);

var_dump($exists);
~~~~~~~
> Output: bool(false)

> Output: bool(true)

###### Boole Değerlerinin Kullanımı
~~~~~~~
$array = [0, false, null];

$exists = in_array(false, $array);

var_dump($exists);

$exists = in_array(false, $array, true);

var_dump($exists);

$exists = in_array('0', $array, true);

var_dump($exists);
~~~~~~~
> Output: bool(true)

> Output: bool(true)

> Output: bool(false)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$array = ['first' => 'apple', 'second' => 'banana', 'third' => 'cherry'];

$exists = in_array('cherry', $array);

var_dump($exists);
~~~~~~~
> Output: bool(true)

----

### 11. array_diff() Fonksiyonu
+ Bir veya daha fazla array'de bulunmayan değerleri içeren bir array döner.
+ Başka bir deyişle, `belirtilen array'ler arasında karşılaştırma yaparak, ilk array'de olup diğer array'lerde olmayan değerleri bulur ve bu değerleri içeren bir array oluşturur.`
~~~~~~~
array_diff(array $array, array ...$arrays): array
~~~~~~~
  - `$array:` Karşılaştırmanın yapılacağı ana array
  - `...$array:` Karşılaştırılacak diğer array'ler
  - `Dönen Değer:` Diğer array'lerde bulunmayan ana array'deki değerleri içeren bir array döner.

###### Temel Kullanım
~~~~~~~
$array1 = ["a" => "green", "red", "blue", "red"];
$array2 = ["b" => "green", "yellow", "red"];

$result = array_diff($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [1] => blue
)

###### Birden Fazla Array ile Kullanım
~~~~~~~
$array1 = ["a" => "green", "red", "blue", "red"];
$array2 = ["b" => "green", "yellow", "red"];
$array3 = ["green", "blue"];

$result = array_diff($array1, $array2, $array3);

print_r($result);
~~~~~~~
> Output: Array
(
)

###### Sayısal Key'lerle Kullanım
~~~~~~~
$array1 = [1, 2, 3, 4, 5];
$array2 = [4, 5, 6, 7, 8];

$result = array_diff($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 3
)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$array1 = ["a" => "green", "b" => "red", "c" => "blue"];
$array2 = ["a" => "green", "b" => "yellow", "d" => "blue"];

$result = array_diff($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [b] => red
    [c] => blue
)

###### Boole Değerlerinin Kullanımı
~~~~~~~
$array1 = [true, false, 1, 0];
$array2 = [false, 1];

$result = array_diff($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [0] => 1
    [3] => 0
)

----

### 12. array_diff_assoc() Fonksiyonu
+ Bir veya daha fazla array'de bulunmayan değerleri ve key'leri da dikkate alarak karşılaştırma yapar. `Yani, key ve değer çiftlerinin ikisi de eşleşmiyorsa, o çift sonuç dizisine dahil edilir.`
+ Bu fonksiyon, key'leri de dikkate alarak, ilk array'de olup diğer array'lerde olmayan key-değer çiftlerini bulur ve bunları içeren bir array döner.
~~~~~~~
array_diff_assoc(array $array, array ...$arrays): array
~~~~~~~
  - `$array:` Karşılaştırmanın yapılacağı ana array
  - `...$array:` Karşılaştırılacak diğer array'ler
  - `Dönen Değer:` Diğer array'lerde bulunmayan key-değer çiftlerini içeren bir array döner.

###### Temel Kullanım
~~~~~~~
$array1 = ["a" => "green", "b" => "brown", "c" => "blue", "red"];
$array2 = ["a" => "green", "yellow", "red"];

$result = array_diff_assoc($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [b] => brown
    [c] => blue
    [0] => red
)

###### Birden Fazla Array ile Kullanım
~~~~~~~
$array1 = ["a" => "green", "b" => "brown", "c" => "blue", "red"];
$array2 = ["a" => "green", "yellow", "red"];
$array3 = ["b" => "brown", "c" => "purple"];

$result = array_diff_assoc($array1, $array2, $array3);

print_r($result);
~~~~~~~
> Output: Array
(
    [c] => blue
    [0] => red
)

###### Sayısal Key'lerle Kullanım
~~~~~~~
$array1 = [1 => "one", 2 => "two", 3 => "three"];
$array2 = [1 => "one", 2 => "two", 3 => "four"];

$result = array_diff_assoc($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [3] => three
)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$array1 = ["a" => "green", "b" => "red", "c" => "blue"];
$array2 = ["a" => "green", "b" => "yellow", "d" => "blue"];

$result = array_diff_assoc($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [b] => red
    [c] => blue
)

###### Boole Değerlerinin Kullanımı
~~~~~~~
$array1 = ["a" => true, "b" => false, "c" => 1, "d" => 0];
$array2 = ["a" => true, "b" => 0];

$result = array_diff_assoc($array1, $array2);

print_r($result);
~~~~~~~
> Output: Array
(
    [b] => false
    [c] => 1
    [d] => 0
)

----

### 13. asort() Fonksiyonu
+ Bir array'i değerlerine göre artan sıraya göre sıralar ve key-değer ilişkisini korur. Yani, `bu fonksiyon array'deki değerleri sıralar ancak key'leri değiştirmez.`
~~~~~~~
asort(array &$array, int $sort_flags = SORT_REGULAR): bool
~~~~~~~
  - `$array:` Sıralanacak array
  - `$sort_flags:` (Opsiyonel) Sıralama için kullanılacak bir sabit. Varsayılan olarak `SORT_REGULAR` kullanılır.
  - `Dönen Değer:` Başarı durumunda `true`, aksi halde `false` döner.

###### Temel Kullanım
~~~~~~~
$fruits = ["d" => "lemon", "a" => "orange", "b" => "banana", "c" => "apple"];

asort($fruits);

print_r($fruits);
~~~~~~~
> Output: Array
(
    [c] => apple
    [b] => banana
    [d] => lemon
    [a] => orange
)

###### Sayısal Değerlerle Kullanım
~~~~~~~
$numbers = [4, 2, 8, 6];

asort($numbers);

print_r($numbers);
~~~~~~~
> Output: Array
(
    [1] => 2
    [0] => 4
    [3] => 6
    [2] => 8
)

###### SORT_NUMERIC Kullanımı
~~~~~~~
$numbers = [4, 2, 8, 6];

asort($numbers, SORT_NUMERIC);

print_r($numbers);
~~~~~~~
> Output: Array
(
    [1] => 2
    [0] => 4
    [3] => 6
    [2] => 8
)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$colors = ["a" => "red", "b" => "green", "c" => "blue"];

asort($colors);

print_r($colors);
~~~~~~~
> Output: Array
(
    [a] => red
    [b] => green
    [c] => blue
)

----

### 14. ksort() Fonksiyonu
+ Bir array'i key'lerine göre artan sıraya göre sıralar. Bu fonksiyon, `array'deki key'leri sıralar ancak değerlerin sıralamasını değiştirmez.`
~~~~~~~
ksort(array &$array, int $sort_flags = SORT_REGULAR): bool
~~~~~~~
  - `$array:` Sıralanacak array
  - `$sort_flags:` (Opsiyonel) Sıralama için kullanılacak bir sabit. Varsayılan olarak `SORT_REGULAR` kullanılır.
  - `Dönen Değer:` Başarı durumunda `true`, aksi halde `false` döner.

###### Temel Kullanım
~~~~~~~
$fruits = ["d" => "lemon", "a" => "orange", "b" => "banana", "c" => "apple"];

ksort($fruits);

print_r($fruits);
~~~~~~~
> Output: Array
(
    [a] => orange
    [b] => banana
    [c] => apple
    [d] => lemon
)

###### Sayısal Değerlerle Kullanım
~~~~~~~
$numbers = [4, 2, 8, 6];

ksort($numbers);

print_r($numbers);
~~~~~~~
> Output:Array
(
    [0] => 4
    [1] => 2
    [2] => 8
    [3] => 6
)

###### Asosiyatif Array ile Kullanım
~~~~~~~
$colors = ["a" => "red", "b" => "green", "c" => "blue"];

ksort($colors);

print_r($colors);
~~~~~~~
> Output: Array
(
    [a] => red
    [b] => green
    [c] => blue
)

###### SORT_NUMERIC Kullanımı
~~~~~~~
$numbers = ["10" => "10", "2" => "2", "20" => "20", "1" => "1"];

ksort($numbers, SORT_NUMERIC);

print_r($numbers);
~~~~~~~
> Output: Array
(
    [1] => 1
    [2] => 2
    [10] => 10
    [20] => 20
)

----

### 15. usort() Fonksiyonu
+ Bir array'i kullanıcı tanımlı bir karşılaştırma işlevine göre sıralar. Bu işlev, array'deki öğeleri karşılaştırmak için kullanılır ve sonuç olarak sıralı bir array döndürür.
~~~~~~~
usort(array &$array, callable $compare_function): bool
~~~~~~~
  - `$array:` Sıralanacak array
  - `$compare_function:` Kullanıcı tanımlı bir karşılaştırma işlevi (callable)
  - `Dönen Değer:` Başarı durumunda `true`, aksi halde `false` döner.

###### Temel Kullanım
~~~~~~~
function compare_length($x, $y) {
    return strlen($x) - strlen($y);
}

$fruits = ["apple", "orange", "banana", "kiwi"];

usort($fruits, "compare_length");

print_r($fruits);
~~~~~~~
> Output: Array
(
    [0] => kiwi
    [1] => apple
    [2] => orange
    [3] => banana
)

###### Anonim Fonksiyon ile Kullanım
~~~~~~~
$numbers = [5, 2, 8, 3, 1];

usort($numbers, function($x, $y) {
    return $x - $y;
});

print_r($numbers);
~~~~~~~
> Output: Array
(
    [0] => 1
    [1] => 2
    [2] => 3
    [3] => 5
    [4] => 8
)
> + Bu fonksiyon, `$x`'i `$y`'den çıkarır. Bu işlemin sonucuna göre, sıralama yapılır. Eğer sonuç negatif ise `$x` önce gelir; pozitif ise `$y` önce gelir; sıfır ise değişiklik olmaz.
----

### Array destructuring (Array'i yeniden yapılandırma)
+ Array destructuring, bir array içindeki değerleri ayrıştırarak, bu değerleri tek tek değişkenlere atama işlemidir.
+ Bu işlem, modern PHP sürümlerinde kullanılabilen bir özelliktir ve genellikle kodun daha okunabilir ve kısa olmasını sağlar.

#### Array Destructuring'in Kullanımı
+ Öncelikle bir array oluşturulur.
~~~~~~~
$colors = ["red", "green", "blue"];
~~~~~~~

+ Sonra, bu array ayrıştırılarak değişkenlere atama yapılır.
~~~~~~~
[$color1, $color2, $color3] = $colors;
~~~~~~~
> Bu işlem, `$colors` dizisindeki ilk değeri `$colors1` değişkenine, ikinci değeri `$colors2` değişkenine ve üçüncü değeri `$colors3` değişkenine atar.

+ Array'deki bazı değerlere erişmek istenmiyotsa, `_` işareti kullanarak atama işlemini yapılabilir.
~~~~~~~
[$color1, , $color3] = $colors;
~~~~~~~
> Bu durumda, `$colors2` değişkeni atlanacak ve sadece `$colors1` ve `$colors3` değişkenlerine değer atanacaktır.

> Bu özellik aynı zamanda çok boyutlu array'ler için de geçerlidir. 
~~~~~~~
$person = ["John", "Doe", ["age" => 30, "city" => "New York"]];
[$firstName, $lastName, ["age" => $age, "city" => $city]] = $person;
~~~~~~~
> Bu durumda, `$firstName` ve `$lastName` değişkenlerine sırasıyla "John" ve "Doe" değerleri atanırken, `$age` değişkenine 30 ve `$city` değişkenine "New York" değeri atanacaktır.

> Array destructuring, özellikle fonksiyonlardan dönen değerleri kolayca işlemek ve belirli değerlere erişmek için kullanışlıdır. Ayrıca, array içindeki değerlerin anlamını belirlemek ve bu değerlere daha anlamlı isimler vermek için de kullanılabilir.
