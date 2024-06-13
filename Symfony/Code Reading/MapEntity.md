+ Bu PHP kodu, Symfony framework'ü kullanarak yazılmış bir controller class'ını temsil eder.
+ Bu controller, URL'den gelen bir başlığa göre `MicroPost` adlı bir varlığı veri tabanından bulur ve onu görüntülemek için kullanır.

##### Kullanılan Namespace ve Bileşenler
~~~~~~~
use App\Entity\User;
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;
~~~~~~~
> + `App\Entity\User`: `User` varlık class'ı (`Entity`) için kullanılan namespace'dir.
> + `Symfony\Bridge\Doctrine\Attribute\MapEntity`: Symfony'nin Doctrine ile entegrasyonunu sağlayan bir özelliktir. Bu özellik, belirtilen ifade ile varlıkları (entities) otomatik olarak eşleştirir.
> + `Symfony\Component\Routing\Annotation\Route`: Symfony'deki yönlendirme (routing) işlemlerini tanımlamak için kullanılır.

##### `UserController` Class
~~~~~~~
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
 
###### Route Tanımlaması
~~~~~~~
#[Route('/user/{id}', name: 'user_show')]
~~~~~~~
> + Bu ifade, `/user/{id}` URL'ine bir GET isteği yapıldığında bu method'un çalıştırılacağını belirtir.
> + `{id}` URL'de dinamik bir parametre olarak kullanılır ve bu parametre yönteme iletilir.
> + Bu rota `user_show` adıyla tanımlanır.

###### `show` Method'u
~~~~~~~
public function show(
    #[MapEntity(disabled: true)] User $user
): Response
~~~~~~~
> + Bu method, `id` parametresine göre bir `User` varlığını veri tabanından bulur.
> + `#[MapEntity(disabled: true)]` ifadesi, otomatik varlık eşlemesini devre dışı bırakır. Bu, `User` varlığının manuel olarak bulunması gerektiği anlamına gelir.

###### MapEntity Özelliği
+ `#[MapEntity(disabled: true)]`: Bu özellik, Symfony'nin otomatik olarak User varlığını veri tabanından getirmesini devre dışı bırakır. Normalde, `MapEntity` özelliği otomatik olarak belirli bir ifade ile veri tabanında arama yapar ve varlığı bulur. Ancak, `disabled: true` ile bu otomatik eşleme devre dışı bırakılır.

###### Manuel Varlık Getirme
~~~~~~~
public function show(
    #[MapEntity(disabled: true)] User $user
): Response
{
    // In this case, $user is not fetched automatically.
    // You can fetch the $user object manually.
}
~~~~~~~
> + Bu method'da `$user` parametresi doğrudan `MapEntity` tarafından otomatik olarak getirilmez. Bunun yerine, varlık manuel olarak veri tabanından getirilmelidir.
> + Manuel getirme işlemi, Doctrine'nin `EntityManager` veya `Repository` class'ları kullanılarak yapılabilir.

###### Örneğin
~~~~~~~
public function show(
    #[MapEntity(disabled: true)] User $user
): Response
{
    // Manually fetch the user from the database
    $user = $this->getDoctrine()->getRepository(User::class)->find($user);

    // Dump user object to screen for debugging
    dd($user);

    // Send data to template and render (comment may be removed)
    // return $this->render('user/show.html.twig', ['user' => $user]);
}
~~~~~~~
> + `$this->getDoctrine()->getRepository(User::class)->find($user)`: Bu ifade, belirtilen ID'ye göre `User` varlığını veri tabanından getirir.
> + `dd($user)`: Bu ifade, `User` nesnesini ekrana döker ve ardından betiği durdurur, hata ayıklama amacıyla kullanılır.
> + Yoruma alınmış `return` ifadesi, `User` verisini bir Twig şablonunda görüntülemek için kullanılır.

###### Özet
+ Bu controller, belirli bir ID'ye (`id`) göre `User` varlığını veri tabanından manuel olarak bulur.
+ `MapEntity` özelliği kullanılarak otomatik varlık eşlemesi devre dışı bırakılmıştır (`disabled: true`), bu nedenle varlık manuel olarak alınmalıdır.
+ Manuel olarak varlık getirilmesi, Doctrine `Repository` veya `EntityManager` kullanılarak yapılır.
+ Bu yapı, Symfony'nin güçlü yönlendirme ve veri tabanı entegrasyon özelliklerini kullanarak dinamik veri sorgulama ve görüntüleme işlemlerini esnek bir şekilde gerçekleştirme olanağı sağlar.
