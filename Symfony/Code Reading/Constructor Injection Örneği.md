+ Bu PHP kodu, basit bir bağımlılık enjeksiyonu örneği sunar.
+ Bağımlılık enjeksiyonu, bir class'ın (örneğin `UserService` sınıfının) ihtiyaç duyduğu diğer class'ların (örneğin Logger class'ının) dışarıdan sağlanması yöntemidir. Bu, kodun daha esnek ve test edilebilir olmasını sağlar.

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
> + `UserService` class'ı, kullanıcıları yönetmek için kullanılan bir hizmet class'tır.
> + `__construct` method'u, bir `Logger` nesnesini alır ve class'ın `logger` özelliğine atar. bu, `UserService` class'ının `Logger` class'ına bağımlı olduğunu gösterir.
> + `createUser` method'u, kullanıcı oluşturma işlemlerini gerçekleştirir ve `Logger` nesnesini kullanarak kullanıcı oluşturma işlemini loglar.

##### Bağımlılıkları Manuel Olarak Enjekte Etme
~~~~~~~
$logger = new Logger();
$userService = new UserService($logger);

$userService->createUser("zehraseren");
~~~~~~~
> + `Logger` class'ından bir nesne oluşturulur.
> + `UserService` class'ından bir nesne oluşturulurken, `Logger` nesnesi ona bağımlılık olarak enjekte edilir.
> + `createUser` method'u çağırılır ve `zehraseren` adı kullanıcı oluşturulur.
