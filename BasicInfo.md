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
+ Kullanım Türleri:
    - Temel Kullanım: Aşağıdaki kullanım, ```<?php echo "Hello world"; ?>``` ifadesinin kısa bir yazımıdır ve işlevsel olarak aynıdır. ```<?=``` ifadesi ```<?php echo``` ifadesinin yerine geçer ve PHP'nin ```echo``` komutunu çalıştırır.
      ~~~~~~~
      <?= "Hello world" ?>
      ~~~~~~~
      
    - HTML İÇerisinde Kullanım: Kısa echo etiketi genellikle HTML içerisinde PHP çıktısı almak için kullanılır. Böylece PHP kodunu iç içe yazarken kodun temiz ve okunabilir olması sağlanır.
      ~~~~~~~
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Example</title>
      </head>
      <body>
          <h1><?= "Hello world" ?></h1>
      </body>
      </html>
      ~~~~~~~
      
      > Burada ```<h1>``` etiketinin içine PHP kullanılarak "Hello world" yazdırılır. ```<?= "Hello world" ?>``` ifadesi, ```<?php echo "Hello world"; ?>``` ile aynı işlevi görür ve "Hello world" metnini tarayıcıda gösterir.

    - Değişkenlerle Kullanımı: Kısa echo etiketi, değişkenleri yazdırmak için kullanılır.
      ~~~~~~~
      <?php
      $isim = "Zehra";
      ?>
      <p>Hello, <?= $isim ?>!</p>
      ~~~~~~~

      > Output: <p>Hello Zehra!</p>
      
    - Koşullu İfadelerle Kullanımı: Kısa echo etiketi koşullu ifadelerle de kullanılabilir.
      ~~~~~~~
      <?php
      $loggedIn = true;
      ?>
      <p>Kullanıcı durumu: <?= $loggedIn ? "Giriş yapmış" : "Giriş yapmamış" ?></p>
      ~~~~~~~

      > Burada ```$loggedIn``` değişkenine göre "Giriş yapmış" veya "Giriş yapmamış" metinlerinden biri yazdırılır.
      > Kısa PHP etiketleri ```(<?)``` bazı sunucu yapılandırmalarında etkin olmayabilir. Bu durumda kısa PHP etiketlerini kullanmak yerine standart PHP etiketlerini kullanmak gerekebilir. Ancak kısa echo etiketi, kısa PHP etiketlerinden farklıdır ve yaygın olarak desteklenir.
      
***
### Print ile Echo'nun farkı nedir?
***
+ ```echo``` ve ```print``` arasında bazı küçük farklar vardır:
    - Hız: ```echo``` biraz daha hızlıdır çünkü dönüş değeri yoktur.
    - Kullanım: ```print``` bir değeri döndürür (1) ve bu yüzden bir ifadenin parçası olarak kullanılabilir. ```echo``` ise dönüş değeri dönmez.
    - Parantez Kullanımı: ```echo``` parantezsiz kullanılabilirken, ```print``` parantez gerektirir.
 
> ```echo```, PHP'de metin ve değişkenlerin değerlerini ekrana yazdırmak için kullanılan basit ama çok güçlü yapıdır. Parantezler gerektirmemesi ve birden fazla değeri yazdırabilmesi gibi özellikleri sayesinde oldukça esnek ve kullanışlıdır.
