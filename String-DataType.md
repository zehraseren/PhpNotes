***
### String tanımlama yöntemleri nelerdir?
***
+ String değerler birkaç farklı şekilde tanımlanabilir.

1. Tek Tırnaklar ile Tanımlama
  - Tek tırnaklar içinde bir string tanımlamak, string içindeki özel karakterlerin (çift tırnak içindeki gibi) işlevselliğini kaybetmesini sağlar.
  - Tek tırnaklarla tanımlanan bir string'de değişkenin değeri doğrudan yazıldığı şekilde çıkar.
~~~~~~~
$string = 'Merhaba, dünya!';
~~~~~~~
> Output: Merhaba, dünya!

2. Çift Tırnaklar ile Tanımlama
  - Çift tırnaklar içinde bir string tanımlamak, değişkenlerin değerlerini ve kaçış dizilerini (escape sequences) dikkate alır. Yani, çift tırnaklarla tanımlanan bir string'de değişkenlerin değerleri yerine geçer.
~~~~~~~
$isim = 'Zehra';
$string = "Merhaba, $isim!";
~~~~~~~
> Output: Merhaba Zehra!

3. Heredoc Yapısı
  - Uzun string'lerin tanımlanmasını sağlar ve çift tırnaklarla tanımlanan string'lere benzer şekilde değişkenleri ve kaçış dizilerini dikkate alır.
~~~~~~~
$isim = 'Zehra';
$string = <<<EOT
Merhaba, $isim!
Bu uzun bir string örneğidir.
EOT;
~~~~~~~
> Output: Merhaba Zehra! Bu uzun bir string örneğidir.
> + Heredoc yapısını kullanmak için, <<< operatörünü ve ardından bir belirleyici (identifier) yazılır.
> + Belirleyici olarak genellikle ```EOT``` (End Of Text) kullanılır, ancak bu herhangi bir isim olabilir.
> + ```EOT``` de bu sözdizimi yapısının bitiş belirtecidir.
> + ```Bitirici belirleyici, satırın başında ve herhangi bir boşluk veya karakter olmaksızın yazılmalıdır.```

4. Nowdoc Yapısı
  - Tek tırnaklarla tanımlanan string'lere benzer şekilde çalışır, yani içindeki değişkenleri ve kaçış dizilerini dikkate almaz.
  - Nowdoc, değişkenlerin değerlendirilmemesi gereken durumlarda kullanılır.
~~~~~~~
$isim = 'Zehra';
$string = <<<'EOT'
Merhaba, $isim!
Bu uzun bir string örneğidir.
EOT;
~~~~~~~
> Output: Merhaba, $isim! Bu uzun bir string örneğidir.

5. String Fonksiyonları ile Oluşturma
  - Birçok string fonksiyonu, string oluşturmak için kullanılabilir.
  - Örneğin, ```implode()```, ```sprintf()```, ```str_repeat()```, vb.
~~~~~~~
$isim = 'Zehra';
$string = sprintf("Merhaba, %s!", $isim);
~~~~~~~
> Output: Merhaba, Zehra!

##### String fonksiyonlarının açıklamaları
+ implode() Fonksiyonu
  - Bir dizi elemanlarını belirli bir ayraç kullanarak birleştirir ve tek bir string oluşturur.
    - Kullanımı: string implode (string $glue, array $pieces)
    - ```$glue:``` Dizi elemanlarını birleştirmek için kullanılan ayraç.
    - ```$pieces:``` Birleştirilecek dizi.
~~~~~~~
$pieces = ['Ahmet', 'Mehmet', 'Ayşe'];
$string = implode(", ", $pieces);
echo $string; 
~~~~~~~
> Output: Ahmet, Mehmet, Ayşe"

+ sprintf() Fonksiyonu
  - Formatlanmış bir string döndürür. Belirli bir format dizgesi içinde belirtilen yer tutuculara ```(%s, %d, vb.)``` değişkenleri yerleştirir.
    - Kullanımı: string sprintf (string $format, mixed $values [, mixed $values ...])
    - ```$format:``` Format dizgesi.
    - ```$values:``` Format dizgesindeki yer tutucuların yerine geçecek değerler.
~~~~~~~
$isim = 'Zehra';
$string = sprintf("Merhaba, %s!", $isim);
echo $string;
~~~~~~~
> Output: "Merhaba, Zehra!"

+ str_repeat() Fonksiyonu
  - Belirli bir string'i belirtilen sayıda tekrarlar ve döner.
    - Kullanımı: string str_repeat (string $input, int $multiplier)
    - ```$input:``` Tekrar edilecek string
    - ```$multiplier:``` String'in kaç kez tekrarlanacağını belirten sayı.
~~~~~~~
echo str_repeat("Merhaba! ", 3);
~~~~~~~
> Output: Merhaba!, Merhaba!, Merhaba!
