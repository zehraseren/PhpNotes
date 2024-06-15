### Doctrine and First Entity
#### `Symphony` Varlılığını Oluşturma
+ İlk olarak, `make:entity` komutu kullanılarak `Symphony` varlığı oluşturulur.
~~~~~~~
php bin/console make:entity Symphony
~~~~~~~

#### `Symphony` Varlığını Alan Ekleme
+ `Symfony` varlığına alanlar eklensin.
  1. Başlık (string, uzunluk 255, boş olamaz)
  2. Başlık Yılı (integer, boş olamaz)
  3. Besteci (`Composer` varlığı ile ManyToOne ilişkisi)

###### Interaktif Diyalog Örneği
~~~~~~~
php bin/console make:entity Symfony
~~~~~~~
  - Alan adı: `title`
  - Alan türü: `string`
  - Alan uzunluğu: `255`
  - Bu alan boş olabilir?: `hayır`
  - Alan adı: `year_composed`
  - Alan türü: `integer`
  - Bu alan boş olabilir?: `hayır`
  - Alan adı: `composer`
  - Alan türü: `relation`
  - İlişki türü: `ManyToOne`
  - Hedef Varlık: `Composer`
  - Inversed By (veya boş bırakın): (boş bırakın)
  - Bu alan boş olabilir?: `hayır`
> Bu işlem, belirtilen alanlarla birlikte `Symphony` varlığını oluşturarak ve `Composer` varlığı ile ilişki kurar.

#### `Symphony` Varlığını Gözden Geçirme
~~~~~~~
// src/Entity/Symphony.php

namespace App\Entity;

use App\Repository\SymphonyRepository;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity(repositoryClass: SymphonyRepository::class)]
class Symphony
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;

    #[ORM\Column(type: 'string', length: 255)]
    private $title;

    #[ORM\Column(type: 'integer')]
    private $year_composed;

    #[ORM\ManyToOne(targetEntity: Composer::class)]
    #[ORM\JoinColumn(nullable: false)]
    private $composer;

    public function getId(): ?int
    {
        return $id;
    }

    public function getTitle(): ?string
    {
        return $title;
    }

    public function setTitle(string $title): self
    {
        $this->title = $title;

        return $this;
    }

    public function getYearComposed(): ?int
    {
        return $year_composed;
    }

    public function setYearComposed(int $year_composed): self
    {
        $this->year_composed = $year_composed;

        return $this;
    }

    public function getComposer(): ?Composer
    {
        return $composer;
    }

    public function setComposer(?Composer $composer): self
    {
        $this->composer = $composer;

        return $this;
    }
}
~~~~~~~

#### Migration Oluşturma
+ Her iki varlık (`Composer` ve `Symphony`) oluşturulduğuna göre, veritabanı şemasını güncellemek için bir migrasyon oluşturulup çalıştırılır.
  1. Migration oluşturulur:
  ~~~~~~~
  php bin/console ake:migration
  ~~~~~~~

  2. Migtation dosyası gözden geçirilir (`migrations/` dizininde bulunur).
  3. Migration oluşturulur:
  ~~~~~~~
  php bin/console doctrine:migrations:migrate
  ~~~~~~~

###### Özet
+ Symfony console'u kullanılarak `first_name`, `last_name`, `date_of_birth` ve `country_code` alanlarına sahip `Composer` varlığı oluşturulur.
+ `title`, `year_composed` ve `ManyToOne` ilişkisi olan `Composer` alanlarına sahip `Symphony` varlığı oluşturuldu.
+ Veritabanı şemasını yeni varlıklar ve ilişkilerle güncellemek için migration oluşturulur ve çalıştırılır.
> Bu iki varlık ve aralarındaki ilişki ile temel bir yapı kurulmuş olunur. Bu varlıklar kullanılarak iş mantığı, CRUD işlemleri ve daha fazlası uygulanabilir.

