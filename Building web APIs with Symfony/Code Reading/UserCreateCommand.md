+ Bu PHP kodu, Symfony framework'ü ile bir console komutu oluşturarak yeni bir kullanıcı yaratmayı sağlar. Komut, kullanıcı adı, şifre ve roller gibi bilgileri alarak yeni bir kullanıcı oluşturur ve database'e kaydeder.

##### Namespace ve Kullanılan Kütüphaneler
~~~~~~~
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
~~~~~~~
+ `namespace App\Command;`: Komut class'ının namespace'ini tanımlar.
+ `use` ifadeleri ile gerekli kütüphaneler ve class'lar import edilir. Bu kütüphaneler Symfony'nin konsol komutları, kullanıcı veritabanı işlemleri ve şifreleme işlemleri için kullanılır.

##### Class Tanımı
~~~~~~~
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
~~~~~~~
+ `UserCreateCommand` class, Symfony'nin Command class'ından türetilmiştir.
+ `AsCommand` attribute'u ile komutun adı (`app:user-create`) ve açıklaması (`Create a new user with password and roles.`) tanımlanmıştır.
+ `UserRepository` ve `UserPasswordHasherInterface` bağımlılıkları constructor aracılığıyla class'a enjekte edilir.

##### `configure` Method
~~~~~~~
    protected function configure(): void
    {
        $this
            ->addArgument('username', InputArgument::REQUIRED, 'Username')
            ->addArgument('password', InputArgument::REQUIRED, 'Password (plain text)')
            ->addArgument('roles', InputArgument::REQUIRED | InputArgument::IS_ARRAY, 'Roles (separate with space for multiple roles)');
    }
~~~~~~~
+ Bu method, komut için gerekli olan argümanları tanımlar:
  - `username:` Kullanıcının adı (zorunlu).
  - `password:` Kullanıcının şifresi (zorunlu, düz metin olarak girilir).
  - `roles:` Kullanıcının rolleri (zorunlu, birden fazla rol boşluk ile ayrılabilir).

##### `execute` Method
~~~~~~~
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
+ `execute` method'u komut çalıştırıldığında ne yapılacağını tanımlar.
+ `SymfonyStyle` ile konsol çıktıları daha okunabilir hale getirilir.
+ Argümanlar `InputInterface` ile alınır: `username`, `password`, ve `roles`.
+ Kullanıcı adı database'de zaten varsa bir hata mesajı gösterilir ve komut `FAILURE` kodu ile sonlandırılır.
+ Yeni bir `User` nesnesi oluşturulur, şifre hashlenir ve kullanıcıya atanır.
+ Kullanıcı database'e kaydedilir.
+ Başarılı işlem mesajı gösterilir ve komut `SUCCESS` kodu ile sonlandırılır.

> Bu komut class'ı, kullanıcı oluşturma işlemini kolaylaştırır ve CLI üzerinden çalıştırılabilir. Kullanıcı bilgilerini alır, doğrular, şifreyi hashler ve yeni kullanıcıyı database'e kaydeder. Bu tür komutlar, özellikle büyük uygulamalarda yönetici işlemlerini otomatikleştirmek için çok faydalıdır.
