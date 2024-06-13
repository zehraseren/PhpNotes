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

+ İlişkisel veritabanları, tablolar etrafında yapılandırılmıştır ve bu tablolar sütunlardan oluşur. Her sütun belirli bir veri türüne sahiptir. Örneğin, bir blog gönderisini oluşturulmak istenirse, bir `blog_posts` tablosu oluşturulur. Bu tablo şu sütunlara sahip olabilir:
  - title
  - content
  - created_at

+ Her tablo satırı, her sütun için bir değer içerir. Genel olarak, bu oldukça basit bir konsepttir.

#### SQL ve Symfony'de Doctrine ORM
+ İlişkisel veritabanları ile çalışmak için SQL bilinmesi gerekmektedir. SQL'i bilmek kesinlikle faydalıdır, ancak Symfony ile çalışırken SQL'i çok iyi bilinmesine gerek yoktur. Bunun yerine, Symfony'de veritabanı ile etkileşime geçmek için Doctrine ORM (Object-Relational Mapping) kullanılır.
+ Doctrine ORM, Symfony uygulamaları için varsayılan nesne-ilişkisel haritalama aracıdır. ORM'nin amacı, veritabanı ile nesne yönelimli programlama arasında bir köprü kurmaktır. Başka bir deyişle, `veritabanı tablolarını PHP class'larına, sütunları PHP class alanlarına (properties) eşler.`
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
 > + `5432:` Veritabanı sunucusunun portu
 > + `db_name:` Kullanılacak veritabanının adı
 
 3. `Doctrine Kurulumu:` Doctrine'i projeye dahil etmek için aşağıdaki komut çalıştırılmalıdır.
 ~~~~~~~
 composer require symfony/orm-pack
 composer require --dev symfony/maker-bundle
 ~~~~~~~
 
 4. `Veritabanı Yapılandırması:` Veritabanını oluşturmak ve yapılandırmak için aşağıdaki komutlar kullanılmalıdır.
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
+ Docker, uygulamaları izole edilmiş ortamlarda (container'larda) çalıştırılmasını sağlayan bir platformdur. Docker'ın temel bileşenleri şunlardır:
  - `Image:` Belirli bir uygulamayı çalıştırmak için gereken her şeyi tanımlayan dosyalardır. Örneğin, bir MySQL veya PostgreSQL sunucusu çalıştırmak için önceden oluşturulmuş imajlar vardır.
  - `Container:` İmajları kullanarak oluşturulan ve çalışan izole edilmiş ortamlardır. Bir imaj bir class'a benzetilebilirken, container bu class'ın bir örneği gibidir.

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
   - Termal açılır, proje dizini girilir.
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
+ Symfony projesinin kök dizinindeki `.env` dosyasında aşağıdaki gösterilen satır düzenlenmelidir:
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
+ Docker Desktop'ın çalıştığından emin olunmalıdır ve ardından terminalde aşağıdaki komut çalıştırılarak container'lar başlatılmalıdır:
~~~~~~~
docker-compose up -d
~~~~~~~
> Bu komut, belirtilen hizmetleri (PostgreSQL ve Adminer) arka planda çalıştırır. Bu işlem, imajları indirip container'ları başlatacaktır.

##### 4. Veritabanına Erişim
+ Adminer'ı kullanarak veritabanına erişmek için tarayıcıdan `http://localhost:5432` adresine gidilir.
+ Giriş bilgilerini şu şekilde olmalıdır:
  - Sunucu: `postgres`
  - Kullanıcı Adı: `symfony_user`
  - Parola: `symfony_pass`
  - Veritabanı: `symfony_db`

##### 5. Symfony Projesinde Veritabanı Bağlantısının Yapılandırılması
+ Symfony uygulamasının `.env` dosyasında veritabanı bağlantı bilgileri tanımlanmalıdır.
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
+ Symfony'nin `make:entity` komutu kullanılarak yeni bir entity oluşturulur. Terminalde aşağıdaki komut çalıştırılır:
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
+ Oluşturulan `MicroPost` entity'si ìncelendiğinde bu dosya `src/Entity/MicroPost.php` altında bulunur.
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
+ Doctrine Repositories, veritabanındaki tabloyu temsil eden entity'lere veri eklemek, güncellemek, silmek ve verileri sorgulamak için kullanılır.

