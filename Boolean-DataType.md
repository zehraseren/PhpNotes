***
### Boolean tanımlama yöntemleri nelerdir?
***
+ Boolean (mantıksal) değerler, sadece iki olası değere sahiptir: true ve false.

1. Doğrudan Atama
   - Boolean değerler doğrudan değişkenlere atanabilir.
~~~~~~~
$doğru = true;
$yanlis = false;
~~~~~~~

2. Karşılaştırma Operatörleri Sonucu
  - Karşılaştırma operatörleri kullanılarak bir ifade oluşturulabilir ve bu ifade doğrudan bir boolean değer döndürebilir.
~~~~~~~
$buyuk_mu_kucuk_mu = (10 > 5)
~~~~~~~
> Output: true

~~~~~~~
$esit_mi = (10 == 5);
~~~~~~~
> Output: false
 
3. Boolean Fonksiyonları
  - Bazı PHP fonksiyonları, koşulları değerlendirerek doğrudan boolean değer döndürürler.
~~~~~~~
$is_string = is_string("hello");
~~~~~~~
> Output: true

~~~~~~~
$is_array = is_array(["apple", "banana", "cherry"]);
~~~~~~~
> Output: true
 
4. Tür Dönüşümü (Type Casting)
  - Diğer veri türlerini boolean'a dönüştürülebilir. Boş bir string, boş bir dizi, 0 veya 0.0 olmayan herhangi bir değer bir sayı veya ```null``` ```false``` olarak kabul edilir; diğer durumlarda ```true``` döner.
~~~~~~~
$boolean_from_int = (bool) 1;
~~~~~~~
> Output: true
 
~~~~~~~
$boolean_from_string = (bool) "hello";
~~~~~~~
> Output: true

~~~~~~~
$boolean_from_null = (bool) null;
~~~~~~~
> Output: true
