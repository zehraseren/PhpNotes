### Adım 1: Dockerfile Oluşturma
+ Öncelikle PHP ortamımızı ayarlamak için bir `Dockerfile` oluşturulur.
~~~~~~~
# Resmi PHP görüntüsünü kullanılır
FROM php:8.2-fpm

# Gerekli paketler ve uzantılar yüklenir
RUN apt-get update && apt-get install -y \
    libzip-dev \
    zip \
    unzip \
    libpq-dev \
    libicu-dev \
    git \
    && docker-php-ext-install pdo pdo_pgsql intl zip \
    && pecl install redis \
    && docker-php-ext-enable redis

# Composer yüklenir
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Symfony CLI yüklenir
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony*/bin/symfony /usr/local/bin/symfony

# Çalışma dizini ayarlanır
WORKDIR /app

# Mevcut uygulama dizini kopyalanır
COPY . /app

# 8000 portu açılır
EXPOSE 8000

# Symfony sunucusu başlatılır (manuel olarak halledilir)
~~~~~~~

***
### Adım 2: Docker Compose Dosyası Oluşturma
+ Servisleri tanımlamak için bir `docker-compose.yml` dosyası oluşturulur.
~~~~~~~
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    networks:
      - app-network

  redis:
    image: "redis:alpine"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
~~~~~~~

***
### Adım 3: Docker Compose'u Başlatma
+ Aşağıdaki komut ile container'lar başlatılır.
~~~~~~~
docker-compose up -d --build
~~~~~~~

***
### Adım 4: Symfony Projesini Kurma
+ Symfony projesi Docker container'ı içinde Composer kullılanarak kurulur.
  1. Çalışan Docker container'ında bir terminal açılır:
  ~~~~~~~
  docker-compose exec app bash
  ~~~~~~~

  2. Container'ın içinde, yeni bir Symfony projesi oluşturmak için aşağıdaki komut çalıştırılır:
  ~~~~~~~
  composer create-project symfony/skeleton opera
  ~~~~~~~

  3. Dosyaları kök dizine taşınır ve temizlenir:
  ~~~~~~~
  mv opera/* ./ && mv opera/.* ./ 2>/dev/null
  rmdir opera
  ~~~~~~~

***
### Adım 5: Doctrine'i Kurma
+ Doctrine'in kurulması ve proje için yapılandırılması gerekmektedir.
  1. Doctrine ORM'i kurmak için aşağıdaki komutlar çalıştırılır:
  ~~~~~~~
  composer require symfony/orm-pack
  composer require symfony/maker-bundle --dev
  ~~~~~~~

  2. PostgreSQL kullanmak için `.env` dosyası düzenlenir:
  ~~~~~~~
  DATABASE_URL="postgresql://user:password@postgres:5432/dbname?serverVersion=13&charset=utf8"
  ~~~~~~~
  > `user`, `password` ve `dbname` yerlerine projede kullanılacak veritabanı bilgileri yazılmalıdır.

***
### Adım 6: Varlıklar (Entities) Oluşturma
+ Composer ve Symphony varlıkları oluşturulsun.
  1. Composer varlığını oluşturmak için `Symfony Maker Bundle` kullanılır:
  ~~~~~~~
  php bin/console make:entity Composer
  ~~~~~~~
  > Eklenecek alanlar
  > + `firstName` (string)
  > + `lastName` (string)
  > + `dateOfBirth` (datetime)
  > + `countryCode` (string)
 
  2. Sonra Symphony varlığı oluşturulur:
  ~~~~~~~
  php bin/console make:entity Symphony
  ~~~~~~~
  > Eklenecek alanlar
  > + `name` (string)
  > + `composer` (ManyToOne ilişkisi Composer ile)
  > + `date` (datetime)

***
### Adım 7: Migration (Veritabanı Geçişleri) Oluşturma ve Çalıştırma
+ Veritabanı tablolarını oluşturmak için veritabanı geçişleri oluşturulur ve çalıştırılır:
~~~~~~~
php bin/console make:migration
php bin/console doctrine:migrations:migrate
~~~~~~~

***
### Adım 8: Kontrolörler (Controllers) Oluşturma
+ Varlıklar için CRUD işlemlerini gerçekleştirecek controllers oluşturulur.
  1. ComposerController'ı oluşturulur:
  ~~~~~~~
  php bin/console make:controller ComposerController
  ~~~~~~~
  > `src/Controller/ComposerController.php` dosyası CRUD işlemlerini içerecek şekilde güncellenmelidir.

  2. SymfonyController'ı oluşturulur:
  ~~~~~~~
  php bin/console make:controller SymphonyController
  ~~~~~~~
  > `src/Controller/SymphonyController.php` dosyası CRUD işlemlerini içerecek şekilde güncellenmelidir.

***
### Adım 9: Endpoint'leri Test Etme
+ Son olarak, endpoint'leri CURL veya Postman kullanarak manuel olarak test edilir.
  1. Symfony yerel sunucusunu başlatılır:
  ~~~~~~~
  symfony server:start
  ~~~~~~~

  2. CURL veya Postman kullanarak endpoint'lerle etkileşimde bulunulur (örneğin, besteci ve senfonileri oluşturma, güncelleme, silme).
