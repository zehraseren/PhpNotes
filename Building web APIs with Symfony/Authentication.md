### User Entity
+ Bir symphony uygulamasının güvence altına almak ve belirli rotaların yalnızca yetkili kullanıcılar tarafından erişilebilir olmasını sağlamak için kullanıcı kimlik doğrulamasını ve yetkilendirmesini eklenir.
+ Bu, kullanıcıların geçerli kimlik bilgilerini sağlamalarını gerektirecek ve başarılı bir kimlik doğrulama sonrası onlara erişim tokeni sağlanır. Bu adımlar izlenerek `Symfony Security Bundle` kullanılarak bu süreç uygulanır.

##### 1. Symfony Security Bundle Kurulumu
+ İlk olarak, `Symfony Security Bundle` yüklenmelidir.
+ Şifreleme, yetkilendirme ve kimlik doğrulama gibi birçok güvenlik ilgili özelliği sağlar.
~~~~~~~
composer require symfony/security-bundle
~~~~~~~

##### 2. User Entity Oluşturma
+ Symfony Security Bundle ile kullanıcı doğrulaması ve yetkilendirmesi yapabilmek için bir kullanıcı entity'sine ihtiyaç vardır.
+ Symfony'nin make komutlarıyla kullanıcı entity'si oluşturulur.
~~~~~~~
php bin/console make:user
~~~~~~~
+ Bu komut çalıştırıldığında, çeşitli seçenekler sorulur:
  - Kullanıcı class adı: `User`
  - Doctrine kullanmak istiyor musunuz?: `yes`
  - Kullanıcı için identifier seçin: `username`
  - Şifre kullanacak mısınız?: `yes`

> Bu işlemler tamamlandığında, `User` class'ı ve gerekli repository oluşturulur ve `security.yaml` dosyası güncellenir.

##### 3. User Entity İncelemesi
~~~~~~~
use Symfony\Component\Security\Core\User\PasswordAuthenticatedUserInterface;
use Symfony\Component\Security\Core\User\UserInterface;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity(repositoryClass: UserRepository::class)]
class User implements UserInterface, PasswordAuthenticatedUserInterface
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;

    #[ORM\Column(type: 'string', length: 180, unique: true)]
    private $username;

    #[ORM\Column(type: 'json')]
    private $roles = [];

    #[ORM\Column(type: 'string')]
    private $password;

    // ... get/set methods ...
}
~~~~~~~
> User class'ı gerekli interface'leri uygulayıp, güvenlik paketiyle uyumlu çalışır hale getirilir.

##### 4. Security.yaml Dosyasını Güncelleme
+ `security.yaml` dosyası `make:user` komutuyla otomatik olarak güncellenir.
~~~~~~~
security:
    encoders:
        App\Entity\User:
            algorithm: auto

    providers:
        app_user_provider:
            entity:
                class: App\Entity\User
                property: username

    firewalls:
        dev:
            pattern: ^/(_(profiler|wdt)|css|images|js)/
            security: false

        main:
            anonymous: true
            provider: app_user_provider
            form_login:
                login_path: login
                check_path: login
            logout:
                path: app_logout
                target: /

    access_control:
        - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/, roles: ROLE_USER }
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/security.yaml.md)

##### 5. Database Migrations
+ User entity ve gerekli database tablolarını oluşturmak için migration'ların oluşturulup çalıştırılması gerekmektedir.
~~~~~~~
php bin/console make:migration
php bin/console doctrine:migrations:migrate
~~~~~~~

##### Sonraki Adımlar
1. `Giriş İşlemi:` Kullanıcıların giriş yapabilmesi ve token alabilmesi için bir giriş uç noktası oluşturulur.
2. `JWT veya OAuth:` Kimlik doğrulama için JWT veya OAuth gibi bir mekanizma kullanarak token oluşturma ve doğrulama işlemleri yapılır.
3. `Testler:` Güvenlik ve kimlik doğrulama işlemleri test edilmelidir.

> Bu adımlarla, kullanıcı kimlik doğrulama ve yetkilendirme işlemlerini gerçekleştirebilmek için gerekli altyapıyı kurmuş olacağız. Artık kullanıcılar veritabanında saklanabilir ve belirli rotalara erişimleri yetkilendirilebilir.

***
### User CLI Commands
+ Uygulama geliştirirken, kullanıcı yönetimi için endpoints açmak önemli güvenlik riskleri ve hususlarla gelir.
+ Daha güvenli ve daha kapsayıcı bir yaklaşım, kullanıcı oluşturma ve listeleme gibi kullanıcı yönetim görevleri için CLI komutları uygulamaktır. Bu yöntem, güvenlik açıklarını en aza indirir ve kullanıcı işlemlerini yönetmeyi basitleştirir.
#### Symfony ile Özel Komutlar Oluşturma
##### Adım 1. Komut İskeleleri Oluşturma
+ Symfony Maker Bundle kullanarak iki özel komut oluşturulur.
  - `Kullanıcı Oluşturma Komutu:` Yeni kullanıcılar oluşturmak için.
  - `Kullanıcı Listeleme Komutu:` Tüm kullanıcıları listelemek için.
###### Bu komutlar aşağıdaki gibi oluşturulmalıdır:
~~~~~~~
php bin/console make:command app:user-create
php bin/console make:command app:user-list
~~~~~~~
> Bu komutlar, `src/Command/` altında `UserCreateCommand.php` ve `UserListCommand.php` adlı iki dosya oluşturur.

