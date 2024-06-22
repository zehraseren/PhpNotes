+ Bu PHP kodu, Symfony framework'ü ile bir konsol komutu oluşturur ve database'deki tüm kullanıcıları listelemek için kullanılır. Komut, kullanıcıların ID, kullanıcı adı ve rollerini bir tablo şeklinde konsola yazdırır.

##### Namespace ve Kullanılan Kütüphaneler
~~~~~~~
namespace App\Command;

use App\Repository\UserRepository;
use Symfony\Component\Console\Attribute\AsCommand;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Helper\Table;
~~~~~~~
+ `namespace App\Command;`: Komut class'ının namespace'ini tanımlar.
+ `use` ifadeleri ile gerekli kütüphaneler ve class'lar import edilir. Bu kütüphaneler, Symfony'nin konsol komutları ve tablo oluşturma işlevselliği için kullanılır.

##### Class Tanımı
~~~~~~~
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
~~~~~~~
+ `UserListCommand` class'ı, Symfony'nin `Command` class'ından türetilmiştir.
+ `AsCommand` attribute'u ile komutun adı (`app:user-list`) ve açıklaması (`'List all existing users in the database.`) tanımlanmıştır.
+ `UserRepository` bağımlılığı constructor aracılığıyla class'a enjekte edilir.

##### `execute` Method
~~~~~~~
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
+ `execute` method'u, komut çalıştırıldığında ne yapılacağını tanımlar.
+ `findAll` method'u kullanılarak tüm kullanıcılar `UserRepository` aracılığıyla database'den çekilir.
+ Symfony'nin `Table` class'ı kullanılarak bir tablo oluşturulur.
+ `setHeaders` method'u ile tablonun başlıkları ayarlanır.
+ `setRows` method'u ile kullanıcı verileri tabloya eklenir. Kullanıcıların `ID`, `username` ve `roles` bilgileri tabloya yerleştirilir.
+ `render` method'u ile tablo konsola yazdırılır.
+ Komut başarılı bir şekilde tamamlandığında `Command::SUCCESS` kodu döndürülür.

###### Özet
+ Bu komut class'ı, CLI üzerinden çalıştırılarak database'deki tüm kullanıcıları tablo formatında listeler. Kullanıcıların ID, kullanıcı adı ve rolleri gibi bilgileri konsola yazdırılır. Bu tür komutlar, sistemdeki kullanıcıları yönetmek ve gözden geçirmek için oldukça kullanışlıdır.
