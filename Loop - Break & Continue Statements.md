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

##### Örnek
~~~~~~~
$user = [
    "name" => "Zehra",
    "surname" => "Şeren",
    "skills" => ["PHP", "Angular", "JavaScript", "React"],
];

foreach ($user as $key => $value) {
    echo $key . ": ";

    if (is_array($value)) {
        foreach ($value as $skill) {
            echo $skill . ", ";
        }
    }
    else {
        echo $value;
    }
    echo '<br />';
}
~~~~~~~
+ `foreach ($user as $key => $value) {`: `$user` adlı array döngüye sokulur. Her bir array öğesi için, key (`$key`) ve değer (`$value`) çiftini alır.
+ ` echo $key . ": ";`: key (`$key`) ekrana yazdırılı ve `:` ile ayrım yapılır. Değerlerin ekrana yazdırılması bu ayrımdan sonra gerçekleştirilir.
+ `if (is_array($value)) {`: `$value`'nun bir array olup olmadığı kontrol edilir. Eğer `$value` bir array ise, içeri girerek ve array elemanlarını işlemek için başka bir iç içe döngü başlatılır.
+ `foreach ($value as $skill) {`: `$value` bir array ise bu iç içe döngü, `$value`'nun her bir elemanını alır ve `$skill` değişkenine atar. `Bu iç içe döngü, array elemanlarını tek tek ekrana yazdırmak için kullanılır.`
+ `echo $skill . ", ";`: İç içe döngüdeki her bir eleman, virgül ile ayrılarak ekrana yazdırılır.
+ `else {echo $value;}`: Eğer `$value` bir dizi değilse, yani tek bir değer ise, bu değer doğrudan ekrana yazdırılır.
+ ` echo '<br />';`: Her bir key-value çifti işlendikten sonra bir `<br />` etiketi kullanılarak satır değişimi sağlanır.

#### endfor ve endforeach nedir?
+ `endfor` ve `endforeach` ifadeleri, uzun `for` ve `foreach` döngülerini sonlandırmak için kullanılan özel ifadelerdir.
> Bu ifadeler, karmaşık döngülerde kodun okunabilirliğini artırmak için kullanılır.

+ `endfor` ifadesi, bir for döngüsünü sonlandırmak için kullanılır. Genellikle, bir for döngüsünde birden fazla satırda kod bulunuyorsa ve döngünün sonunu belirlemek zor oluyorsa kullanılır.
~~~~~~~
<?php
for ($i = 0; $i < 5; $i++) {
    echo $i . "<br>";
}
endfor;
?>
~~~~~~~

+ `endforeach` ifadesi, bir for döngüsünü sonlandırmak için kullanılır. Genellikle, bir foreach döngüsünde birden fazla satırda kod bulunuyorsa ve döngünün sonunu belirlemek zor oluyorsa kullanılır.
~~~~~~~
<?php
$array = [1, 2, 3, 4, 5];
foreach ($array as $item) {
    echo $item . "<br>";
}
endforeach;
?>
~~~~~~~
