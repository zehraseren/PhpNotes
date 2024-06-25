### User Entity
+ Bir symphony uygulamasının güvence altına almak ve belirli rotaların yalnızca yetkili kullanıcılar tarafından erişilebilir olmasını sağlamak için kullanıcı kimlik doğrulamasını ve yetkilendirmesini eklenir.
+ Bu, kullanıcıların geçerli kimlik bilgilerini sağlamalarını gerektirecek ve başarılı bir kimlik doğrulama sonrası onlara erişim token'ı sağlanır. Aşağıdaki adımlar izlenerek `Symfony Security Bundle` kullanılarak bu süreç uygulanır.

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
> + [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/tree/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading)

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
1. `Giriş İşlemi:` Kullanıcıların giriş yapabilmesi ve token alabilmesi için bir giriş endpoint'i oluşturulur.
2. `JWT veya OAuth:` Kimlik doğrulama için JWT veya OAuth gibi bir mekanizma kullanılarak token oluşturma ve doğrulama işlemleri yapılır.
3. `Testler:` Güvenlik ve kimlik doğrulama işlemleri test edilmelidir.

> Bu adımlarla, kullanıcı kimlik doğrulama ve yetkilendirme işlemlerini gerçekleştirebilmek için gerekli altyapı kurulur. Böylece kullanıcılar database'de saklanabilir ve belirli rotalara erişimleri yetkilendirilebilir.

***
### User CLI Commands
+ Uygulama geliştirirken, kullanıcı yönetimi için endpoints açmak önemli güvenlik riskleri ve hususlarla gelir.
+ Daha güvenli ve daha kapsayıcı bir yaklaşım, kullanıcı oluşturma ve listeleme gibi kullanıcı yönetim görevleri için CLI komutları uygulamaktır. Bu yöntem, güvenlik açıklarını en aza indirir ve kullanıcı işlemlerini yönetmeyi basitleştirir.
#### Symfony ile Özel Komutlar Oluşturma
##### Adım 1. Komut İskeleleri Oluşturma
+ Symfony Maker Bundle kullanarak iki özel komut oluşturulur.
  - Kullanıcı Oluşturma Komutu
  - Kullanıcı Listeleme Komutu
    
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
    description: 'Create a new user with password and roles.',
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
            ->addArgument('username', InputArgument::REQUIRED, 'Username')
            ->addArgument('password', InputArgument::REQUIRED, 'Password (plain text)')
            ->addArgument('roles', InputArgument::REQUIRED | InputArgument::IS_ARRAY, 'Roles (separate with space for multiple roles)');
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io = new SymfonyStyle($input, $output);

        $username = $input->getArgument('username');
        $password = $input->getArgument('password');
        $roles = $input->getArgument('roles');

        if ($this->userRepository->findOneBy(['username' => $username])) {
            $io->error('There is already a user with this username.');
            return Command::FAILURE;
        }

        $user = new User();
        $user->setUsername($username);
        $user->setRoles($roles);
        $hashedPassword = $this->passwordHasher->hashPassword($user, $password);
        $user->setPassword($hashedPassword);

        $this->userRepository->save($user, true);

        $io->success('User created with ' . $user->getId() . ' ID');

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
    description: 'List all existing users in the database.',
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
            ->setHeaders(['ID', 'Username', 'Roles'])
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
+ Kullanıcıları doğrulamak ve uygulama endpoint'lerine erişim sağlamak için bir `JWT (JSON Web Token)` tabanlı kimlik doğrulama mekanizması uygulansın.
+ JWT kullanarak, kullanıcılar yalnızca bir kez oturum açacak ve daha sonra belirli bir süre geçerli olacak bir erişim jetonu alacaklar. Bu yöntem, kullanıcı adı ve şifreyi her istekte göndermek zorunda kalmadan güvenli bir şekilde kimlik doğrulama yapmayı sağlar.

##### Adım 1: Gereksinimleri Yükleme
+ JWT tabanlı kimlik doğrulama için `lexik/jwt-authentication-bundle` paketini kullanılır. Bu paket Composer ile yüklenir:
~~~~~~~
composer require lexik/jwt-authentication-bundle
~~~~~~~

##### Adım 2: Paket Yapılandırması
+ JWT yapılandırmasını yapmak için aşağıdaki komut kullanılarak gerekli dosyaları oluşturulur.
~~~~~~~
php bin/console lexik:jwt:generate-keypair
~~~~~~~
> Bu komut, `config/packages/lexik_jwt_authentication.yaml` dosyasını ve RSA anahtar çiftlerini (private ve public key) oluşturur.

