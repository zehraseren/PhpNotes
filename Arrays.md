### 1. array_chunk() Fonksiyonu
+  Bir array'i belirli bir boyuta sahip parçalara böler.
~~~~~~~
array_chunk(array $array, int $size, bool $preserve_keys = false): array
~~~~~~~
  - `$array:` Bölünecek olan orijinal dizi
  - `$size:` Her bir alt dizinin boyutu.
  - `$preserve_keys:` (Opsiyonel) Anahtarların korunup korunmayacağını belirten bir boolean. Varsayılan değeri `false`'tur. `true` olarak ayarlanırsa, orijinal dizinin anahtarları korunur.
  - `Dönen değer:` Orijinal diziyi alt dizilere bölen çok boyutlu bir dizi döner

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
  - `Dönen değer:` Key'leri `$keys` array'inden, değerleri `$values` array'inden alan yeni bir array döner. Eğer `$keys` ve `$values`array'lerinin boyutları eşit değilse `false` döner.

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
+ Bu fonksiyon, bir array'e uygulanan bir callback fonksiyonu aracılığıyla her öğeyi değerlendirir. Callback fonksiyonu, her öğe için doğru (true) veya yanlış (false) dönen bir koşul belirtir. Sadece koşulu sağlayan öğeler, sonuç array'inde yer alır.
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

----

### 5. array_keys() Fonksiyonu

----

### 6. array_map() Fonksiyonu

----

### 7. array_merge() Fonksiyonu

----

### 8. array_reduce() Fonksiyonu

----

### 9. array_search() Fonksiyonu

----

### 10. in_Array() Fonksiyonu

----

### 11. array_diff() Fonksiyonu

----

### 12. array_diff_assoc() Fonksiyonu

----

### 13. asort() Fonksiyonu

----

### 14. ksort() Fonksiyonu

----

### 15. usort() Fonksiyonu

----

### Array destructuring (Array'i yeniden yapılandırma)
