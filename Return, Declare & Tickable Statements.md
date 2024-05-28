***
### Return, Declare & Tickable Statements nedir?
***
+ `return`, `declare` ve `tickable` ifadeleri, kodun akışını ve yürütülme şeklini kontrol etmek için kullanılan özel ifadelerdir.
+ Özellikle büyük ve karmaşık uygulamalarda bu ifadelerin doğru kullanımı, kodun daha verimli ve bakımı kolay olmasını sağlar.

1. `return` ifadesi
+ Bir fonksiyonun veya metodun sonucunu belirlemek ve bu sonucu çağrıldığı yere geri döndürmek için kullanılır.
+ Bir fonksiyon içindeki `return` ifadesi aynı zamanda fonksiyonun çalışmasını sonlandırır. 
~~~~~~~
function sum($x, $y) {
  return $x + $y;
}

$result = sum(3, 4);
echo $result;
~~~~~~~
> Output: 7

2. `declare` ifadesi
+ Belirli yürütme yönergeleri belirlemek için kullanılır. Genellikle PHP'nin çalışma zamanı davranışını değiştirmek için kullanılır.
~~~~~~~
declare (directive)
statement
~~~~~~~

###### Kullanılabilir Yönergeler:
+ `ticks:` Kodun belirli sayıda döngü veya işlem sonrası "tick" adı verilen özel olaylar yaratmasını sağlar.
~~~~~~~
declare(ticks=1);

function tick_handler() {
    echo "Tick işlemi çalıştırıldı.\n";
}

register_tick_function('tick_handler');

for ($i = 0; $i < 5; ++$i) {
    echo "i: $i\n";
}
~~~~~~~
> Output: Tick işlemi çalıştırıldı. i:0 Tick işlemi çalıştırıldı.i: 1 Tick işlemi çalıştırıldı. i: 2 Tick işlemi çalıştırıldı. i: 3 Tick işlemi çalıştırıldı. i: 4 Tick işlemi çalıştırıldı. Tick işlemi çalıştırıldı.

+ `declare(ticks=1);`: Bu ifade, her bir tick olayının gerçekleşmesini sağlar. Tick olayları, genellikle bir döngü adımı veya bir deyim yürütüldüğünde tetiklenir.
+ `function tick_handler() {echo "Tick işlemi çalıştırıldı.\n";}`: `tick_handler` adında bir işlev tanımlanır. Bu işlev, her tick olayında çalıştırılır ve ekrana "Tick işlemi çalıştırıldı." mesajını yazdırır.
+ `register_tick_function('tick_handler');`: `tick_handler` işlevi tick olayları için kayıt eder. Yani, her tick olayında bu işlev çağrılacaktır.
+ `for ($i = 0; $i < 5; ++$i) {echo "i: $i\n";}`: 0'dan 4'e kadar dönen bir döngüyü tanımlar. Her döngü adımında, döngü değişkeni `$i`'nin değeri ekrana yazdırılır (örneğin, "i: 0", "i: 1", vb.).

3. `tickable` ifadesi
+  Her bir `tick` olayının meydana gelmesi durumunda çağrılan kod parçacıklarıdır.
+  Ticks, `declare(ticks=1);` ifadesi ile belirlenir ve belirli bir sayıda düşük seviyeli PHP komutunun yürütülmesinden sonra tetiklenir.
+  `tickable` ifadeler doğrudan PHP dilinde tanımlı değildir, ancak ticks yönergesi ile birlikte çalışır.
+  `register_tick_function` ile kaydedilir ve belirli sayıda komut yürütüldüğünde bu fonksiyonlar çağrılır.

##### Özet:
+ `return:` Fonksiyonların veya metodların sonucunu döndürmek ve çalışmasını sonlandırmak için kullanılır.
+ `declare:` PHP'nin çalışma zamanı davranışını belirli yönergeler ile değiştirmek için kullanılır. Yaygın olarak kullanılan yönerge `ticks`'dir.
+ `tickable:` `declare(ticks=1);` ifadesi ile belirlenen ve belirli sayıda düşük seviyeli komut yürütüldüğünde tetiklenen olaylardır. `register_tick_function` ile kullanılır.


