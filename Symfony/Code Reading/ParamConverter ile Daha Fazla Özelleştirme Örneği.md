+ Bu PHP kodu, Symfony framework'ü kullanarak yazılmış bir controller class'ını temsil eder.
+ Bu controller, URL'den gelen bir başlığa göre `MicroPost` adlı bir varlığı veri tabanından bulur ve onu görüntülemek için kullanır.

##### Kullanılan Namespace ve Bileşenler
~~~~~~~
use Symfony\Bridge\Doctrine\Attribute\MapEntity;
use Symfony\Component\Routing\Annotation\Route;
~~~~~~~
> + `Symfony\Bridge\Doctrine\Attribute\MapEntity:` Symfony'nin Doctrine ile entegrasyonunu sağlayan bir özelliktir. Bu özellik, belirtilen ifade ile varlıkları (entities) otomatik olarak eşleştirir.
> + `Symfony\Component\Routing\Annotation\Route:` Symfony'deki yönlendirme (routing) işlemlerini tanımlamak için kullanılır.

##### `MicroPostController` Class
~~~~~~~
class MicroPostController extends AbstractController
{
    #[Route('/micropost/title/{title}', name: 'micropost_show_by_title')]
    public function showByTitle(
        #[MapEntity(expr: 'repository.findOneByTitle(title)')] MicroPost $microPost
    ): Response
    {
        dd($microPost);

        // return $this->render('micropost/show.html.twig', ['microPost' => $microPost]);
    }
}
~~~~~~~

###### Route Tanımlanması
~~~~~~~
#[Route('/micropost/title/{title}', name: 'micropost_show_by_title')]
~~~~~~~
> + Bu ifade, `/micropost/title/{title}` URL'sine bir GET isteği yapıldığında bu method'un çalıştırılacağını belirtir.
> + `{title}` URL'de dinamik bir parametre olarak kullanılır ve bu parametre method'a iletilir.
> + Bu rota `micropost_show_by_title` adıyla tanımlanır.

###### `showByTitle` Method'u
~~~~~~~
public function showByTitle(
    #[MapEntity(expr: 'repository.findOneByTitle(title)')] MicroPost $microPost
): Response
~~~~~~~
> + Bu method, `title` parametresine göre bir `MicroPost` varlığını veri tabanından bulur.
> + `#[MapEntity(expr: 'repository.findOneByTitle(title)')]` ifadesi, `MicroPost` varlığını `title` parametresine göre bulmak için bir Doctrine ifadesi kullanır. Bu ifade, `repository.findOneByTitle(title)` şeklinde yazılmıştır ve `title` parametresine göre veri tabanında arama yapar.

###### İşlevsellik
~~~~~~~
{
    dd($microPost);

    // return $this->render('micropost/show.html.twig', ['microPost' => $microPost]);
}
~~~~~~~
> + `dd(#microPosts)`, `MicroPost` varlığını ekrana döker ve ardından betiği durdurur. Bu, genellikle hata ayıklama (debugging) amacıyla kullanılır.
> + Yoruma alınmış `return $this->render('micropost/show.html.twig', ['microPost' => $microPost]);` ifadesi, `microPost` varlığını bir Twig şablonuna göndererek HTML çıktısı üretmek için kullanılır. Bu ifade yoruma alındığından, şu anda şablon render edilmez.

###### Özet
+ Bu controller, belirli bir başlığa (`title`) göre `MicroPost` varlığını veri tabanından bulur ve onu görüntülemek için kullanır.
+ Symfony'nin `MapEntity` özelliği kullanılarak, `title` parametresi ile veri tabanında arama yapılır ve sonuç doğrudan `MicroPost` nesnesine eşlenir.
+ `dd(microPost)`, `MicroPost` nesnesini ekrana dökerek hata ayıklama amacıyla kullanılır.
+ Yoruma alınmış render ifadesi, `MicroPost` verisini bir Twig şablonunda görüntülemek için kullanılır.