##### 1. Repository Sınıfının Tanıtımı
+ Doctrine Repositories, belirli bir entity için veri sorgulama işlemlerini gerçekleştirir.
+ Symfony, entity için otomatik olarak bir repository class'ı oluşturur. Örneğin, `MicroPost` entity'si için `MicroPostRepository` adlı bir class oluşturulur.

###### src/Repository/MicroPostRepository.php
~~~~~~~
<?php

namespace App\Repository;

use App\Entity\MicroPost;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

/**
 * @extends ServiceEntityRepository<MicroPost>
 *
 * @method MicroPost|null find($id, $lockMode = null, $lockVersion = null)
 * @method MicroPost|null findOneBy(array $criteria, array $orderBy = null)
 * @method MicroPost[]    findAll()
 * @method MicroPost[]    findBy(array $criteria, array $orderBy = null, $limit = null, $offset = null)
 */
class MicroPostRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, MicroPost::class);
    }

    // Add your custom methods here
}
~~~~~~~
+ Bu class, `ServiceEntityRepository` class'ını genişletir ve `MicroPost` entity'si ile ilgili temel veri erişim işlemlerini sağlar. Bu class, veritabanı sorguları için çeşitli önceden tanımlanmış method'lara sahiptir:
  - `find($id):` Belirtilen ID'ye sahip entity'yi bulur.
  - `findOneBy(array $criteria):` Belirtilen kriterlere sahip tek bir entity'yi bulur.
  - `findAll():` Tüm entity'leri döner.
  - `findBy(array $criteria):` Belirtilen kriterlere sahip entity'leri bulur.

##### 2. Repository Kullanarak Veri Sorgulama
+ `MicroPostRepository` class'ını kullanarak bazı veriler sorgulanabilir. Bu işlemleri gerçekleştirmek için bir controller oluşturulur.
~~~~~~~
php bin/console make:controller MicroPostController
~~~~~~~

+ Bu komut, `src/Controller/` klasöründe `MicroPostController.php` adlı yeni bir controller dosyası oluşturur. Bu dosyanın içeriği aşağıdaki gibi düzenlenmelidir:
~~~~~~~
<?php

namespace App\Controller;

use App\Repository\MicroPostRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/microposts', name: 'app_microposts')]
    public function index(MicroPostRepository $microPostRepository): Response
    {
        // Find all micro posts
        $microPosts = $microPostRepository->findAll();

        // Render a template and pass the micro posts to it
        return $this->render('micro_post/index.html.twig', [
            'micro_posts' => $microPosts,
        ]);
    }
}
~~~~~~~
+ Yukarıdaki kodda:
  - `/microposts` rotası tanımlanmıştır.
  - `MicroPostRepository` [dependency injection](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Dependency%20Injection.md) yoluyla alınmıştır.
  - Tüm `MicroPost` kayıtları `findAll()` method'u ile sorgulanmış ve bir Twig şablonuna (`micro_post/index.html.twig`) geçirilmiştir.

##### 3. Twig Şablonu Oluşturma
+ `templates/micro_post/index.html.twig` dosyasını oluşturulur ve içeriği aşağıdaki gibi düzenlenir:
~~~~~~~
{% extends 'base.html.twig' %}

{% block title %}Micro Posts{% endblock %}

{% block body %}
    <h1>Micro Posts</h1>
    <ul>
        {% for microPost in micro_posts %}
            <li>
                <strong>{{ microPost.title }}</strong> - {{ microPost.text }} <em>({{ microPost.createdAt|date('Y-m-d H:i') }})</em>
            </li>
        {% endfor %}
    </ul>
{% endblock %}
~~~~~~~
> Bu şablon, `MicroPost` verilerini listelemek için kullanılır.

