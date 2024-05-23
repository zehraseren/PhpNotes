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

+ Constant'lar, bir kez tanımlandıktan sonra isimleri ve değerleri sabit kalır. Ancak, constant'ların isimlerini dinamik olarak oluşturmak veya erişmek için bazı teknikler kullanılabilir.
    - ```Sabit Tanımlama (Dil seçimi örneği)```
    ~~~~~~~
    define("GREETING_EN", "Hello");
    define("GREETING_FR", "Bonjour");
    define("GREETING_ES", "Hola");
    ~~~~~~~
    
    - ```constant() Fonksiyonu ile Dinamik Sabit Kullanımı:```
    ~~~~~~~
    $lang = "EN";
    $constantName = "GREETING".$lang;
    echo constant($constantName);
    ~~~~~~~
    > Output: Hello
    
    ~~~~~~~
    $lang = "FR";
    $constantName = "GREETING".$lang;
    echo constant($constantName);
    ~~~~~~~
    > Output: Bonjour
    
    - Başka bir örnek:
    ~~~~~~~
    function getGreeting($language) {
        $constantName = "GREETING_".strtoupper($language);
        if(defined($constantName)) {
            return constant($constantName);
        } else {
            return "Language not supported."
        }
    }

    echo getGreeting("ES");
    echo getGreeting("DE");
    ~~~~~~~
    > Output: Hola
    
    > Output: Language not supported.

    > ```strtoupper``` fonksiyonu, bir string'in tüm karakterlerini büyük harfe dönüştürmek için kullanılır. Bu fonksiyon, string manipülasyonları ve karşılaştırmaları için güçlü ve kullanışlı bir araçtır. Özellikle kullanıcı girdileri, sabit tanımlamaları ve büyük/küçük harf duyarsız işlemler için sıkça kullanılır.

+ Bazen constant'ların belirli koşullara göre tanımlanması gerekebilir. Bu durumda, defined() fonksiyonunu kullanarak bir constant'ın tanımlı olup olmadığını kontrol edebilir ve tanımlı değilse tanımlanabilir.
~~~~~~~
if (!defined("GREETING_IT")) {
    define("GREETING_IT", "Ciao");
}

echo GREETING_IT;
~~~~~~~
> Output: Ciao

> Yukarıdaki örnekte dikkat edilmesi noktalar:
> + ```defined()``` fonksiyonu, belirtilen sabit tanımlıysa ```true```, tanımlı dğeilse ```false``` döner.
> + Buradaki ```!``` operatörü, mantıksal ```NOT``` operatörüdür ve ```defined()``` fonksiyonunun sonucunu tersine çevirir.
>     - Eğer ```GREETING_IT``` sabiti tanımlı değilse, ```!defined(GREETING_IT)``` ifadesi ```true``` döner ve ```if``` bloğu içindeki kod çalır.
>     - Eğer ```GREETING_IT``` sabiti tanımlıysa, ```!defined(GREETING_IT)``` ifadesi ```false``` döner ve ```if``` bloğu içindeki kod çalışmaz (yukarıdaki örnekte if bloğunda sabit tanımlı değildir).
> + ```define("GREETING_IT","Ciao")``` ifadesinde sabit tanımlıdır ve değeri "Ciao"dur.
>     - Eğer ```GREETING_IT``` sabiti daha önce tanımlanmışsa, ```define()``` fonksiyonu tekrar çalıştırılamaz ve sabitin değeri değiştirilemez.

***
#### Dikkat edilmesi gerekenler
***
+ ```Performans:``` constant() fonksiyonu her çağırıldığımda sabitin ismini çözümlemesi gerekitğinden, sabit ismine doğrudan erişimden biraz daha yavaştır. Ancak bu fark genellikle ihmal edilebilir düzeydedir.
+ ```Hata Kontrolü:``` Dinamik sabit isimleri oluşturulurken hatalara karşı dikkatli olunmalıdır. Sabit ismi yanlış oluşturulursa, ```constant()``` fonksiyonu bir hata döndürür.
+ ```Güvenlik:``` Kullanıcı girdilerine dayalı olarak dinamik sabit isimleri oluşturulurken dikkatli olunmalıdır. Kullanıcı girdilerinin doğruluğunu ve güvenliğini sağlamak önemlidir.

