### Databases and Doctrine | Veritabanları ve Doctrine
+ Veritabanları, bir uygulamanın veri yönetimi için temel bileşenlerdir. Farklı veritabanı sistemleri ve bu sistemlerin farklı kullanım amaçları bulunmaktadır.

![Databases   Doctrine@2x](https://github.com/zehraseren/PhpNotes/assets/94180168/0690675b-de54-45f3-8f76-c089029c0265)

#### Veritabanı Sistemleri
+ Veritabanı sistemleri iki ana kategoriye ayrılır:
1. `İlişkisel Veritabanları:` Bu tür veritabanları tablolara dayalıdır. Örneğin:
  -  PostgreSQL
  -  MySQL

2. `NoSQL Veritabanları:` Bu tür veritabanları tablo yerine farklı veri modelleri kullanır. Örneğin:
  - Redis
  - MongoDB

+ İlişkisel veritabanları, tablolar etrafında yapılandırılmıştır ve bu tablolar sütunlardan oluşur. Her sütun belirli bir veri türüne sahiptir. Örneğin, bir blog gönderisini temsil etmek istiyorsanız, bir `blog_posts` tablosu oluşturulur. Bu tablo şu sütunlara sahip olabilir:
  - title
  - content
  - created_at

+ Her tablo satırı, her sütun için bir değer içerir. Genel olarak, bu oldukça basit bir konsepttir.

#### SQL ve Symfony'de Doctrine ORM
+ İlişkisel veritabanları ile çalışmak için SQL bilinmesi gerekmektedir. SQL'i bilmek kesinlikle faydalıdır, ancak Symfony ile çalışırken SQL'i çok iyi bilinmesine gerek yoktur. Bunun yerine, Symfony'de veritabanı ile etkileşime geçmek için Doctrine ORM (Object-Relational Mapping) kullanılır.
+ Doctrine ORM, Symfony uygulamaları için varsayılan nesne-ilişkisel haritalama aracıdır. ORM'nin amacı, veritabanı ile nesne yönelimli programlama arasında bir köprü kurmaktır. Başka bir deyişle, `veritabanı tablolarını PHP class'larına ve sütunları PHP class alanlarına (properties) eşler.`
+ Doctrine ORM'nin çalışma mantığı şu şekildedir:
  - Her veritabanı tablosu için bir PHP class'ı oluşturulur.
  - Bu class'lar içinde tablo sütunlarını temsil eden alanlar bulunur.
  - Her class örneği (instance), bir tablo satırını temsil eder.
+ Yeni bir tablo satırı eklemek için yeni bir class örneği oluşturur ve bir `save` method'u çağırılır. Bu, veritabanında yeni bir satır oluşturur.

#### Symfony ile Veritabanı Bağlantısı ve Kurulumu
+ Veritabanı ile çalışmaya başlamadan önce, veritabanı sunucusunun yerel olarak çalıştırılması ve Symfony uygulamasının bu veritabanına bağlanması gerekmektedir. Adım adım yapılması gerekenler şunlardır:
 1. `Veritabanı Sunucusunu Çalıştırma:` PostgreSQL veya MySQL gibi bir veritabanı sunucusu çalıştırılır. Bu, Docker kullanarak veya doğrudan bilgisayara kurularak yapılabilir.
 2. `Symfony Uygulamasını Yapılandırma:` Symfony uygulamasının `.env` dosyasında veritabanı bağlantı bilgileri tanımlanmalıdır.
 ###### .env
 ~~~~~~~
 DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name"
 ~~~~~~~
 > Burada:
 > + `db_user:` Veritabanı kullanıcı adı
 > + `db_password:` Veritabanı şifresi
 > + `127.0.0.1:` Veritabanı sunucusunun adresi
 > + `3306:` Veritabanı sunucusunun portu
 > + `db_name:` Kullanılacak veritabanının adı
 
 3. `Doctrine Kurulumu:` Doctrine'i projeye dahil etmek için aşağıdaki komutu çalıştırılmalıdır.
 ~~~~~~~
 composer require symfony/orm-pack
 composer require --dev symfony/maker-bundle
 ~~~~~~~
 
 4. `Veritabanı Yapılandırması:` Veritabanını oluşturmak ve yapılandırmak için aşağıdaki komutları kullanılmalıdır:
 ~~~~~~~
 php bin/console doctrine:database:create
 php bin/console make:entity
 php bin/console doctrine:migrations:diff
 php bin/console doctrine:migrations:migrate
 ~~~~~~~
 > Bu komutlar veritabanını oluşturur, entity class'larını tanımlar ve gerekli tabloları veritabanına ekler.

***
### What is Docker? | Docker nedir?
+
~~~~~~~
~~~~~~~
>

***
### Running MySQL Server and Connecting from Symfony | MySQL Sunucusunu Çalıştırma ve Symfony'den Bağlanma
+
~~~~~~~
~~~~~~~
>

***
### Generating and Understanding Entities | Varlıkları Oluşturma ve Anlama 
+
~~~~~~~
~~~~~~~
>

***
### Doctrine Migrations | Doctrine Geçişleri
+
~~~~~~~
~~~~~~~
>

***
### Doctrine Fixtures (Fake Data) | Doctrine Fikstürleri (Sahte Veriler)
+
~~~~~~~
~~~~~~~
>

***
### No More Add or Save Method (Symfony 6.3 Changes) | Artık Ekle veya Kaydet Yöntemi Yok (Symfony 6.3 Değişiklikleri)
+
~~~~~~~
~~~~~~~
>

***
### Doctrine Repostories (Fetching, Storing, Updating ½ Deleting Data)
+
~~~~~~~
~~~~~~~
>

***
### No Extra Bundle Needed! (Symfony 6.2 Changes) | Ekstra Pakete Gerek Yok! (Symfony 6.2 Değişiklikleri)
+
~~~~~~~
~~~~~~~
>

***
### Param Converter (Auto Fetching Entity) | Param Dönüştürücü (Otomatik Getiren Varlık)
+
~~~~~~~
~~~~~~~
>

***

### Project - Getting Posts From Databases | Proje - Veritabanlarından Gönderi Alma
+
~~~~~~~
~~~~~~~
>



