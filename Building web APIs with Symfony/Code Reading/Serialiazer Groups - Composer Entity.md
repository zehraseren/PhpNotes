+ Bu `Composer` class'ı, Symfony uygulamasında bir doctrine entity olarak tanımlanmış ve bir composer nesnesini temsil etmektedir.
+ Class'ın içerdiği özellikler ve Groups kullanımı, bu entity'nin nasıl seri hale getirileceğini (serialize) ve hangi durumlarda hangi özelliklerin dahil edileceğini belirtir.
~~~~~~~
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Serializer\Annotation\Groups;

/**
 * @ORM\Entity()
 */
class Composer
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue()
     * @ORM\Column(type="integer")
     * @Groups({"read"})
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100)
     * @Groups({"create", "update", "read"})
     */
    private $firstName;

    /**
     * @ORM\Column(type="string", length=100)
     * @Groups({"create", "update", "read"})
     */
    private $lastName;

    /**
     * @ORM\Column(type="date")
     * @Groups({"create", "update", "read"})
     */
    private $dateOfBirth;

    /**
     * @ORM\Column(type="string", length=2)
     * @Groups({"create", "update", "read"})
     */
    private $countryCode;

    // Getter/setter methods

    // ID
    public function getId(): ?int
    {
        return $this->id;
    }

    // First Name
    public function getFirstName(): ?string
    {
        return $this->firstName;
    }

    public function setFirstName(string $firstName): self
    {
        $this->firstName = $firstName;

        return $this;
    }

    // Last Name
    public function getLastName(): ?string
    {
        return $this->lastName;
    }

    public function setLastName(string $lastName): self
    {
        $this->lastName = $lastName;

        return $this;
    }

    // Date of Birth
    public function getDateOfBirth(): ?\DateTimeInterface
    {
        return $this->dateOfBirth;
    }

    public function setDateOfBirth(\DateTimeInterface $dateOfBirth): self
    {
        $this->dateOfBirth = $dateOfBirth;

        return $this;
    }

    // Country Code
    public function getCountryCode(): ?string
    {
        return $this->countryCode;
    }

    public function setCountryCode(string $countryCode): self
    {
        $this->countryCode = $countryCode;

        return $this;
    }
}
~~~~~~~

##### 1. Namespace
+ `namespace App\Entity;`: Class'ın bulunduğu namespace'i belirtir.

##### 2. ORM Annotasyonları (`@ORM\...`)
+ `@ORM\Entity()`: Doctrine tarafından yönetilen bir entity olduğunu belirtir.
+ `@ORM\Id, @ORM\GeneratedValue, @ORM\Column`: ID alanı için doctrine ORM tarafından sağlanan annotasyonlar.
  - `type="integer"`: Veritabanında saklanacak alanın türü.
+ `@ORM\Column(type="string", length=100)`: Veritabanında saklanacak string tipindeki alanın türü ve maksimum uzunluğu.
+ `@ORM\Column(type="date")`: Veritabanında saklanacak tarih tipindeki alanın türü.

##### 3. Symfony Serializer Grupları (`@Groups({"create", "update", "read"})`)
+ `create`, `update`, `read`: Symfony Serializer tarafından kullanılan gruplar.
  - `create`: Bu grup, class'ın özelliklerinin bir POST veya PUT isteği sırasında istemciye gönderilmesi gerektiğini belirtir.
  - `update`: Bu grup, class'ın özelliklerinin bir PUT isteği sırasında güncelleme işlemi için istemciye gönderilmesi gerektiğini belirtir.
  - `read`: Bu grup, class'ın özelliklerinin bir GET isteği sırasında istemciye gönderilmesi gerektiğini belirtir.

##### 4. Getter ve Setter Method'ları
+ Özelliklere erişim sağlamak ve bu özellikleri ayarlamak için kullanılır. Örneğin `getId()`, `setId()`, `getFirstName()`, `setFirstName()` gibi method'lar class'ın özelliklerine erişimi sağlar ve güncellemeleri yapar.

> Bu yapı, Symfony uygulamasında composers database'inde saklandığı ve ilgili CRUD (Create, Read, Update, Delete) operasyonlarını yönetmek için kullanıldığı varsayılan bir entity class'ını temsil eder. Bu sayede Symfony ORM (Doctrine) tarafından yönetilen ve Symfony Serializer tarafından seri hale getirilen bir veri yapısına sahip olunur.