##### Adım 3: JWT Sağlayıcı Yapılandırması
+ `config/packages/security.yaml` dosyasında JWT sağlayıcısı yapılandırılmalıdır:
###### config/packages/security.yaml
~~~~~~~
security:
    encoders:
        App\Entity\User:
            algorithm: auto

    providers:
        users_in_database:
            entity:
                class: App\Entity\User
                property: username

    firewalls:
        login:
            pattern:  ^/api/login
            stateless: true
            anonymous: true
            json_login:
                check_path:               /api/login_check
                username_path:            username
                password_path:            password
                success_handler:          lexik_jwt_authentication.handler.authentication_success
                failure_handler:          lexik_jwt_authentication.handler.authentication_failure

        api:
            pattern:   ^/api
            stateless: true
            guard:
                authenticators:
                    - lexik_jwt_authentication.jwt_token_authenticator

    access_control:
        - { path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api, roles: IS_AUTHENTICATED_FULLY }
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/lexik_jwt_authentication.yaml.md)

##### Adım 4: AuthController Oluşturma
+ Bir `AuthController` oluşturulur ve login endpoint'i tanımlanır.
~~~~~~~
php bin/console make:controller AuthController
~~~~~~~

+ `src/Controller/AuthController.php` dosyası açılır ve aşağıdaki gibi düzenlenir:
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Core\Security;
use Symfony\Component\Security\Http\Authentication\AuthenticationUtils;

class AuthController extends AbstractController
{
    #[Route('/api/login', name: 'api_login', methods: ['POST'])]
    public function login()
    {
        $user = $this->getUser();

        if (!$user) {
            return new JsonResponse(['message' => 'Invalid credentials'], JsonResponse::HTTP_UNAUTHORIZED);
        }

        return new JsonResponse(['token' => 'dummy-token-for-now']); // This part is updated with real tokens
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Create%20AuthControllerClass.md)

##### Adım 5: JWT Token Üretimi
+ Yukarıdaki controller'daki `dummy-token-for-now` kısmını gerçek bir token ile değiştirmek için, `LexikJWTAuthenticationBundle`'ın sağladığı `JWTTokenManagerInterface` ve `EventDispatcherInterface` class'ları kullanılır.
+ `src/Controller/AuthController.php` dosyası aşağıdaki gibi güncellenmelidir:
~~~~~~~
<?php

namespace App\Controller;

use Lexik\Bundle\JWTAuthenticationBundle\Services\JWTTokenManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Core\Security;
use Symfony\Component\Security\Http\Authentication\AuthenticationUtils;
use Symfony\Contracts\EventDispatcher\EventDispatcherInterface;

class AuthController extends AbstractController
{
    private $jwtManager;
    private $dispatcher;

    public function __construct(JWTTokenManagerInterface $jwtManager, EventDispatcherInterface $dispatcher)
    {
        $this->jwtManager = $jwtManager;
        $this->dispatcher = $dispatcher;
    }

    #[Route('/api/login', name: 'api_login', methods: ['POST'])]
    public function login()
    {
        $user = $this->getUser();

        if (!$user) {
            return new JsonResponse(['message' => 'Invalid credentials'], JsonResponse::HTTP_UNAUTHORIZED);
        }

        $token = $this->jwtManager->create($user);

        return new JsonResponse(['token' => $token]);
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Update%20AuthControllerClass.md)

##### Adım 6: Korunan Endpoint'leri Test Etme
+ JWT tabanlı kimlik doğrulamayı test etmek için öncelikle kullanıcı adı ve şifre ile oturum açılır ve ardından alınan token ile korunan bir endpoint'e erişim sağlanır.
  1. Oturum Açma
  ~~~~~~~
  curl -X POST http://localhost:8000/api/login -H "Content-Type: application/json" -d '{"username":"admin","password":"1234"}'
  ~~~~~~~