##### 4. Uygulamayı Test Etme
+ Symfony sunucusu başlatılır.
~~~~~~~
symfony server:start
~~~~~~~
+ Tarayıcıda `http://localhost:5432/microposts` adresine gidilir ve `MicroPost` verilerinin listelendiği kontrol edilir.

***
### No More Add or Save Method (Symfony 6.3 Changes) | Artık Ekle veya Kaydet Yöntemi Yok (Symfony 6.3 Değişiklikleri)
+ Symfony 6.3 framework'ünün versiyonuyla birlikte bazı önemli değişiklikler yapılmıştır ve bunlardan biri de repository class'larındaki `add` veya `save` method'larının kaldırılmasıdır.
+ Öncelikle, yukarıda oluşturulan `MicroPost` entity'sine veri eklemek için kullanılan `AppFixtures` class'ına bir göz atıldığında:
~~~~~~~
<?php

namespace App\DataFixtures;

use App\Entity\MicroPost;
use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;

class AppFixtures extends Fixture
{
    public function load(ObjectManager $manager): void
    {
        $microPost1 = new MicroPost();
        $microPost1->setTitle('Welcome to Symfony')
                   ->setText('This is the first post')
                   ->setCreated(new \DateTime());

        $manager->persist($microPost1);

        $microPost2 = new MicroPost();
        $microPost2->setTitle('Learning Symfony')
                   ->setText('This is the second post')
                   ->setCreated(new \DateTime());

        $manager->persist($microPost2);

        $manager->flush();
    }
}
~~~~~~~

#### Yeni Veritabanı Kaydı Oluşturma
+ Symfony 6.3 ile birlikte  veri eklemek için `EntityManager`'ı kullanılması gerekmektedir. `MicroPost` entity'sine yeni bir kayıt eklemek için bir controller örneği üzerindenv yapılışı:
  - Öncelikle, `MicroPostController` controller'ı oluşturulsun:
  ~~~~~~~
  php bin/console make:controller MicroPostController
  ~~~~~~~
  - `MicroPostController.php` dosyasının içeriği aşağıdaki gibi düzenlenir:
  ~~~~~~~
  <?php

  namespace App\Controller;
  
  use App\Entity\MicroPost;
  use Doctrine\ORM\EntityManagerInterface;
  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
  use Symfony\Component\HttpFoundation\Request;
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\Routing\Annotation\Route;
  
  class MicroPostController extends AbstractController
  {
      #[Route('/micropost/new', name: 'app_micropost_new', methods: ['POST'])]
      public function new(Request $request, EntityManagerInterface $entityManager): Response
      {
          $microPost = new MicroPost();
          $microPost->setTitle($request->request->get('title'))
                    ->setText($request->request->get('text'))
                    ->setCreated(new \DateTime());
  
          $entityManager->persist($microPost);
          $entityManager->flush();
  
          return new Response('MicroPost created successfully.');
      }
  }
  ~~~~~~~
  > `EntityManagerInterface` aracılığıyla `persist` ve `flush` method'ları kullanılarak yeni bir `MicroPost` kaydı eklenir.

#### Mevcut Veritabanı Kaydını Güncelleme
+ Mevcut bir kaydı güncellemek için de benzer bir yaklaşım kullanılır. Ancak burada `persist` method'unu çağırılmasına gerek yoktur; sadece değişiklikleri yapıp `flush` method'unu çağırmak yeterlidir.
+ `MicroPostController` içerisine güncelleme method'u eklendiğinde:
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\MicroPost;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/new', name: 'app_micropost_new', methods: ['POST'])]
    public function new(Request $request, EntityManagerInterface $entityManager): Response
    {
        $microPost = new MicroPost();
        $microPost->setTitle($request->request->get('title'))
                  ->setText($request->request->get('text'))
                  ->setCreated(new \DateTime());

        $entityManager->persist($microPost);
        $entityManager->flush();

        return new Response('MicroPost created successfully.');
    }

    #[Route('/micropost/{id}/edit', name: 'app_micropost_edit', methods: ['POST'])]
    public function edit(int $id, Request $request, EntityManagerInterface $entityManager): Response
    {
        $microPost = $entityManager->getRepository(MicroPost::class)->find($id);

        if (!$microPost) {
            throw $this->createNotFoundException('No MicroPost found for id: '.$id);
        }

        $microPost->setTitle($request->request->get('title'))
                  ->setText($request->request->get('text'));

        $entityManager->flush();

        return new Response('MicroPost updated successfully.');
    }
}
~~~~~~~
> Bu örnekte, `id` ile mevcut bir `MicroPost` kaydı bulunur ve güncelleyip flush method'u çağırılarak değişiklikler veritabanına kaydedilir.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/Mevcut%20Veritaban%C4%B1%20Kayd%C4%B1n%C4%B1%20G%C3%BCncelleme.md)