##### Adım 2: Kullanıcı Oluşturma Komutunu Uygulama
+ `UserCreateCommand.php` dosyasında, yeni bir kullanıcı oluşturma mantığı tanımlanır.
  - `Komutu Yapılandırma:` Kullanıcı adı, şifre ve roller için argümanlar tanımlanır.
  - `Şifreyi Hash'leme:` Symfony'nin `PasswordHasherInterface`'i kullanılarak şifre hash'lenir.
  - `Kullanıcıyı Kalıcı Hale Getirme:` Yeni kullanıcı `UserRepository` kullanılarak database'e kaydedilir.
~~~~~~~
<?php

namespace App\Command;

use App\Entity\User;
use App\Repository\UserRepository;
use Symfony\Component\Console\Attribute\AsCommand;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\PasswordHasher\Hasher\UserPasswordHasherInterface;
use Symfony\Component\Console\Style\SymfonyStyle;

#[AsCommand(
    name: 'app:user-create',
    description: 'Şifre ve rollerle yeni bir kullanıcı oluşturun.',
)]
class UserCreateCommand extends Command
{
    private UserRepository $userRepository;
    private UserPasswordHasherInterface $passwordHasher;

    public function __construct(UserRepository $userRepository, UserPasswordHasherInterface $passwordHasher)
    {
        parent::__construct();
        $this->userRepository = $userRepository;
        $this->passwordHasher = $passwordHasher;
    }

    protected function configure(): void
    {
        $this
            ->addArgument('username', InputArgument::REQUIRED, 'Kullanıcı adı')
            ->addArgument('password', InputArgument::REQUIRED, 'Şifre (düz metin)')
            ->addArgument('roles', InputArgument::REQUIRED | InputArgument::IS_ARRAY, 'Roller (birden fazla rol için boşlukla ayırın)');
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io = new SymfonyStyle($input, $output);

        $username = $input->getArgument('username');
        $password = $input->getArgument('password');
        $roles = $input->getArgument('roles');

        if ($this->userRepository->findOneBy(['username' => $username])) {
            $io->error('Bu kullanıcı adıyla zaten bir kullanıcı var.');
            return Command::FAILURE;
        }

        $user = new User();
        $user->setUsername($username);
        $user->setRoles($roles);
        $hashedPassword = $this->passwordHasher->hashPassword($user, $password);
        $user->setPassword($hashedPassword);

        $this->userRepository->save($user, true);

        $io->success('Kullanıcı ' . $user->getId() . ' ID ile oluşturuldu.');

        return Command::SUCCESS;
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/UserCreateCommand.md)

##### Adım 3: Kullanıcı Listeleme Komutunu Uygulama
+ `UserListCommand.php` dosyasında, database'deki tüm kullanıcılar listelenir.
  - `Kullanıcıları Getirme:` Tüm kullanıcıları `UserRepository` kullanılarak getirilir.
  - `Tabloyu Render Etme:` Symfony'nin tablo yardımı ile kullanıcılar güzel bir formatta gösterilir.
~~~~~~~
<?php

namespace App\Command;

use App\Repository\UserRepository;
use Symfony\Component\Console\Attribute\AsCommand;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Helper\Table;

#[AsCommand(
    name: 'app:user-list',
    description: 'Veritabanındaki tüm mevcut kullanıcıları listeleyin.',
)]
class UserListCommand extends Command
{
    private UserRepository $userRepository;

    public function __construct(UserRepository $userRepository)
    {
        parent::__construct();
        $this->userRepository = $userRepository;
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $users = $this->userRepository->findAll();

        $table = new Table($output);
        $table
            ->setHeaders(['ID', 'Kullanıcı Adı', 'Roller'])
            ->setRows(array_map(function ($user) {
                return [
                    $user->getId(),
                    $user->getUsername(),
                    implode(', ', $user->getRoles())
                ];
            }, $users));

        $table->render();

        return Command::SUCCESS;
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/UserListCommand.md)

#### Komutları Çalıştırma
##### Kullanıcı Oluşturma
+ Yeni bir kullanıcı oluşturmak için aşağıdaki komut kullanılır.
~~~~~~~
php bin/console app:user-create <username> <password> <role1> <role2> ...
~~~~~~~

###### Örnek 
~~~~~~~
php bin/console app:user-create admin 1234 ROLE_ADMIN
~~~~~~~

##### Kullanıcı Listeleme
+ Tüm kullanıcıları listelemek için aşağıdaki komut kullanılır.
~~~~~~~
php bin/console app:user-list
~~~~~~~

> Bu CLI komutları uygulanarak, endpoints açmadan kullanıcıları yönetmek için güvenli ve verimli bir yol sağlanır. Bu yaklaşım, uygulamanın güvenliğini artırır ve Symfony'nin güçlü komut bileşenini kullanarak kullanıcı yönetim görevlerini verimli bir şekilde ele alır.

***
### Authentication Controller
+ 
~~~~~~~
~~~~~~~
>

***
### Access Tokens and Redis Storage
+ 
~~~~~~~
~~~~~~~
>

***
### Stateless Firewall
+ 
~~~~~~~
~~~~~~~
>

***
### Authrization tests
+ 
~~~~~~~
~~~~~~~
>

***
### Granular Access
+ 
~~~~~~~
~~~~~~~
>