  2. Korumalı Uç Noktaya Erişim:
  ~~~~~~~
  curl -X GET http://localhost:8000/api/some-protected-endpoint -H "Authorization: Bearer YOUR_JWT_TOKEN"
  ~~~~~~~

> JWT tabanlı kimlik doğrulama ile kullanıcıların kimlik bilgilerini yalnızca bir kez kullanarak oturum açmalarını ve daha sonra bir token ile uygulamanın korunan endpoint'lerine erişim sağlandı. Bu yöntem, güvenliği artırır ve kullanıcı deneyimini iyileştirir.

***
### Access Tokens and Redis Storage
+ Symfony uygulamasında Redis kullanılarak token tabanlı kimlik doğrulama sistemi uygulanır.

#### Yapılacak Adımlar
##### 1. Docker Compose ile Redis Kurulumu
+ Docker Compose yapılandırmasına bir Redis container'ı eklenir.
+ Redis PHP uzantısının kurulu ve etkin olduğundan emin olunmalıdır.

##### 2. Redis Bağlantısının Yapılandırılması
+ Symfony’nin `services.yaml` dosyasında bir Redis servisi oluşturulur.
+ Bağlantı çevresel değişkenler ve Symfony’nin bağımlılık enjeksiyon container'ı kullanılarak yapılandırılır.

##### 3. Erişim Tokenlarının Oluşturulması ve Saklanması
+ Token oluşturma ve saklama işlevlerini yönetmek için yeni bir `AccessTokenHandler` class'ı oluşturulur.
+ PHP’nin `session_create_id()` fonksiyonunu kullanarak token oluşturulur.
+ Oluşturulan token belirli bir süre için Redis'e kaydedilir.

##### 4. Giriş Noktasını Oluşturma
+ `AuthController` içerisinde bir login noktası tanımlanır.
+ Kullanıcıları database üzerinden kimlik doğrulaması yapılır ve başarılı girişlerde bir erişim token'ı oluşturulur.
+ Oluşturulan token istemciye döndürülür.

##### 5. Token ile Gelen İstekleri Yönetme
+ `security.yaml` dosyasında bir token handler (işleyici) yapılandırılır.
+ Gelen isteklerdeki token'ları doğrulamak için `AccessTokenHandler` class'ındaki `getUserBadgeFrom` method'unu uygulanır.
+ Redis'ten token ile ilişkilendirilen kullanıcıyı bulmak için sorgu yapılır.

##### 6. Test Etme
+ Giriş yapma ve korunan endpoint'lere erişim işlemlerini `curl` komutları ile doğrulanır.
+ Geçerli giriş, geçersiz giriş, token doğrulama ve geçerli/geçersiz token ile endpoint erişimini test edilir.

#### Uygulama
##### 1. Docker Compose Yapılandırması
~~~~~~~
services:
  redis:
    image: "redis:latest"
    ports:
      - "6379:6379"
~~~~~~~

##### 2. Redis Servis Yapılandırması
+ `services.yaml` dosyasında:
~~~~~~~
parameters:
  redis_host: '%env(REDIS_URL)%'
  redis_port: '%env(int:REDIS_PORT)%'

services:
  Redis:
    class: Redis
    calls:
      - method: connect
        arguments:
          - '%redis_host%'
          - '%redis_port%'
~~~~~~~

##### 3. AccessTokenHandler Class
+ `src/Security/AccessTokenHandler.php` dosyasında:
~~~~~~~
namespace App\Security;

use Symfony\Component\Security\Http\Authenticator\Passport\Badge\UserBadge;
use Symfony\Component\Security\Core\Exception\BadCredentialsException;

class AccessTokenHandler
{
    private $redis;

    public function __construct(\Redis $redis)
    {
        $this->redis = $redis;
    }

    public function createForUser($user)
    {
        $accessToken = session_create_id();
        $this->redis->setex('sessions/' . $accessToken, 10800, $user->getUserIdentifier());
        return $accessToken;
    }

    public function getUserBadgeFrom($accessToken)
    {
        $userId = $this->redis->get('sessions/' . $accessToken);
        if (!$userId) {
            throw new BadCredentialsException('Invalid token');
        }
        return new UserBadge($userId);
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/AccessTokenHandler%20Class.md)

##### 4. AuthController
+ `src/Controller/AuthController.php` dosyasında:
~~~~~~~
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;
use App\Security\AccessTokenHandler;

class AuthController extends AbstractController
{
    private $accessTokenHandler;

    public function __construct(AccessTokenHandler $accessTokenHandler)
    {
        $this->accessTokenHandler = $accessTokenHandler;
    }