> Sonuç olarak, Symfony'nin en son sürümlerinde repository class'larındaki `add` ve `save` method'ları kaldırılmıştır. Bunun yerine, veri eklemek ve güncellemek için doğrudan `EntityManager` kullanması gerekmektedir.

***
### Doctrine Repostories (Fetching, Storing, Updating ½ Deleting Data)
+ Symfony framework'ünde her bir entity için bir repository class'ı bulunur. Bu repository class'ları, veritabanı tablosundaki verilerin yönetilmesine yardımcı olur ve SQL sorgularını elle yazma zorunluluğunu ortadan kaldırarak veri katmanına erişimi soyutlar.
+ Repository'ler, Doctrine ORM tarafından sağlanan hazır method'lar içerir. Bu method'lar sayesinde veri ekleme, silme, güncelleme ve sorgulama işlemleri kolaylıkla yapılabilir.

##### 1. Repository'e Erişim
+ Symfony'de bir repository'e erişmek için `dependency injection` kullanılır. Dependency injection, bir class'ın bağımlılıklarını dışarıdan almasını sağlar. Bu sayede repository class'larına kolayca erişebilir.

###### MicroPostController.php
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\MicroPost;
use App\Repository\MicroPostRepository;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/microposts', name: 'app_micropost_index')]
    public function index(MicroPostRepository $microPostRepository): Response
    {
        $microPosts = $microPostRepository->findAll();

        // Dump and Die to show the data
        dd($microPosts);

        return new Response('Check the debug output.');
    }
}
~~~~~~~
> Bu örnekte, `index` method'unda `MicroPostRepository`'i dependency injection ile alır. Bu sayede `findAll` method'u kullanarak tüm `MicroPost` kayıtlarını getirilebilir.

##### 2. Veri Ekleme
+ Symfony'nin yeni sürümlerinde repository class'larındaki `add` ve `save` method'ları kaldırılmıştır. Bunun yerine, veriyi eklemek için `EntityManager` kullanılması gerekmektedir.
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\MicroPost;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/new', name: 'app_micropost_new', methods: ['POST'])]
    public function new(Request $request, EntityManagerInterface $entityManager): Response
    {
        $microPost = new MicroPost();
        $microPost->setTitle($request->request->get('title'))
                  ->setText($request->request->get('text'))
                  ->setCreated(new \DateTime());

        $entityManager->persist($microPost);
        $entityManager->flush();

        return new Response('MicroPost created successfully.');
    }
}
~~~~~~~
> Yukarıdaki örnekte, yeni bir `MicroPost` entity'si oluşturulmakta ve `persist` ve `flush` method'ları kullanılarak veritabanına eklenmektedir.

