### 1. array_chunk() Fonksiyonu
+  Bir array'i belirli bir boyuta sahip parçalara böler.
~~~~~~~
array_chunk(array $array, int $size, bool $preserve_keys = false): array
~~~~~~~
  - `$array:` Bölünecek olan orijinal dizi
  - `$size:` Her bir alt dizinin boyutu.
  - `$preserve_keys:` (Opsiyonel) Anahtarların korunup korunmayacağını belirten bir boolean. Varsayılan değeri `false`'tur. `true` olarak ayarlanırsa, orijinal dizinin anahtarları korunur.
  - Dönen değer: Orijinal diziyi alt dizilere bölen çok boyutlu bir dizi döner

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

----

### 3. array_filter() Fonksiyonu

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

#### 14. ksort() Fonksiyonu

----

#### 15. usort() Fonksiyonu

----

#### Array destructuring (Array'i yeniden yapılandırma)