***
### Migrations and Many to One Relation | Migration'lar ve Çoktan Bire İlişki
#### Veritabanını Başlatma ve Migration'ları Yönetme
1. Veritabanını Docker ile Başlatma
+ Öncelikle Docker Compose dosyasında tanımlanan PostgreSQL veritabanının başlatılması gerekmektedir. Bunun için aşağıdaki komut kullanılır:
~~~~~~~
docker-compose up -d
~~~~~~~
> Bu komut, PostgreSQL veritabanı container'ını arka planda çalıştırır.

2. Uygulama Container'ına Erişim
+ Uygulama container'ına erişmek için aşağıdaki komut kullanılır:
~~~~~~~
docker exec -it <application-container-name> /bin/bash
~~~~~~~
> Bu komut, uygulama container'ına bağlanmasını sağlar.

3. İlk Migration'ı Oluşturma
+ `Composer` varlığı için ilk migration'ı oluşturmak için aşağıdaki komut kullanılır:
~~~~~~~
php bin/console make:migration
~~~~~~~
> Bu komut, varlık tanımlamaları ile mevcut veritabanı şeması arasındaki farkları tespit eder ve gerekli SQL ifadelerini bir migrasyon dosyasına yazar.

4. Migration'ı Çalıştırma
+ Oluşturulan migration'ı veritabanına uygulamak için aşağıdaki komut kullanılır:
~~~~~~~
php bin/console doctrine:migrations:migrate
~~~~~~~
> Bu komut, migration dosyasındaki SQL ifadelerini çalıştırarak veritabanı şemasını günceller.

5. Durum Kontrolü ve Versiyon Yönetimi
+ Migration'ların durumunu ve mevcut veritabanı versiyonlarını kontrol etmek için aşağıdaki komutlar kullanılır:
  - Durum Kontrolü:
  ~~~~~~~
  php bin/console doctrine:migrations:status
  ~~~~~~~
  > Bu komut, mevcut veritabanı durumunu ve hangi migration'ların uygulandığını gösterir.

  - Versiyon Kontrolü:
  ~~~~~~~
  php bin/console doctrine:migrations:list
  ~~~~~~~
  > Bu komut, tüm migration'ları ve durumlarını listeler.

#### `Symphony` Varlığını Oluşturma ve Migration'u Yönetme
1. Symphony Varlığını Oluşturma
+ `Symphony` varlığını oluşturmak için aşağıdaki komut kullanılır:
~~~~~~~
php bin/console make:entity Symphony
~~~~~~~
+ Aşağıdaki alanları eklenir:
  - name: `string`, 255 karakter, boş olamaz
  - description: `text`, boş olabilir
  - composer: `ManyToOne` ilişki, `Composer` varlığı ile, boş olamaz
  - createdAt: `dateTime_immutable` boş olamaz
  - finishedAt: `dateTime_immutable`, boş olabilir

2. Migration'u Oluşturma ve Çalıştırma
+ `Symphony` varlığı için migration'u oluşturmak ve çalıştırmak için aşağıdaki komutlar kullanılır:
  - Migration oluşturulur:
  ~~~~~~~
  php bin/console make:migration
  ~~~~~~~

  - Migration Çalıştırılır:
  ~~~~~~~
  php bin/console doctrine:migrations:migrate
  ~~~~~~~

3. Durum ve Versiyon Kontrolü
+ `Symphony` varlığı için migration'ların durumunu kontrol etmek için `status` ve `list` komutları kullanılır.
  - Durum Kontrolü:
  ~~~~~~~
  php bin/console doctrine:migrations:status
  ~~~~~~~

  - Versiyon Listesi:
  ~~~~~~~
  php bin/console doctrine:migrations:list
  ~~~~~~~

###### Özet
+ Bu adımlar izlenerek, hem `Composer` hem de `Symphony` varlıklar oluşturulur, gerekli migration dosyaları oluşturulur ve bu migration'ları çalıştırarak veritabanı şeması güncellenir.
+ Doctrine, veritabanı yönetimi ve migration'ları kolaylaştırarak, SQL sorgularını otomatik olarak oluşturur ve yürütür. Bu sayede veritabanı yönetimi süreçlerini daha verimli bir şekilde gerçekleştirilir.