##### 3. Veri Güncelleme
+ Mevcut bir veriyi güncellemek için `EntityManager` kullanarak veriyi bulup, gerekli değişiklikleri yaptıktan sonra `flush` method'unun çağırılması yeterlidir.
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\MicroPost;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/{id}/edit', name: 'app_micropost_edit', methods: ['POST'])]
    public function edit(int $id, Request $request, EntityManagerInterface $entityManager): Response
    {
        $microPost = $entityManager->getRepository(MicroPost::class)->find($id);

        if (!$microPost) {
            throw $this->createNotFoundException('No MicroPost found for id: '.$id);
        }

        $microPost->setTitle($request->request->get('title'))
                  ->setText($request->request->get('text'));

        $entityManager->flush();

        return new Response('MicroPost updated successfully.');
    }
}
~~~~~~~
> Yukarıdaki örnekte, `id` ile mevcut bir `MicroPost` kaydı bulunur ve güncellenip `flush` method'u ile veritabanına kaydedilir.

##### 4. Veri Silme
+ Bir kaydı silmek için de `EntityManager` kullanarak veriyi bulup `remove` ve `flush` method'larını çağırılması gerekmektedir.
~~~~~~~
<?php

namespace App\Controller;

use App\Entity\MicroPost;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/{id}/delete', name: 'app_micropost_delete', methods: ['DELETE'])]
    public function delete(int $id, EntityManagerInterface $entityManager): Response
    {
        $microPost = $entityManager->getRepository(MicroPost::class)->find($id);

        if (!$microPost) {
            throw $this->createNotFoundException('No MicroPost found for id '.$id);
        }

        $entityManager->remove($microPost);
        $entityManager->flush();

        return new Response('MicroPost deleted successfully.');
    }
}
~~~~~~~
> Yukarıdaki örnekte, `id` ile mevcut bir `MicroPost` kaydı bulunur ve `remove` ve `flush` method'ları ile veritabanından silinir.

***
### No Extra Bundle Needed! (Symfony 6.2 Changes) | Ekstra Pakete Gerek Yok! (Symfony 6.2 Değişiklikleri)
+ Symfony 6.2 ile tanıtılan bu yeni özellik, rotada bir entity class'ını type hint yaparak Symfony'nin otomatik olarak veritabanından bu nesneyi ID ile getirmesini sağlar. Dolayısıyla, ekstra bir paket kurulmasına gereken kalmaz, bu işlevsellik artık Symfony'nin yerleşik bir özelliğidir.
~~~~~~~
use App\Entity\MicroPost;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/{id}', name: 'micropost_show')]
    public function show(MicroPost $microPost): Response
    {
        // Symfony automatically fetches the MicroPost object with ID.
        return new Response('Title: ' . $microPost->getTitle());
    }
}
~~~~~~~

##### `MapEntity` Özelliği
+ Yeni `mapEntity` özelliği, bu otomatik getirme işlevselliğini nasıl yapılandırabileceği ve hatta belirli durumlarda devre dışı bırakabileceğini sağlar. Örneğin, bir kullanıcı farklı bir şekilde getirilmek istendiği durumlarda bu özellik kullanılabilir.
~~~~~~~
use App\Entity\User;
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;

class UserController extends AbstractController
{
    #[Route('/user/{id}', name: 'user_show')]
    public function show(
        #[MapEntity(disabled: true)] User $user
    ): Response
    {
        // In this case, $user is not fetched automatically.
        // You can fetch the $user object manually.
    }
}
~~~~~~~
> [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/mapEntity.md)
***
### Param Converter (Auto Fetching Entity) | Param Dönüştürücü (Otomatik Getiren Varlık)
+ Symfony'nin repository method'larını kullanılarak veri almayı ve ParamConverter ile tekil kayıtları basitleştirilebilir.
+ Tüm mikro gönderileri listelemek için index eylemini oluşturulur ve tek bir gönderiyi göstermek için bir eylem eklenir.

##### Tüm Gönderileri Listelemek İçin Index Eylemi
+ İlk olarak, tüm mikro gönderileri alıp listelecek olan index eylemini tanımlanmalıdır.
~~~~~~~
namespace App\Controller;

