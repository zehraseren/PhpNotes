 ***
### Loop - Break & Continue Statements nedir?
***
+ `break` ve `continue` ifadeleri, döngülerin davranışını kontrol etmek için kullanılan yapısal elemanlardır.

1. while
----
+ Belirli bir koşul doğru olduğu sürece bir kod bloğunu çalıştırmak için kullanılır.
~~~~~~~
$i = 0;
while ($i < 5) {
    echo $i . " ";
    $i++;
}
~~~~~~~
> Output: 0 1 2 3 4
> +  `$i` değişkeninin değeri 0 olduğu sürece ($i < 5 koşulu doğru olduğu sürece) kod bloğunu çalıştırır ve her adımda `$i`'yi bir artırır.

2. do-while
----
+ Kod bloğunu en az bir kez çalıştırmak için kullanılır ve ardından koşul doğru olduğu sürece döngüyü tekrarlar.
~~~~~~~
$i = 0;
do {
    echo $i . " ";
    $i++;
} while ($i < 5);
~~~~~~~
> Output: 0 1 2 3 4
> + Kod bloğunu en az bir kez çalıştırır ve ardından `$i` değeri 5 olana kadar ($i < 5 koşulu doğru olduğu sürece) tekrarlar.

3. for
----
+ Belirli bir başlangıç değeri, bir koşul ve bir artış/azalış ifadesi ile belirli bir aralıkta döngüyü çalıştırmak için kullanılır.
~~~~~~~
for ($i = 0; $i < 5; $i++) {
    echo $i . " ";
}
~~~~~~~
> Output: 0 1 2 3 4
> + `$i` değişkeninin değeri 0 ile başlayıp, 4 olduğu koşulu sağlayana kadar ($i < 5), her döngü adımında `$i`'yi bir artırarak çalışır.

4. foreach
----
+ Bir array'in veya başka bir iterable (yinelenebilir) nesnenin her elemanı için bir kod bloğunu çalıştırmak için kullanılır.
~~~~~~~
$array = [1, 2, 3, 4, 5];
foreach ($array as $item) {
    echo $item . " ";
}
~~~~~~~
> Output: 0 1 2 3 4
> + `$array`'in her elemanı için kod bloğunu çalıştırır ve her bir elemanı $item`` değişkenine atar.

##### Hangi döngü ne şekilde kullanılmalıdır?
+ `while:` Bir koşul doğru olduğu sürece döngüyü çalıştırmak için.
+ `do-while:` Kod bloğunu en az bir kez çalıştırmak ve ardından koşul doğru olduğu sürece tekrar etmek için.
+ `for:` Belirli bir aralıkta döngüyü çalıştırmak için.
+ `foreach:` Bir array'in veya iterable nesnenin her elemanı için döngüyü çalıştırmak için.
