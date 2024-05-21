***
### Echo nedir?
***
+  PHP'de bir dil yapısıdır ve metin ya da değişkenlerin değerlerini ekrana (veya çıktı akışına) yazdırmak için kullanılır.
+ ```echo```nun temel amacı, belirli bir değeri (string, sayısal değerler, değişkenler, vb.) kullanıcıya göstermek veya çıktıda yer almasını sağlamaktır.

    - En basit haliyle, echo bir veya birden fazla string veya değişkeni yazdırabilir.
    - Birden fazla değeri virgül ile ayırarak yazdırabilir:
      ~~~~~~~
      echo "Hello", " ", "World", "!";
      ~~~~~~~
    - Değişkenlerin değerlerini de yazdırabilir:
      ~~~~~~~
      $name = "Zehra";
      echo "Hello ", $name, "!";
      
      Output: Hello Zehra!  
      ~~~~~~~
    - Kullanım için parantezler kullanılabilir ancak gerekli değildir. Çünkü ```echo``` bir dil yapısı olduğundan, parantez kullanımı daha az yaygındır:
      ~~~~~~~
      echo("Hello World!")
      ~~~~~~~
    - HTML ile Kullanım:
      ~~~~~~~
      echo "<h1>Hello World!</h1>"
      ~~~~~~~
***
### Print ile Echo'nun farkı nedir?
***
+ ```echo``` ve ```print``` arasında bazı küçük farklar vardır:
    - Hız: ```echo``` biraz daha hızlıdır çünkü dönüş değeri yoktur.
    - Kullanım: ```print``` bir değeri döndürür (1) ve bu yüzden bir ifadenin parçası olarak kullanılabilir. ```echo``` ise dönüş değeri dönmez.
    - Parantez Kullanımı: ```echo``` parantezsiz kullanılabilirken, ```print``` parantez gerektirir.
 
> ```echo```, PHP'de metin ve değişkenlerin değerlerini ekrana yazdırmak için kullanılan basit ama çok güçlü yapıdır. Parantezler gerektirmemesi ve birden fazla değeri yazdırabilmesi gibi özellikleri sayesinde oldukça esnek ve kullanışlıdır.
