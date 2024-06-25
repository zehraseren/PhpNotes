+ Bu `Symphony` class'ı, Symfony uygulamasında bir doctrine entity olarak tanımlanmış ve bir composer nesnesini temsil etmektedir.
+ Class'ın içerdiği özellikler ve Groups kullanımı, bu entity'nin nasıl seri hale getirileceğini (serialize) ve hangi durumlarda hangi özelliklerin dahil edileceğini belirtir.

~~~~~~~
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Serializer\Annotation\Groups;

/**
 * @ORM\Entity()
 */
class Symphony
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
    private $name;

    /**
     * @ORM\Column(type="text")
     * @Groups({"create", "update", "read"})
     */
    private $description;

    /**
     * @ORM\Column(type="datetime")
     * @Groups({"update", "read"})
     */
    private $finishedAt;

    // Getter ve Setter metodları burada olacak

    // ID
    public function getId(): ?int
    {
        return $this->id;
    }

    // Name
    public function getName(): ?string
    {
        return $this->name;
    }

    public function setName(string $name): self
    {
        $this->name = $name;

        return $this;
    }

    // Description
    public function getDescription(): ?string
    {
        return $this->description;
    }

    public function setDescription(string $description): self
    {
        $this->description = $description;

        return $this;
    }

    // FinishedAt
    public function getFinishedAt(): ?\DateTimeInterface
    {
        return $this->finishedAt;
    }

    public function setFinishedAt(\DateTimeInterface $finishedAt): self
    {
        $this->finishedAt = $finishedAt;

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
+ `@ORM\Column(type="text")`: Veritabanında saklanacak metin tipindeki alanın türü.
+ `@ORM\Column(type="datetime")`: Veritabanında saklanacak tarih ve zaman tipindeki alanın türü.

##### 3. Symfony Serializer Grupları (`@Groups({"create", "update", "read"})`)
+ `create`, `update`, `read`: Symfony Serializer tarafından kullanılan gruplar.
  - `create`: Bu grup, class'ın özelliklerinin bir POST veya PUT isteği sırasında istemciye gönderilmesi gerektiğini belirtir.
  - `update`: Bu grup, class'ın özelliklerinin bir PUT isteği sırasında güncelleme işlemi için istemciye gönderilmesi gerektiğini belirtir.
  - `read`: Bu grup, class'ın özelliklerinin bir GET isteği sırasında istemciye gönderilmesi gerektiğini belirtir.

##### 4. Getter ve Setter Method'ları
+ Özelliklere erişim sağlamak ve bu özellikleri ayarlamak için kullanılır. Örneğin `getId()`, `setId()`, `getName()`, `setName()` gibi method'lar class'ın özelliklerine erişimi sağlar ve güncellemeleri yapar.

> Bu yapı, Symfony uygulamasında symphonies database'inde saklandığı ve ilgili CRUD (Create, Read, Update, Delete) operasyonlarını yönetmek için kullanıldığı varsayılan bir entity class'ını temsil eder. Bu sayede Symfony ORM (Doctrine) tarafından yönetilen ve Symfony Serializer tarafından seri hale getirilen bir veri yapısına sahip olunur.
