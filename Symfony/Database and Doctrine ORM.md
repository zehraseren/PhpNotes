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
 DATABASE_URL="postgresql://symfony_user:symfony_pass@127.0.0.1:5432/symfony_db"
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
![Docker@2x](https://github.com/zehraseren/PhpNotes/assets/94180168/7c5cbb94-2798-4b0a-98c6-12a8cab7d6b5)

#### Docker nedir?
+ Docker, uygulamaları izole edilmiş ortamlarda (konteynerlerde) çalıştırılmasını sağlayan bir platformdur. Docker'ın temel bileşenleri şunlardır:
  - `Image:` Belirli bir uygulamayı çalıştırmak için gereken her şeyi tanımlayan dosyalardır. Örneğin, bir MySQL veya PostgreSQL sunucusu çalıştırmak için önceden oluşturulmuş imajlar vardır.
  - `Container:` İmajları kullanarak oluşturulan ve çalışan izole edilmiş ortamlardır. Bir imaj bir class'a benzetilebilirken, container bu class'ı bir örneği gibidir.

#### Docker'ın Avantajları
+ `İzolasyon:` Her uygulama izole edilmiş bir ortamda çalışır, bu nedenle birbirleriyle veya ana işletim sistemiyle çakışmazlar.
+ `Taşınabilirlik:` Uygulamalar, her işletim sisteminde (Windows, Mac, Linux) aynı şekilde çalışır.
+ `Temizleme Kolaylığı:` Kullanılmayan bir container'ı kaldırarak sistem temiz ve düzenli tutulabilir.

#### Docker Compose
+ Docker Compose, birden fazla container'ı bir araya getirip birlikte çalıştırılmasına olanak tanır. Örneğin, bir PostgreSQL container'ı ve bu veritabanını yönetmek için Adminer gibi bir araç çalıştırılabilir. Docker Compose, bu container'ların birbirleriyle nasıl etkileşime geçeceğini tanımlayan bir `docker-compose.yml` dosyası kullanır.

#### Adım Adım PostgreSQL Kurulumu
##### 1. Docker ve Docker Compose Kurulumu:
   - Docker Compose, Docker ile birlikte gelir ancak gerekirse manuel olarak kurulabilir.
 
 ##### 2. Proje Dizini Oluşturma:
   - Bir proje dizini oluşturulup bu dizin içinde bir `docker-compose.yml` dosyası oluşturulur.
 
 ##### 3. `docker-compose.yml` Dosyasını Yapılandırma
   - Aşağıdaki yapılandırmayı `docker-compose.yml` dosyasına eklenmelidir.
 ~~~~~~~
 version: '3.8'

 services:
   postgres:
     image: postgres:13
     container_name: my_postgres
     environment:
       POSTGRES_DB: symfony_db
       POSTGRES_USER: symfony_user
       POSTGRES_PASSWORD: symfony_pass
     ports:
       - "5432:5432"
     volumes:
       - postgres_data:/var/lib/postgresql/data

   adminer:
     image: adminer
     container_name: my_adminer
     ports:
       - "5432:5432"

 volumes:
   postgres_data:
 ~~~~~~~
 > + `postgres:` PostgreSQL veritabanı sunucusunu çalıştırır.
 > + `adminer:` Web tabanlı veritabanı yönetim aracı Adminer'ı çalıştırır.
 
 ##### 4. Docker Compose ile Konteynerleri Başlatma
   - Termali açılır, proje dizini girilir.
   - Aşağıdaki komut ile container'ler başlatılır.
 ~~~~~~~
 docker-compose up -d
 ~~~~~~~
 > Bu komut, belirtilen hizmetleri (PostgreSQL ve Adminer) arka planda çalıştırır.

 ##### 5. Veritabanına Erişim
   - Adminer'i kullanarak veritabanına erişmek için tarayıcıdan `http://localhost:5432` adresine gidilir.
   - Giriş bilgilerini şu şekilde olmalıdır:
     - Sunucu: `postgres`
     - Kullanıcı Adı: `symfony_user`
     - Parola: `symfony_pass`
     - Veritabanı: `symfony_db`
 > Artık PostgreSQL veritabanı sunucusu çalışıyor ve Adminer aracılığıyla yönetilebilir durumda.

#### Symfony ile Veritabanı Bağlantısı
1. Symfony Projesinde `.env` Dosyasını Düzenleme
+ Symfony projen'n kök dizinindeki `.env` dosyasında aşağıdaki gösterilen satır düzenlenmelidir:
~~~~~~~
DATABASE_URL="postgresql://symfony_user:symfony_pass@127.0.0.1:5432/symfony_db"
~~~~~~~
> PostgreSQL varsayılan portu olan `5432` kullanılır.

2. Doctrine'i Yükleme
+ Doctrine ORM'yi yüklemek için aşağıdaki komut çalıştırılır:
~~~~~~~
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
~~~~~~~

3. Veritabanını ve Varlıkları Oluşturma
+ Veritabanını oluşturmak için:
~~~~~~~
php bin/console doctrine:database:create
~~~~~~~

+ Bir varlık (entity) oluşturmak için:
~~~~~~~
php bin/console make:entity
~~~~~~~

+ Veritabanı şemasını oluşturmak veya güncellemek için:
~~~~~~~
php bin/console doctrine:migrations:diff
php bin/console doctrine:migrations:migrate
~~~~~~~

***
### Running PostgreSQL Server and Connecting from Symfony | PostgreSQL Sunucusunu Çalıştırma ve Symfony'den Bağlanma
##### 1. Doctrine ORM'nin Kurulması
+ Öncelikle, Doctrine ORM'yi Symfony projesine eklenmelidir. Bunu yapmak için aşağıdaki komutu çalıştırılmalıdır:
~~~~~~~
composer require symfony/orm-pack
~~~~~~~
> Komut çalıştırıldıktan sonra, bazı ek yapılandırmalar yapılması gerekmektedir. `composer require` komutu Docker yapılandırmasını güncellemek için bir öneri sunabilir, bu yapılandırmayı manuel olarak yapılması için sonraki adımların yapılması gerekmektedir.

##### 2. Docker Compose Dosyasının Oluşturulması
+ Symfony projesinin kök dizininde `docker-compose.yml` dosyası oluşturulup aşağıdaki şekilde eklenmelidir.
~~~~~~~
version: '3.8'

services:
  postgres:
    image: postgres:14
    container_name: my_postgres
    environment:
      POSTGRES_DB: symfony_db
      POSTGRES_USER: symfony_user
      POSTGRES_PASSWORD: symfony_pass
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  adminer:
    image: adminer
    container_name: my_adminer
    ports:
      - "5432:5432"

volumes:
  postgres_data:
~~~~~~~
 > + `postgres:` PostgreSQL veritabanı sunucusunu çalıştırır.
 > + `adminer:` Web tabanlı veritabanı yönetim aracı Adminer'ı çalıştırır.

##### 3. Docker Container'larını Başlatma
+ Docker Desktop'ın çalıştığından emin olunmalıdır ve ardından terminalde aşağıdaki komut çalıştırılarak container'ları başlatılmalıdır:
~~~~~~~
docker-compose up -d
~~~~~~~
> Bu komut, belirtilen hizmetleri (PostgreSQL ve Adminer) arka planda çalıştırır. Bu işlem, imajları indirip konteynerleri başlatacaktır

##### 4. Veritabanına Erişim
 Adminer'i kullanarak veritabanına erişmek için tarayıcıdan `http://localhost:5432` adresine gidilir.
   - Giriş bilgilerini şu şekilde olmalıdır:
     - Sunucu: `postgres`
     - Kullanıcı Adı: `symfony_user`
     - Parola: `symfony_pass`
     - Veritabanı: `symfony_db`

##### 5. Symfony Projesinde Veritabanı Bağlantısının Yapılandırılması
+ Symfony uygulamasının .env dosyasında veritabanı bağlantı bilgileri tanımlanmalıdır.
~~~~~~~
DATABASE_URL="postgresql://symfony_user:symfony_pass@127.0.0.1:5432/symfony_db"
~~~~~~~

##### 6. Doctrine'i Yapılandırma
+ `config/packages/doctrine.yaml` dosyasını açıp aşağıdaki satır eklenmelidir:
~~~~~~~
doctrine:
    dbal:
        server_version: '16'
~~~~~~~

##### 7. Veritabanı ve Varlıkların (Entities) Oluşturulması
+ Veritabanını oluşturmak için aşağıdaki komutu çalıştırılmalıdır:
~~~~~~~
php bin/console doctrine:database:create
~~~~~~~

+ Yeni bir varlık oluşturmak için:
~~~~~~~
php bin/console make:entity
~~~~~~~

+ Veritabanı şemasını oluşturmak veya güncellemek için:
~~~~~~~
php bin/console doctrine:migrations:diff
php bin/console doctrine:migrations:migrate
~~~~~~~

##### 8. Symfony'nin Bağlantıyı Doğrulaması
+ Symfony uygulamasını çalıştırmak için:
~~~~~~~
symfony server:start
~~~~~~~
> Ardından tarayıcı açılmalıdır. Profiler'ı kullanarak Doctrine bağlantısının başarılı olup olmadığını kontrol edilebilir. Herhangi bir hata görünmüyorsa, bağlantı başarılı demektir.

***
### Generating and Understanding Entities | Varlıkları Oluşturma ve Anlama 
+ Bir entity oluşturulurken, veritabanında bir tabloya karşılık gelecek ve Doctrine ORM tarafından yönetilir.

##### 1. Entity Oluşturma
+ Symfony'nin `make:entity` komutu kullanılarak yeni bir entity oluşturulur. Terminalde aşağıdaki komutu çalıştırılır:
~~~~~~~
php bin/console make:entity
~~~~~~~

+ Ardından entity için bir isim girilir. Örneğin entity ismi olarak `MicroPost` adını verilsin.
~~~~~~~
Class name of the entity to create or update (e.g. BraveCarrot):
> MicroPost
~~~~~~~
> Entity ve repository class'ları otomatik olarak oluşturulacak.

##### 2. Entity İçin Alanlar (Fields) Eklemek
+ Entity'ye bazı alanlar eklenmelidir. `make:entity` komutu sırasıyla bu alanların eklenmesi için rehberlik edecektir:
  - `title` alanı (string, nullable olamaz)
  - `text` alanı (string, nullable olabilir)
  - `created_at` alanı (datetime, nullable olabilir)

###### Örnek girişler: 
~~~~~~~
New property name (press <return> to stop adding fields):
> title

Field type (enter ? to see all types) [string]:
> string

Field length [255]:
>

Can this field be null in the database (nullable) (yes/no) [no]:
> no

New property name (press <return> to stop adding fields):
> text

Field type (enter ? to see all types) [string]:
> string

Field length [255]:
> 1000

Can this field be null in the database (nullable) (yes/no) [no]:
> yes

New property name (press <return> to stop adding fields):
> created_at

Field type (enter ? to see all types) [string]:
> datetime

Can this field be null in the database (nullable) (yes/no) [no]:
> yes

New property name (press <return> to stop adding fields):
>
~~~~~~~
> Bu adımlar, MicroPost entity'sine `title`, `text` ve `created_at` alanlarını ekler.

##### 3. Entity Dosyasını İncelemek
+ Oluşturulan `MicroPost` entity'sini ìncelendiğinde bu dosya `src/Entity/MicroPost.php` altında bulunur.
~~~~~~~
<?php

namespace App\Entity;

use App\Repository\MicroPostRepository;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity(repositoryClass: MicroPostRepository::class)]
class MicroPost
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;

    #[ORM\Column(type: 'string', length: 255)]
    private $title;

    #[ORM\Column(type: 'string', length: 1000, nullable: true)]
    private $text;

    #[ORM\Column(type: 'datetime', nullable: true)]
    private $createdAt;

    // Getters and setters...
}
~~~~~~~

