***
### Değişken ataması
***
+ Aşağıdaki örnekte ```$firstName``` değişkenine string bir değer atanmıştır. Daha sonra bu değişkene yeni bir değer atandığında önceki değer kaybolur ve değişken yeni değeri taşır.
~~~~~~~
<?php

$firstName= "Zehra";
$firstName= "Zehra Şeren";

echo $firstName;
~~~~~~~

> Output: Zehra Şeren

***
### Define Function ('name', 'value')
***
+ ```define()``` fonksiyonu, bir sabit (constant) tanımlamak için kullanılır. Sabitler, bir kez tanımlandıktan sonra değeri değiştirilemeyen ve herhangi bir değişken işaretçisi kullanmadan (örneğin ```$``` olmadan) doğrudan erişilebilen değerlerdir.
+ ```Sabitler genellikle konfigürasyon ayarlarını, sabit değerleri ve değiştirilemez bilgileri tutmak için kullanılır.``` 
+ ```define()``` fonksiyonu iki ana parametre alır:
    - ```Sabitin Adı:``` Sabit adı büyük harflerle yazılır. Genellikle konvansiyon olarak büyük harf kullanılır, ancak zorundu değildir.
    - ```Sabitin Değeri:``` Sabitin tutacağı değer.
    ~~~~~~~
    define("SITE_NAME", "My Website");
    echo SITE_NAME;
    ~~~~~~~
    
    > Output: My Website

+ Sabitlerin özellikleri:
    - ```Değiştirilemezler:``` Bir sabit tanımlandıktan sonra değeri değiştirilemez. Bu, sabitlerin güvenilir ve sabit bir referans noktası olmasını sağlar.
    ~~~~~~~
    define("GREETING", "Hello, World!");
      
    // GREETING = "Hi there!"; // Hatalı, sabitler yeniden atanamaz
    ~~~~~~~
    - ```Global Erişim:``` Sabitler tüm PHP betiği boyunca her yerden erişilebilir. Fonksiyonlar ve class'lar dahil her yerde kullanılabilirler.
    ~~~~~~~
    define("SITE_URL", "https://www.example.com");

    function printSiteUrl() {
        echo SITE_URL;
    }

    printSiteUrl();
    ~~~~~~~
    > Output: https://www.example.com

***
### Magic Constans (Sihirli Sabitler)
***
+ PHP ayrıca bazı özel sabitler sağlar. ```Bunlar genellikle kodun belirli bilgilerini almak için kullanılır ve her zaman çift tırnak içinde yazılırlar:```
    - ```__LINE__:``` Dosyada geçerli satır numarası.
    - ```__FILE__:``` Dosyanın tam yolu ve dosya adı.
    - ```__DIR__:``` Dosyanın bulunduğu dizin.
    - ```__FUNCTION__:``` Fonksiyonun adı.
    - ```__CLASS__:``` Sınıfın adı.
    - ```__METHOD__:``` Sınıf yönetiminin adı.
    - ```__NAMESPACE__:``` Geçerli namespace adı.
  
  ~~~~~~~
  echo "Line number: ".__LINE__; (2. satırda olduğu varsayılır)
  ~~~~~~~  
  > Output: Line number: 2

  ~~~~~~~
  echo "File path: ".__FILE__;
  ~~~~~~~
  > Output: File path: /path/to/your/file.php
