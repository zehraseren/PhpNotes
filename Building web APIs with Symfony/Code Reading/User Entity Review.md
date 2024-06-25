+ Bu class, Symfony ve Doctrine ORM kullanılarak bir `User` entity tanımlanır. Bu User entity, Symfony'nin güvenlik sistemi ile uyumlu olacak şekilde `UserInterface` ve `PasswordAuthenticatedUserInterface` interface'leri uygular.  
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

~~~~~~~

#### Anotasyonlar ve Özellikler
##### 1. ORM Anotasyonları
+ `#[ORM\Entity(repositoryClass: UserRepository::class)]`: Bu class'ın bir Doctrine varlığı olduğunu belirtir ve bu entity için özel bir depo (repository) class'ı olan `UserRepository` kullanılır.
+ `#[ORM\Id]`: Bu özelliğin (property) varlığın primary key'i olduğunu belirtir.
+ `#[ORM\GeneratedValue]`: Bu özelliğin otomatik olarak üretileceğini belirtir (örneğin, bir database tarafından).
+ `#[ORM\Column(type: 'integer')]`: Bu özelliğin veritabanında bir integer sütunu olacağını belirtir.
+ `#[ORM\Column(type: 'string', length: 180, unique: true)]`: Bu özelliğin veritabanında bir string sütunu olacağını belirtir, maksimum uzunluk 180 karakterdir ve bu sütun benzersiz (unique) olmalıdır.
+ `#[ORM\Column(type: 'json')]`: Bu özelliğin veritabanında bir json sütunu olacağını belirtir.
+ `#[ORM\Column(type: 'string')]`: Bu özelliğin veritabanında bir string sütunu olacağını belirtir.

##### 2. Özellikler
+ `$id`: Kullanıcının benzersiz kimliği.
+ `$username`: Kullanıcının benzersiz kullanıcı adı.
+ `$roles`: Kullanıcının rollerini içeren bir JSON dizisi.
+ `$password`: Kullanıcının şifrelenmiş parolası.

#### Interface'ler
+ `UserInterface`: Symfony'nin güvenlik sistemi ile uyumlu kullanıcı class'larının uygulaması gereken yöntemleri tanımlar. Bu yöntemler şunlardır:
  - `getUsername()`: Kullanıcının kullanıcı adını döndürür.
  - `getRoles()`: Kullanıcının rollerini döndürür.
  - `getPassword()`: Kullanıcının şifrelenmiş parolasını döndürür.
  - `eraseCredentials()`: Kullanıcı oturumu sonlandırıldığında hassas verilerin silinmesi için kullanılır (genellikle boş bırakılır).
+ `PasswordAuthenticatedUserInterface`: Kullanıcı class'ının bir parola ile doğrulanmasını sağlar. Bu interface genellikle `UserInterface` ile birlikte kullanılır.

###### get/set Methods
~~~~~~~
public function getId(): ?int
{
    return $this->id;
}

public function getUsername(): string
{
    return $this->username;
}

public function setUsername(string $username): self
{
    $this->username = $username;

    return $this;
}

public function getRoles(): array
{
    $roles = $this->roles;
    // guarantee every user at least has ROLE_USER
    $roles[] = 'ROLE_USER';

    return array_unique($roles);
}

public function setRoles(array $roles): self
{
    $this->roles = $roles;

    return $this;
}

public function getPassword(): string
{
    return $this->password;
}

public function setPassword(string $password): self
{
    $this->password = $password;

    return $this;
}

public function eraseCredentials()
{
    // If you store any temporary, sensitive data on the user, clear it here
    // $this->plainPassword = null;
}
~~~~~~~

+ `getRoles()` method her kullanıcının en az `ROLE_USER` rolüne sahip olmasını garanti eder. Bu method ayrıca, bir kullanıcının rollerinin benzersiz olmasını sağlamak için `array_unique` fonksiyonunu kullanır.
+ `eraseCredentials()` method, oturum kapatıldığında geçici veya hassas verileri temizlemek için kullanılır. Örneğin, düz metin parolaları gibi.

> Bu class, Symfony'nin kullanıcı doğrulama ve yetkilendirme sistemleriyle sorunsuz bir şekilde çalışmak üzere tasarlanmıştır ve Doctrine ORM ile database'ine kolayca entegre edilebilir.