+ Bu class'ta:
  - `#[ORM\Entity]:` Class'ın bir entity olduğunu belirtir.
  - `#[ORM\Id]:` Bu alanın birincil key olduğunu belirtir.
  - `#[ORM\GeneratedValue]:` Bu alanın otomatik olarak arttırıldığını belirtir.
  - `#[ORM\Column]:` Bu alanın bir veritabanı kolonu olduğunu belirtir ve tür, uzunluk gibi özellikleri belirler.

##### 4. Migration Oluşturma ve Uygulama
+ Entity class'ını oluşturduktan sonra, veritabanında karşılık gelen tabloyu oluşturmak için bir migration oluşturulmalıdır.

###### Migration oluşturmak için:
~~~~~~~
php bin/console make:migration
~~~~~~~

###### Migration'ı uygulamak için:
~~~~~~~
php bin/console doctrine:migrations:migrate
~~~~~~~
> Bu komut, yeni bir tablo oluşturacak ve bu tabloya `MicroPost` entity'sinin alanlarını ekleyecektir.

##### 5. Veritabanı Bağlantısını Test Etme
+ Veritabanı bağlantısının çalıştığını doğrulamak için Symfony uygulaması çalıştırılır:
~~~~~~~
symfony server:start
~~~~~~~
> Uygulama tarayıcıda açılıp Symfony Profiler'ı kontrol edilmelidir. Doctrine bağlantısının başarılı olduğu görülmelidir.

