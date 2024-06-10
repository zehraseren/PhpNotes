+ Bu PHP kodu, PHP-DI (Dependency Injection) kütüphanesini kullanarak bağımlılık enjeksiyonu (DI) uygulayan bir örnektir. PHP-DI, bağımlılık enjeksiyonunu kolaylaştıran popüler bir kütüphanedir.
+ Bu kodda, `Logger` ve `UserService` class'ları ve bir DI konteyneri kullanılarak bağımlılıkların nasıl otomatik olarak çözülüp enjekte edildiği gösterilmektedir. 

##### Gerekli Dosyaların Yüklenmesi
~~~~~~~
require 'vendor/autoload.php';
~~~~~~~
> `vendor/autoload.php`, Composer tarafından oluşturulan otomatik yükleyici dosyadır. Bu, tüm bağımlılıkların otomatik olarak yüklenmesini sağlar.

##### Namespace Kullanımı
~~~~~~~
use DI\Container;
~~~~~~~
> `DI\Container`, PHP-DI kütüphanesinin container class'ını kullanmak için gerekli `use` ifadesidir.

##### `Logger` Class
~~~~~~~
class Logger {
    public function log($message) {
        echo "Logging message: $message";
    }
}
~~~~~~~
> + `Logger` class'ı, mesajları loglamak için kullanılan basit bir class'tır.
> + `log` method'u, aldığı mesajı ekrana yazdırır.

##### `UserService` Class
~~~~~~~
class UserService {
    private $logger;

    public function __construct(Logger $logger) {
        $this->logger = $logger;
    }

    public function createUser($username) {
        // User creation
        $this->logger->log("User created: $username");
    }
}
~~~~~~~
> + `UserService` class'ı, kullanıcıları yönetmek için kullanılır.
> + `__construct` method'u, bir `Logger` nesnesini parametre olarak alır ve bu nesne class'ın `logger` özelliğine atanır. Bu, `UserService` class'ının `Logger` class'ına bağımlı olduğunu gösterir.
> + `createUser` method'u, kullanıcı oluşturma işlemlerini gerçekleştirir ve `Logger` nesnesini kullanarak bu işlemi loglar.

##### DI Konteynerinin Oluşturulması ve Kullanımı
~~~~~~~
$container = new Container();
$userService = $container->get(UserService::class);
~~~~~~~
> + `DI Konteynerinin Oluşturulması:` `new Container()` ifadesiyle bir DI container'ı oluşturulur. Bu container, bağımlılıkların yönetilmesi ve çözülmesi için kullanılır.
> + `Bağımlılığın Çözülmesi ve Enjekte Edilmesi:` `$container->get(UserService::class)` ifadesi, container'dan `UserService` class'ının bir örneğini alır. PHP-DI container'ı, `UserService` class'ının yapıcısında (constructor) `Logger` class'ına bağımlı olduğunu bilir ve otomatik olarak bir `Logger` nesnesi oluşturup enjekte eder.

##### Kullanıcı Oluşturma
~~~~~~~
$userService->createUser("zehraseren");
~~~~~~~
> + `createUser` method'u çağırılarak, `zehraseren` adlı kullanıcı oluşturulur ve bu işlem `Logger` nesnesi tarafından loglanır.

###### Özet
+ Bu kod, PHP-DI kütüphanesini kullanarak bağımlılık enjeksiyonu uygular.
+ `Logger` class'ı, mesajları loglamak için kullanılır.
+ `UserService` class'ı, kullanıcı oluşturma işlemlerini gerçekleştirir ve bu işlemi loglamak için bir `Logger` nesnesine bağımlıdır.
+ DI container'ı (`Container`), bağımlılıkların otomatik olarak çözülüp enjekte edilmesini sağlar. `UserService` class'ı oluşturulurken, PHP-DI container'ı otomatik olarak `Logger` nesnesini oluşturur ve enjekte eder.
+ Bu yaklaşım, kodun daha esnek, modüler ve test edilebilir olmasını sağlar.
