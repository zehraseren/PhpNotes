+ Bu PHP kodu, bağımlılık enjeksiyonu (dependency injection) yaklaşımını kullanarak bir `UserService` class'ına `Logger` class'ının nasıl enjekte edildiğini göstermektedir.
+ Bu yaklaşım, önceki örnekteki yapıcı (constructor) enjeksiyonuna alternatif olarak setter enjeksiyonu kullanmaktadır. 

##### `Logger` Class
~~~~~~~
class Logger {
    public function log($message) {
        echo "Logging message: $message";
    }
}
~~~~~~~
> + `Logger` class'ı, mesajları loglamak için kullanılır.
> + `log` method'u aldığı mesajı ekrana yazdırır.

##### `UserService` Class
~~~~~~~
class UserService {
    private $logger;

    public function setLogger(Logger $logger) {
        $this->logger = $logger;
    }

    public function createUser($username) {
        // User creation
        $this->logger->log("User created: $username");
    }
}
~~~~~~~
> + `UserService` class'ı, kullanıcıları yönetmek için kullanılır.
> + `setLogger` method'u, `Logger` nesnesini `UserService` class'ına enjekte etmek için kullanılır. Bu method, `UserService` class'ının bir `Logger` nesnesine bağımlı olduğunu gösterir.
> + `createUser` method'u, kullanıcı oluşturma işlemlerini gerçekleştirir ve `Logger` nesnesini kullanarak bu işlemi loglar.

##### Bağımlılıkları Manuel Olarak Enjekte Etme
~~~~~~~
$logger = new Logger();
$userService = new UserService();
$userService->setLogger($logger);

$userService->createUser("zehraseren");
~~~~~~~
> + `Logger` class'ından bir nesne oluşturulur.
> + `UserService` class'ından bir nesne oluşturulur.
> + `setLogger` method'u çağrılarak, `Logger` nesnesi `UserService` class'ına enjekte edilir.
> + `createUser` method'u çağrılır ve `zehraseren` adlı kullanıcı oluşturulur.

###### Özet
+ `Logger` class'ı, mesajları loglamak için kullanılan basit bir class'tır.
+ `UserService` class'ı, kullanıcıları yönetmek için kullanılır ve loglama yapmak için bir `Logger` nesnesine ihtiyaç duyar.
+ Bu örnekte, bağımlılık enjeksiyonu setter method'u kullanılarak yapılmıştır. `setLogger` yöntemi, `Logger` nesnesini `UserService` class'ına enjekte eder.
+ Bağımlılıkları manuel olarak enjekte ederek, `UserService` class'ı, `Logger` nesnesini kullanarak kullanıcı oluşturma işlemini loglar.
