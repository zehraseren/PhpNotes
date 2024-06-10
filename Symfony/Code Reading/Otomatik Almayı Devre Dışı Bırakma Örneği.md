+ Bu PHP kodu, Symfony framework'ü kullanarak yazılmış bir `UserController` class'ını temsil eder.
+ Bu controller, URL'den gelen bir kullanıcı ID'sine göre `User` varlığını veri tabanından bulur ve onu görüntülemek için kullanır.
+ Kodda `MapEntity` özelliği ve manuel veri tabanı sorgulaması yapılmaktadır. 

##### Kulanılan Namespace ve Bileşenler
~~~~~~~
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;
~~~~~~~
> + `Symfony\Bridge\Doctrine\Attribute\MapEntity:` Symfony'nin Doctrine ile entegrasyonunu sağlayan bir özelliktir. Bu özellik, belirtilen ifade ile varlıkları (entities) otomatik olarak eşleştirir.
> + `Symfony\Component\Routing\Annotation\Route:` Symfony'deki yönlendirme (routing) işlemlerini tanımlamak için kullanılır.

##### `UserController` Class
~~~~~~~
class UserController extends AbstractController
{
    #[Route('/user/{id}', name: 'user_show')]
    public function show(
        #[MapEntity(disabled: true)] User $user
    ): Response
    {
        // Get user manually
        $user = $this->getDoctrine()->getRepository(User::class)->find($user);

        dd($user);

        // return $this->render('user/show.html.twig', ['user' => $user]);
    }
}
~~~~~~~

###### Route Tanımlaması
~~~~~~~
#[Route('/user/{id}', name: 'user_show')]
~~~~~~~
> + Bu ifade, `/user/{id}` URL'ine bir GET isteği yapıldığında bu yöntemin çalıştırılacağını belirtir.
> + `{id}` URL'de dinamik bir parametre olarak kullanılır ve bu parametre method'a iletilir.
> + Bu rota `user_show` adıyla tanımlanır.

###### `show` Method
~~~~~~~
public function show(
    #[MapEntity(disabled: true)] User $user
): Response
~~~~~~~
> + Bu method, `id` parametresine göre bir `User` varlığını veri tabanından bulur.
> + `#[MapEntity(disabled: true)]` ifadesi, otomatik varlık eşlemesini devre dışı bırakır. Bu, `User` varlığının manuel olarak bulunması gerektiği anlamına gelir.

###### İşlevsellik
~~~~~~~
{
    // Get user manually
    $user = $this->getDoctrine()->getRepository(User::class)->find($user);

    dd($user);

    // return $this->render('user/show.html.twig', ['user' => $user]);
}
~~~~~~~
> + `$this->getDoctrine()->getRepository(User::class)->find($user):` Bu ifade, `User` varlığını belirtilen ID'ye göre veri tabanından bulur.
> + `dd($user):` Bu ifade, `User` nesnesini ekrana döker ve ardından betiği durdurur. `dd` (dump and die) ifadesi genellikle hata ayıklama (debugging) amacıyla kullanılır.
> + Yoruma alınmış `return $this->render('user/show.html.twig', ['user' => $user]);` ifadesi, `User` varlığını bir Twig şablonuna göndererek HTML çıktısı üretmek için kullanılır. Bu ifade yoruma alındığından, şu anda şablon render edilmez.

###### Özet
+ Bu controller, belirli bir ID'ye (`id`) göre `User` varlığını veri tabanından bulur ve onu görüntülemek için kullanır.
+ `MapEntity` özelliği kullanılarak otomatik varlık eşlemesi devre dışı bırakılmıştır (`disabled: true`), bu nedenle varlık manuel olarak alınır.
+ `$this->getDoctrine()->getRepository(User::class)->find($user)` ifadesi, `User` varlığını belirtilen ID'ye göre veri tabanından bulur.
+ `dd($user)` ifadesi, `User` nesnesini ekrana döker ve betiği durdurur, hata ayıklama amacıyla kullanılır.
+ Yoruma alınmış render ifadesi, `User` verisini bir Twig şablonunda görüntülemek için kullanılır.