***
### Doctrine Migrations | Doctrine Geçişleri
+ Symfony'de veritabanı değişikliklerini yönetmek ve bu değişiklikleri izlemek için migration'lar kullanılır.
+ Migration'lar, veritabanı şemasındaki değişiklikleri kodla tanımlanmasına olanak tanır ve bu değişiklikleri uygulamak veya geri almak için komutlar sağlar. 

##### 1. Migration Oluşturma
+ Öncelikle, `MicroPost` entity'si için bir migration oluşturulmalıdır. Terminalde aşağıdaki komut çalıştırılmalıdır:
~~~~~~~
php bin/console make:migration
~~~~~~~
> Bu komut, migrations klasörü içinde yeni bir migration dosyası oluşturacaktır. Migration dosyasının adı, oluşturulma tarihine ve saatine göre belirlenir.

##### 2. Migration Dosyasını İncelemek
+ `migrations` klasöründeki yeni migration dosyasını açılır. Açılan dosyada `up` ve `down` method'larına sahip bir class içerir.
  - `up` method'u, migration'ı uygulandığında çalıştırılacak SQL komutlarını içerir.
  - `down` method'u ise migration'u geri almak için kullanılır.

###### Örnek migration dosyası:
~~~~~~~
<?php

declare(strict_types=1);