use App\Repository\MicroPostRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/microposts', name: 'micropost_index')]
    public function index(MicroPostRepository $microPostRepository): Response
    {
        $posts = $microPostRepository->findAll();

        // For now, let's dump to see the posts
         dd($post);

        // Then, a template will be processed with the posts
        // return $this->render('micropost/index.html.twig', ['posts' => $posts]);
    }
}
~~~~~~~
> + Burada, index method'u tüm mikro gönderileri repository'nin `findAll` method'u kullanarak alır ve ekrana dump eder (yani döker). Daha sonra, dump şablona dönüştürülür.
> + `dd($posts)` satırı çalıştırıldığında gönderileri görebilir ve ardından işlem durdurulur. Bu, hata ayıklama işlemini kolaylaştırır.

##### Tek Bir Gönderiyi Göstermek İçin Göster Eylemi
+ Bir tek mikro gönderiyi göstermek için bir eylem eklenir. Gönderiyi manuel olarak ID'ye göre almak yerine, `ParamConverter` işlevselliğini kullanılır.
~~~~~~~
use App\Entity\MicroPost;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/{id}', name: 'micropost_show')]
    public function show(MicroPost $microPost): Response
    {
        // Let's dump the micro-post to see the details
        dd($microPost);

        // Next, a template with micro-post details will be processed
        // return $this->render('micropost/show.html.twig', ['microPost' => $microPost]);
    }
}
~~~~~~~
> Burada, eylemin argümanını MicroPost varlığıyla tür belirtilir. ParamConverter sayesinde Symfony otomatik olarak ID'ye göre varlığı alır.

##### Bağımlılıkları İşlemek
+ Geçmişte SensioFrameworkExtraBundle kullanılıyordu ancak artık kullanılmamaktadır. Bunun yerine, varlıkların otomatik alınması Symfony'ye entegre edilmiştir.
~~~~~~~
composer require symfony/framework-bundle
~~~~~~~
> Projenin bu işlevselliğin zaten entegre olduğu en son Symfony sürümünü kullandığından emin olunmalıdır.

##### ParamConverter Kullanımı
+ Varlıkların (entity'lerin) alınmasını basitleştirir. Tür ipuçlarına dayanarak rota parametrelerini otomatik olarak varlıklara dönüştürür.

##### ParamConverter ile Daha Fazla Özelleştirme
+ ParamConverter'ın varlıkları nasıl alacağı daha da özelleştirebilir. `MapEntity` özniteliği kullanılır. Örneğin, bir alanı ID yerine başlık ile bir varlık almak istenildiğinde:
~~~~~~~
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;

class MicroPostController extends AbstractController
{
    #[Route('/micropost/title/{title}', name: 'micropost_show_by_title')]
    public function showByTitle(
        #[MapEntity(expr: 'repository.findOneByTitle(title)')] MicroPost $microPost
    ): Response
    {
        dd($microPost);

        // return $this->render('micropost/show.html.twig', ['microPost' => $microPost]);
    }
}
~~~~~~~
> Yukarıdaki örnekte, `MapEntity` özniteliği, varsayılan ID yerine başlığa dayanarak bir MicroPost varlığı alır.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/ParamConverter%20ile%20Daha%20Fazla%20%C3%96zelle%C5%9Ftirme%20%C3%96rne%C4%9Fi.md)

##### Otomatik Almayı Devre Dışı Bırakma
+ Belirli argümanlar için otomatik alımı devre dışı bırakmak için, MapEntity özniteliğini devre dışı bırakılmış seçeneği true olarak ayarlanır.
~~~~~~~
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;

class UserController extends AbstractController
{
    #[Route('/user/{id}', name: 'user_show')]
    public function show(
        #[MapEntity(disabled: true)] User $user
    ): Response
    {
        // Get user manually
        $user = $this->getDoctrine()->getRepository(User::class)->find($user);

        dd($user);

        // return $this->render('user/show.html.twig', ['user' => $user]);
    }
}
~~~~~~~
> Bu durumda, Symfony otomatik olarak User varlığını almayacak ve almayı manuel olarak işlenebilir.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/Otomatik%20Almay%C4%B1%20Devre%20D%C4%B1%C5%9F%C4%B1%20B%C4%B1rakma%20%C3%96rne%C4%9Fi.md)
