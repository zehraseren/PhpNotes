***
### Conditional statements (koşullu İfadeler) nedir?
***
+  Programın belirli bir koşula bağlı olarak farklı yollar izlemesini sağlayan yapısal elemanlardır.
+  Programın akışını kontrol etmek ve belirli durumlara göre farklı işlemler yapmak için kullanılır.

1. if Statements
----
+ Bir koşul doğru olduğunda belirli bir kod bloğunu çalıştırmak için kullanılır.
~~~~~~~
if (conditional) {
  // Koşul doğruysa buradaki kod bloğu çalışır
}
~~~~~~~

2. if...else Statements
----
+ Bir koşul doğruysa bir kod bloğunu, yanlışsa ise başka bir kod bloğunu çalıştırmak için kullanılır.
~~~~~~~
if (conditional) {
    // Koşul doğruysa buradaki kod bloğu çalışır
} else {
    // Koşul yanlışsa buradaki kod bloğu çalışır
}
~~~~~~~

3. if...elseif...else Statements
----
+ Birden fazla koşulun olduğu senaryolarda, her bir koşulun ayrı ayrı değerlendirilmesini sağlar.
~~~~~~~
if (conditional1) {
    // Koşul1 doğruysa buradaki kod bloğu çalışır
} elseif (conditional2) {
    // Koşul2 doğruysa buradaki kod bloğu çalışır
} else {
    // Hiçbir koşul doğru değilse buradaki kod bloğu çalışır
}
~~~~~~~

> Programın çalışma akışını yönlendirmek ve belirli durumlar altında farklı kod bloklarını yürütmek için çok önemlidir. Özellikle veri işleme, kullanıcı girişi kontrolü, hata yönetimi ve benzeri durumlarda sıklıkla kullanılırlar.