namespace DoctrineMigrations;

use Doctrine\DBAL\Schema\Schema;
use Doctrine\Migrations\AbstractMigration;

final class Version20230601123000 extends AbstractMigration
{
    public function getDescription(): string
    {
        return 'Create the micro_post table';
    }

    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE TABLE micro_post (id INT AUTO_INCREMENT NOT NULL, title VARCHAR(255) NOT NULL, text VARCHAR(1000) DEFAULT NULL, created_at DATETIME DEFAULT NULL, PRIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE `utf8mb4_unicode_ci` ENGINE = InnoDB');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('DROP TABLE micro_post');
    }
}
~~~~~~~
+ Bu dosyada:
  - `getDescription():` Migration'un ne yaptığını açıklayan bir açıklama döner.
  - `up(Schema $schema):` Migration'u uygulamak için gereken SQL komutlarını içerir.
  - `down(Schema $schema):` Migration'u geri almak için gereken SQL komutlarını içerir.

##### 3. Migration'u Uygulama
+ Migration'u uygulamak için aşağıdaki komutu çalıştırılır:
~~~~~~~
php bin/console doctrine:migrations:migrate
~~~~~~~
> Bu komut, veritabanında belirtilen değişiklikleri uygular. Komut çalıştırıldığında, değişikliklerin uygulanacağını belirten bir uyarı alınır ve "yes" yazarak onaylanır.

##### 4. Migration Durumunu Kontrol Etme
+ Migration'ların durumunu kontrol etmek için aşağıdaki komutu kullanılır:
~~~~~~~
php bin/console doctrine:migrations:status
~~~~~~~
> Bu komut, mevcut migration'ların durumunu, kaç migration'ın uygulandığını ve kaç tane migration'ın uygulanmayı beklediğini gösterir.

##### 5. Migration'u Geri Alma
+ Eğer bir migration geri almak istenirse aşağıdaki komut kullanılır:
~~~~~~~
php bin/console doctrine:migrations:migrate prev
~~~~~~~
> Bu komut, son uygulanan migration'ı geri alır. `doctrine:migrations:migrate` komutuna `--help` parametresini eklenerek daha fazla bilgi edinilebilir.

##### 6. Etkileşimsiz Migration
+ Migration'lar etkileşimsiz bir şekilde uygulanmak istenirse, `--no-interaction` parametresi kullanılabilir:
~~~~~~~
php bin/console doctrine:migrations:migrate --no-interaction
~~~~~~~
> Bu komut, herhangi bir kullanıcı etkileşimi gerektirmeden migration'ları uygular.

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



