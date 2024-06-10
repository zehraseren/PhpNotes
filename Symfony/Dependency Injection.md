### Dependency Injection (Bağımlılık Enjeksiyonu) nedir?
+ Dependency Injection (DI), nesnelerin bağımlılıklarının dışarıdan sağlanması prensibine dayanan bir tasarım deseni ve yazılım geliştirme tekniğidir.
+ Bu, class'ların bağımlılıklarını kendileri oluşturmaları yerine, bağımlılıklarının başka bir yerden sağlanmasını sağlar. Bu yaklaşım, yazılımın daha modüler, test edilebilir ve esnek olmasını sağlar.

#### Temel Kavramlar ve Faydalar
1. `Gevşek Bağlılık (Loose Coupling):` Class'lar arasındaki bağımlılıklar minimize edilerek, class'ların birbirine sıkı sıkıya bağlı olmaması sağlanır.
2. `Test Edilebilirlik (Testability):` Class'lar dışarıdan bağımlılık alarak daha kolay test edilebilir hale gelir. `Mock veya Stub nesneleri enjekte edilerek testler yapılabilir.`
3. `Yeniden Kullanılabilirlik (Reusability):` Bağımlılıklar dışarıdan sağlandığında, aynı class farklı bağlamlarda yeniden kullanılabilir.

#### Dependency Injection Türleri
1. `Constructor Injection:` Bağımlılıklar, class'ın yapıcı method'u (constructor) aracılığıyla sağlanır.
2. `Setter Injection:` Bağımlılıklar, setter method'ları aracılığıyla sağlanır.
3. `Interface Injection:` Bağımlılıklar, bir interface aracılığıyla sağlanır. Bu yöntem PHP'de daha az yaygındır.

##### Constructor Injection Örneği
+ Constructor Injection en yaygın kullanılan DI yöntemidir. Bir class'ının bağımlılıkları, yapıcı method aracılığıyla sağlanır.
###### Basit Constructor Injection
~~~~~~~
<?php

class Logger {
    public function log($message) {
        echo "Logging message: $message";
    }
}

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

// Manually injecting dependencies
$logger = new Logger();
$userService = new UserService($logger);

$userService->createUser("zehraseren");
?>
~~~~~~~
> Yukarıdaki örnekte, `UserService` class'ı `Logger` bağımlılığını yapıcı method'u aracılığıyla alır. Bu, `UserService` class'ının `Logger` nesnesini doğrudan oluşturmak yerine, dışarıdan sağlanmasını sağlar.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/Constructor%20Injection%20%C3%96rne%C4%9Fi.md)

##### Setter Injection Örneği
+ Setter Injection, bağımlılıkların setter metotları aracılığıyla sağlanmasıdır.
###### Basit Setter Injection
~~~~~~~
<?php

class Logger {
    public function log($message) {
        echo "Logging message: $message";
    }
}

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

// Manually injecting dependencies
$logger = new Logger();
$userService = new UserService();
$userService->setLogger($logger);

$userService->createUser("zehraseren");
?>
~~~~~~~
> Yukarıdaki örnekte, `UserService` class'ı `Logger` bağımlılığını `setLogger` method'u aracılığıyla alır. Bu yöntem, bağımlılıkların dinamik olarak ayarlanabilmesini sağlar.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/Setter%20Injection%20%C3%96rne%C4%9Fi.md)

#### Constructor Injection vs. Setter Injection
+ Her iki yöntem de bağımlılıkları dışarıdan sağlamayı amaçlar, ancak bunu yapma şekilleri ve kullanım durumları farklıdır.

1. `Constructor Injection`, bağımlılıkların class'ın yapıcı method'u (constructor) aracılığıyla sağlanmasıdır. Bu yöntem, `bağımlılıkların nesne oluşturulurken sağlanmasını garanti eder ve nesne, bağımlılıkları olmadan oluşturulamaz.`

    ###### Özellikleri
+ `Bağımlılıkların Sağlanması:` Bağımlılıklar, nesnenin oluşturulma anında sağlanır ve bu bağımlılıkların değiştirilememesi sağlanır.
+ `Zorunlu Bağımlılıklar:` Bu method, bağımlılıkların zorunlu olduğunu ve olmadan nesnenin işlevsel olamayacağını belirtmek için uygundur.
+ `Daha Az Kod:` Tek seferde tüm bağımlılıklar sağlandığı için, genellikle daha az kod gerektirir.
 
 2. `Setter Injection`, bağımlılıkların class'ın setter method'ları aracılığıyla sağlanmasıdır. Bu method, `bağımlılıkların nesne oluşturulduktan sonra sağlanmasına olanak tanır.`

    ###### Özellikleri
+ `Opsiyonel Bağımlılıklar:` Bu method, bağımlılıkların opsiyonel olabileceği veya sonradan değiştirilebileceği durumlar için uygundur.
+ `Daha Esnek:` Bağımlılıkların nesne ömrü boyunca değiştirilmesine izin verir.
+ `Daha Fazla Kod:` Genellikle setter method'ları eklenmesi gerektiğinden daha fazla kod gerektirir.

> + Constructor Injection: Bağımlılıklar, nesne oluşturulurken sağlanır. Bu method, bağımlılıkların zorunlu ve değiştirilemez olduğu durumlar için uygundur.
> + Setter Injection: Bağımlılıklar, setter method'ları aracılığıyla sağlanır. Bu method, bağımlılıkların opsiyonel veya değiştirilebilir olduğu durumlar için uygundur.

#### Dependency Injection Container (DIC)
+ Bağımlılık enjeksiyonunu yönetmek ve daha karmaşık bağımlılık yapıları ile başa çıkmak için Dependency Injection Container (DIC) kullanılır. DIC, bağımlılıkların oluşturulmasını ve yönetilmesini otomatikleştiren bir araçtır.
###### Basit DIC Kullanımı
~~~~~~~
<?php

require 'vendor/autoload.php';

use DI\Container;

class Logger {
    public function log($message) {
        echo "Logging message: $message";
    }
}

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

//  Creating and configuring DIC
$container = new Container();
$userService = $container->get(UserService::class);

$userService->createUser("zehraseren");
?>
~~~~~~~
> Bu örnekte, `PHP-DI` kullanılarak bağımlılıklar otomatik olarak çözülür ve `UserService` nesnesi oluşturulur.
> + [Yukarıdaki örneğin adım adım açıklaması](https://github.com/zehraseren/PhpNotes/blob/main/Symfony/Code%20Reading/Dependency%20Injection%20Container%20%C3%96rne%C4%9Fi.md)
