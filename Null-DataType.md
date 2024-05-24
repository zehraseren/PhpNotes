***
### Null tanımlama yöntemleri nelerdir?
***
+ ```null``` veri tipi, bir değişkenin değerinin atanmadığını veya geçersiz olduğunu belirtmek için kullanılır.
+ ```null``` değeri atanmış bir değişken, boş bir değere sahiptir 

1. Doğrudan Atama
   - Bir değişkene ```null``` değeri doğrudan atanabilir.
~~~~~~~
#değisken = null;
~~~~~~~

2. Unset Fonksiyonu
   - Bu fonksiyon kullanılarak bir değişkenin değeri ```null``` olarak ayarlanabilir.
~~~~~~~
unset($değsken);
~~~~~~~

3. Fonksiyon Sonucu
   - Bazı durumlarda bir fonksiyon belirli bir koşulu karşılamadığında veya beklenmeyen bir durum oluştuğunda ```null``` değeri döndürebilir.
~~~~~~~
$file_content = file_get_contents("dosya.txt");
if ($file_content === false) {
    $file_content = null;
}
~~~~~~~