    #[Route('/app/auth/login', name: 'auth_login', methods: ['POST'])]
    public function login(Request $request)
    {
        $user = $this->getUser();
        if (!$user) {
            return new JsonResponse(['message' => 'Invalid credentials'], JsonResponse::HTTP_UNAUTHORIZED);
        }
        $token = $this->accessTokenHandler->createForUser($user);
        return new JsonResponse(['token' => $token]);
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/AuthController.md)

##### 5. Güvenlik Yapılandırması
+ `config/packages/security.yaml` dosyasında:
~~~~~~~
security:
  firewalls:
    main:
      custom_authenticators:
        - App\Security\AccessTokenHandler
~~~~~~~

#### Test Komutları
##### 1. Giriş Yap ve Token Al
~~~~~~~
curl -X POST -H "Content-Type: application/json" -d '{"username":"admin", "password":"1234"}' http://localhost:8000/app/auth/login
~~~~~~~

##### 2. Korunan Endpoint'e Erişim
~~~~~~~
curl -H "Authorization: Bearer <token>" http://localhost:8000/composer
~~~~~~~

##### 3. Geçersiz Token
~~~~~~~
curl -H "Authorization: Bearer invalid_token" http://localhost:8000/composer
~~~~~~~

> Bu adımlar takip edilerek, Symfony ve Redis kullanarak güvenli bir token tabanlı kimlik doğrulama sistemi kurulur. Bu, uygulamanın kullanıcı kimlik doğrulamasını verimli bir şekilde yönetmesini sağlar ve token oluşturma ve doğrulama işlemlerinin güvenli bir şekilde yönetilmesini sağlar.

***
### Stateless Firewall
+ API'yı stateless hale getirmek ve Symfony'nin her sorguda oturum başlatmasını engellemek için `security.yaml` dosyasında gerekli değişikliklerin yapılması gerekmektedir. Bu değişiklikler, `oturumların yalnızca Redis'te saklanan erişim token'ları ile yönetilmesini sağlar ve API'yı daha esnek hale getirir.`

##### 1. security.yaml Dosyasını Güncelleme
+ Öncelikle, main firewall'ının stateless olarak ayarlandığından emin olunmalıdır:
~~~~~~~
security:
  firewalls:
    main:
      stateless: true
      custom_authenticators:
        - App\Security\AccessTokenHandler
~~~~~~~
> Bu ayar, Symfony'nin oturumları başlatmasını engeller ve böylece API'ın stateless olmasını sağlar.

##### Testleri Güncelleme
+ Yeni kimlik doğrulama mekanizması ile mevcut testlerin çoğunun başarısız olması muhtemeldir. Testlerin yeni yapıya uyacak şekilde güncellenmesi gerekmektedir.
##### 2.1. Testler İçin Yapılandırma
+ Testlerde kullanılacak olan erişim token'larını oluşturmak için test kullanıcıları ve token'ları ayarlanmalıdır. Örneğin, `AccessTokenHandler`'ı mock'lamak yerine gerçek token'ları kullanılabilir.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SomeControllerTest extends WebTestCase
{
    private $accessToken;

    public function setUp(): void
    {
        parent::setUp();

        //Steps to create test users and access tokens
        $client = static::createClient();
        $userRepository = $client->getContainer()->get(UserRepository::class);
        $user = $userRepository->findOneByEmail('test@example.com');
        
        // Mock the AccessTokenHandler or use the real one to generate a token
        $accessTokenHandler = $client->getContainer()->get(AccessTokenHandler::class);
        $this->accessToken = $accessTokenHandler->createForUser($user);
    }

    public function testSomeEndpoint()
    {
        $client = static::createClient();

        // Send request with authorization header
        $client->request('GET', '/some-endpoint', [], [], [
            'HTTP_AUTHORIZATION' => 'Bearer ' . $this->accessToken,
        ]);

        $this->assertEquals(200, $client->getResponse()->getStatusCode());
        // Other test validations...
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Configuration%20for%20Tests.md)

##### 2.2. Yeni Test Senaryoları Eklemek
+ Yeni token tabanlı kimlik doğrulama mekanizmasını test etmek için birkaç farklı senaryo eklenmelidir. Örneğin, geçersiz bir token ile istek yapıldığında doğru hata mesajının döndüğünden emin olunulmalıdır.
~~~~~~~
public function testInvalidToken()
{
    $client = static::createClient();

    // Sending request with invalid token
    $client->request('GET', '/some-endpoint', [], [], [
        'HTTP_AUTHORIZATION' => 'Bearer invalid_token',
    ]);

    $this->assertEquals(401, $client->getResponse()->getStatusCode());
    // Other test validations...
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Add%20New%20Test%20Scenarios.md)

##### 2.3. Mevcut Testleri Güncelleme
+ Tüm testleri yeni kimlik doğrulama yapısına uygun hale getirmek için, her testin başında yetkili bir kullanıcı ve geçerli bir token oluşturulmalıdır.
~~~~~~~
public function testSomeOtherEndpoint()
{
    $client = static::createClient();

    // Send request with authorization header
    $client->request('POST', '/some-other-endpoint', [], [], [
        'CONTENT_TYPE' => 'application/json',
        'HTTP_AUTHORIZATION' => 'Bearer ' . $this->accessToken,
    ], json_encode(['data' => 'test']));

    $this->assertEquals(201, $client->getResponse()->getStatusCode());
    // Other test validations...
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Update%20Existing%20Tests.md)

> Yukarıdaki adımlar izlenerek, Symfony uygulaması stateless hale getirilip, erişim token'ları ile kimlik doğrulamasını yöneten bir yapı kuruldu. Ayrıca, mevcut testler yeni yapıya uyacak şekilde güncellendi. Bu sayede API daha esnek ve genişletilebilir bir yapıya sahip olacaktır ve farklı türdeki istemcilerle daha uyumlu çalışacaktır.

***
### Authorization Tests  
#### 1. Kullanıcı Oluşturma Komutunu Çağırma
+ Testler çalıştırılmadan önce bir test kullanıcısı oluşturulur ve bu kullanıcıya ait bir erişim token'ı alınır. Bunu yapmak için Symfony uygulaması başlatılır ve `user-create` komutu çalıştırılır.
~~~~~~~
use Symfony\Bundle\FrameworkBundle\Console\Application;
use Symfony\Component\Console\Input\ArrayInput;
use Symfony\Component\Console\Output\BufferedOutput;
use Symfony\Component\Console\Tester\CommandTester;
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class SomeControllerTest extends WebTestCase
{
    private static $testUserToken;

    public function setUp(): void
    {
        parent::setUp();

        $kernel = self::bootKernel();
        $application = new Application($kernel);
        $application->setAutoExit(false);

        $command = $application->find('app:user-create');

        $input = new ArrayInput([
            'username' => 'testuser',
            'password' => 'Test1234',
            'roles' => ['ROLE_USER']
        ]);

        $output = new BufferedOutput();
        $command->run($input, $output);

        // Login to get the access token
        $client = static::createClient();
        $client->request('POST', '/login', [], [], [
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'username' => 'testuser',
            'password' => 'Test1234'
        ]));

        $response = $client->getResponse();
        $data = json_decode($response->getContent(), true);
        self::$testUserToken = $data['token'];
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Invoke%20User%20Creation%20Command.md)

#### 2. Testlerde Token Kullanımı
+ Oluşturulan erişim token'ı testlerde kullanılarak tüm isteklerde bu token gönderilir. Bu sayede tüm endpoint'lere erişim sağlanabilir.

##### 2.1. Yardımcı Method'larda Token Kullanımı
~~~~~~~
protected function post(string $uri, array $data = [], ?string $token = null)
{
    $headers = ['CONTENT_TYPE' => 'application/json'];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('POST', $uri, [], [], $headers, json_encode($data));
}

protected function get(string $uri, ?string $token = null)
{
    $headers = [];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('GET', $uri, [], [], $headers);
}

protected function put(string $uri, array $data = [], ?string $token = null)
{
    $headers = ['CONTENT_TYPE' => 'application/json'];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('PUT', $uri, [], [], $headers, json_encode($data));
}

protected function delete(string $uri, ?string $token = null)
{
    $headers = [];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('DELETE', $uri, [], [], $headers);
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Token%20Usage%20in%20Helper%20Methods.md)

##### 2.2. Testlerde Token Kullanarak İstek Gönderme
~~~~~~~
public function testCreateComposer()
{
    $response = $this->post('/composer', [
        'name' => 'Test Composer'
    ], self::$testUserToken);

    $this->assertEquals(201, $response->getStatusCode());
    // Other test validations...
}

public function testIndexComposer()
{
    $response = $this->get('/composer', self::$testUserToken);

    $this->assertEquals(200, $response->getStatusCode());
    // Other test validations...
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Sending%20Requests%20Using%20Token%20in%20Tests.md)

> Bu adımlarla birlikte, tüm testlerde kimlik doğrulama gereksinimleri karşılanır ve testlerin başarılı bir şekilde geçilmesini sağlar. API stateless ve güvenli bir şekilde çalışır halde ve testler de bu yeni yapıya uygun olarak güncellenmiştir.

***
### Granular Access
+ Testler granular erişim kontrolüyle güncellendiği ve doğru rollerle çalıştıklarına emin olunduktan sonra, aşağıdaki adımlar özetlenerek ve yeni bir test eklenerek `403` durum kodu doğrulaması yapılabilir.

#### 1. Erişim Kontrolü
###### ComposerController
~~~~~~~
// ... (other use statements)

use Symfony\Component\Security\Http\Attribute\IsGranted;

class ComposerController extends AbstractController
{
    // ...

    #[IsGranted('ROLE_USER')]
    public function index(): Response
    {
        // ...
    }

    #[IsGranted('ROLE_USER')]
    public function show(int $id): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function create(Request $request): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function update(int $id, Request $request): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function delete(int $id): Response
    {
        // ...
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Access%20Control%20-%20ComposerController.md)

###### SymphonyController
~~~~~~~
// ... (other use statements)

use Symfony\Component\Security\Http\Attribute\IsGranted;

class SymphonyController extends AbstractController
{
    // ...

    #[IsGranted('ROLE_USER')]
    public function index(): Response
    {
        // ...
    }

    #[IsGranted('ROLE_USER')]
    public function show(int $id): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function create(Request $request): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function update(int $id, Request $request): Response
    {
        // ...
    }

    #[IsGranted('ROLE_ADMIN')]
    public function delete(int $id): Response
    {
        // ...
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Access%20Control%20-%20SymphonyController.md)

#### 2. Yeni Test Kullanıcıları ve Token'ları
+ Test kullanıcıları ve admin kullanıcıları tanımlanır ve ilgili token'lar alınır.
~~~~~~~
class SomeControllerTest extends WebTestCase
{
    private static $testUserToken;
    private static $testAdminToken;

    public function setUp(): void
    {
        parent::setUp();

        $kernel = self::bootKernel();
        $application = new Application($kernel);
        $application->setAutoExit(false);

        $command = $application->find('app:user-create');

        $inputUser = new ArrayInput([
            'username' => 'testuser',
            'password' => 'Test1234',
            'roles' => ['ROLE_USER']
        ]);

        $inputAdmin = new ArrayInput([
            'username' => 'testadmin',
            'password' => 'Test1234',
            'roles' => ['ROLE_ADMIN']
        ]);

        $output = new BufferedOutput();
        $command->run($inputUser, $output);
        $command->run($inputAdmin, $output);

        $client = static::createClient();
        
        $client->request('POST', '/login', [], [], [
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'username' => 'testuser',
            'password' => 'Test1234'
        ]));

        $response = $client->getResponse();
        $data = json_decode($response->getContent(), true);
        self::$testUserToken = $data['token'];

        $client->request('POST', '/login', [], [], [
            'CONTENT_TYPE' => 'application/json'
        ], json_encode([
            'username' => 'testadmin',
            'password' => 'Test1234'
        ]));

        $response = $client->getResponse();
        $data = json_decode($response->getContent(), true);
        self::$testAdminToken = $data['token'];
    }
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/New%20Test%20Users%20and%20Tokens.md)

#### 3. Güncellenmiş Testler
+ Yeni test kullanıcıları ve admin token'larıyla birlikte testler güncellenir.
~~~~~~~
public function testCreateComposer()
{
    $response = $this->post('/composer', [
        'name' => 'Test Composer'
    ], self::$testAdminToken);

    $this->assertEquals(201, $response->getStatusCode());
}

public function testCreateComposerWithUserRole()
{
    $response = $this->post('/composer', [
        'name' => 'Test Composer'
    ], self::$testUserToken);

    $this->assertEquals(403, $response->getStatusCode());
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/Updated%20Tests.md)

#### 4. Yeni Test: 403 Durum Kodunu Doğrulama
+ 403 durum kodunu doğrulayan yeni bir test eklenir.
~~~~~~~
public function testCreateComposerWithUserRole()
{
    $response = $this->post('/composer', [
        'name' => 'Test Composer'
    ], self::$testUserToken);

    $this->assertEquals(403, $response->getStatusCode());
}
~~~~~~~
> [Yukarıdaki kodun adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Building%20web%20APIs%20with%20Symfony/Code%20Reading/New%20Test%3A%20Validate%20403%20Status%20Code.md)

> Bu adımlar sayesinde, rol bazlı erişim kontrolüm granular hale getirilir ve testler bu yeni yapıya uygun hale getirilir. Tüm testler doğru rollerle çalıştıklarından emin olunur ve yeni bir test eklenerek 403 durum kodunu doğrulanır. Bu yaklaşım, gelecekte eklenecek yeni roller ve yetkilendirmeler için de sağlam bir temel oluşturur.