***
### Constant'lar ne zaman kullanılır?
***
+ Sabitler, belirli değerlerin sabit kalmasını sağlamak ve kodun okunabilirliğini, bakımını ve güvenliğini artırmak için kullanılır. İşte sabitlerin ne zaman ve neden kullanılacağına dair bazı durumlar:
    - ```Konfigürasyon Ayarları:``` Uygulamanın yapılandırma ayarlarını sağlamak için kullanılabilir. Özellikle database bağlantı bilgileri, API anahtarları ve diğer yapıladırma parametreleri için yararlıdır.
      ~~~~~~~
      define("DB_HOST", "localhost");
      define("DB_USER", "username");
      define("DB_PASS", "password");
      define("DB_NAME", "my_database");
      ~~~~~~~
      
    - ```Sabit Değerler:``` Uygulama boyunca değişmeyen sabit değerleri saklamak için kulanılabilir. Örneğin, matematiksel sabitler veya uygulamanın iş mantığıyla ilgili sabitler.
      ~~~~~~~
      define("PI", "3.14159");
      define("EARTH_GRAVITY", "9.81");
      ~~~~~~~
      
    - ```Durum ve Rol Tanımları:``` Uygulamanın belirli durumlarını veya rolleri tanımlamak için kullanılabilir. Kodun okunabilirliğini artırır ve hata yapma olasılığını azaltır.
      ~~~~~~~
      define("STATUS_ACTIVE", 1);
      define("STATUS_INACTIVE", 0);
      define("ROLE_ADMIN", "admin");
      define("ROLE_USER", "user");
      define("ROLE_GUEST", "guest");
      ~~~~~~~
      
    - ```Dosya Yolları:``` Dosya ve dizin yollarını saklamak için kullanılabilir. Özellikle dosya işlemlerinde ve include/require işlemlerinde faydalıdır.
      ~~~~~~~
      define("UPLOAD_DIR", "/var/www/uploads/");
      define("LOG_DIR", "/var/log/myapp/");
      ~~~~~~~
      
    - ```Sürüm Bilgileri:``` Uygulamanın sürüm bilgilerini saklamak için kullanılabilir. Uygulamanın farklı sürümlerinde kullanılabilir ve versiyon yönetimini kolaylaştırır. 
      ~~~~~~~
      define("APP_VERSION", "1.0.0");
      ~~~~~~~
      
    - ```Magic Constants:``` PHP'de tanımlanmış bazı sabitler (magic constants) vardır. Bunlar, dosya yolu, satır numarası, fonksiyon adı gibi dinamik bilgileri sağlar ve genellikle hata ayıklama veya loglama amacıyla kullanılır.
      ~~~~~~~
      echo "Bu satır numarası: ".__LINE__;
      ~~~~~~~
      > Output: Mevcut satır numarasını verir.
      
      ~~~~~~~
      echo "Bu dosya: ".__FILE__;
      ~~~~~~~
      > Output: Mevcut dosyanın tam yolunu verir.
      
    - ```Güvenlik:``` Sabitlerü güvenlik açısından önemli bilgileri saklamak için kullanılabilir. Örneğin, ```sabitler kullanılarak kritik değerlerin yanlışlıkla değiştirilmesi önlenir.```
    
    - ```Çok Dilde Destek:``` Dil bağımsız mesajlar veya sabit metinler için kullanılabilir. Uygulamanın farklı dillerde desteklenmesini kolaylaştırır.
      ~~~~~~~
      define("GREETING_EN", "Hello");
      define("GREETING_FR", "Bonjour");
      define("GREETING_ES", "Hola");
      ~~~~~~~

***
#### Peki avantajları nelerdir?
***
+ ```Değişmezlik:``` Bir kez tanımlandığında, değeri değiştirilemez, bu da tutarlılığı sağlar.
+ ```Global Erişim:``` Tanımlandıkları yerden bağımsız olarak tüm uygulama boyunca erişilebilir.
+ ```Perfomans:``` Değişkenlerden biraz daha hızlıdır çünkü OHO, sabitleri doğrudan bellekte saklar.
+ ```Okunabilirlik:``` Anlamlı isimlerle tanımlanan sabitler, kodun okunabilirliği ve anlaşılabilirliğini artırır.
+ ```Hata Azaltma:``` Sabit değerler kullanmak, kodun çeşitli yerlerinde aynı değeri tekrar tekrar yazmaktan kaynaklanan hataları azaltır.
