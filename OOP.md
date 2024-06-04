### OOP nedir?
+ Yazılım geliştirme paradigmasıdır ve veriyi ve işlevselliği bir arada tutan nesneler ve class'lar kullanılarak programlama yapılmasını sağlar.
+ OOP, kodun daha modüler, esnek ve bakımının kolay olmasını sağlamak amacıyla PHP diline dahil edilmiştir.

### OOP'nin Temel Kavramları
***
#### Classes | Sınıflar
+ Nesneler için bir şablon veya taslaktır. Class'lar, özellikler ve yöntemler içerir.
~~~~~~~
<?php

class Car {
    public $color;
    public $model;

    public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
    }

    public function message() {
        return "My car is a " . $this->color . " " . $this->model . ".";
    }
}
?>
~~~~~~~

***
#### Objects | Nesneler
+ Class'lardan türetilen varlıklardır. Class'ların somut örnekleridir.
~~~~~~~
<?php

$myCar = new Car("vantablack", "Volvo XC90");
echo $myCar->message();
?>
~~~~~~~
> Output: My car is a vantablack Volvo XC90.

***
#### Properties | Özellikler
+ Bir class'ın değişkenleridir ve nesnelerin durumunu saklar.
~~~~~~~
<?php

class Car {
    public $color;
    public $model;
}
?>
~~~~~~~

***
#### Methods | Yöntemler
+ Bir class'ın işlevleridir ve nesnelerin davranışlarını tanımlar.
~~~~~~~
<?php

class Car {
    public function message() {
        return "Hello, I am a car!";
    }
}
?>
~~~~~~~

***
#### Inheritance | Kalıtım
+ Bir class'ın (sub class veya türetilmiş class) başka bir class'tan özellik ve yöntemleri miras almasını sağlar.
+ Kodun yeniden kullanılabilirliğini artırır ve yazılımın bakımını ve genişletilmesini kolaylaştırır.

#### Kalıtımın Temel Özellikleri
- `Alt Sınıf (Subclass):` Başka bir class'tan miras alan class.
- `Üst Sınıf (Superclass):` Alt class'a özelliklerini ve yöntemlerini devreden class.
- `extends Keyword:` Bir class'ın başka bir class'tan kalıtım almasını sağlamak için kullanılır.

#### Kalıtımın Avantajları
+ `Kodun Yeniden Kullanımı:` Üst class'ın özellikleri ve yöntemleri, alt class'larda tekrar kullanılabilir.
+ `Bakım Kolaylığı:` Kodun bir yerde değiştirilmesi, bu kodu miras alan tüm class'ları etkiler.
+ `Genişletilebilirlik:` Varolan class'ların davranışlarını genişletmek veya değiştirmek kolaydır.

~~~~~~~
<?php

class ElectricCar extends Car {
    public $batteryLife;

    public function __construct($color, $model, $batteryLife) {
        parent::__construct($color, $model);
        $this->batteryLife = $batteryLife;
    }

    public function batteryStatus() {
        return "Battery life is " . $this->batteryLife . " hours.";
    }
}
?>
~~~~~~~

###### Üst Class Tanımlama
~~~~~~~
<?php

class Animal {
    public $name;
    public $color;

    public function __construct($name, $color) {
        $this->name = $name;
        $this->color = $color;
    }

    public function makeSound() {
        echo "Some generic animal sound";
    }

    public function describe() {
        return "This is a $this->color $this->name.";
    }
}
?>
~~~~~~~

###### Alt Class Tanımlama
+ Alt class, `extends` keyword kullanılarak üst class'tan miras alır.
~~~~~~~
<?php

class Dog extends Animal {
    public function makeSound() {
        echo "Bark";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "Meow";
    }
}

$dog = new Dog("Dog", "Brown");
echo $dog->describe();
$dog->makeSound();

$cat = new Cat("Cat", "Black");
echo $cat->describe();
$cat->makeSound();
?>
~~~~~~~
> Output: This is a Brown Dog.

> Output: Bark


> Output: This is a Black Cat.

> Output: Meow

##### Overriding (Geçersiz Kılma)
+ Alt class'lar, üst class'lardan miras alınan yöntemleri kendi ihtiyaçlarına göre değiştirebilirler. Buna `geçersiz kılma (overriding)` denir.
~~~~~~~
<?php

class Animal {
    public function makeSound() {
        echo "Some generic animal sound";
    }
}

class Dog extends Animal {
    // Overriding the makeSound method
    public function makeSound() {
        echo "Bark";
    }
}

$dog = new Dog();
$dog->makeSound();
?>
~~~~~~~
> Output: Bark

##### Parent Keyword
+ Alt class, üst class'ın yöntemlerine erişmek için parent keyword kullanabilir.
~~~~~~~
<?php

class Animal {
    public function makeSound() {
        echo "Some generic animal sound";
    }
}

class Dog extends Animal {
    public function makeSound() {
        // Call the makeSound method of a superclass
        parent::makeSound();
        echo " and Bark";
    }
}

$dog = new Dog();
$dog->makeSound();
?>
~~~~~~~
> Output: Some generic animal sound and Bark

##### Kalıtım ve Yapıcılar (Constructors)
+ Alt class'lar, üst class'ın yapıcı yöntemini çağırmak için `parent::__construct` yöntemini kullanabilirler.
~~~~~~~
<?php

class Animal {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }
}

class Dog extends Animal {
    public $breed;

    public function __construct($name, $breed) {
        // Call a superclass constructor method
        parent::__construct($name);
        $this->breed = $breed;
    }

    public function describe() {
        return "This is a $this->breed named $this->name.";
    }
}

$dog = new Dog("Rex", "German Shepherd");
echo $dog->describe();
?>
~~~~~~~
> Output: This is a German Shepherd named Rex.

##### Kalıtım ve Erişim Belirleyicileri
+ Kalıtım, erişim belirleyicileri ile birlikte kullanıldığında veri gizliliğini ve erişim kontrolünü sağlar. `public`, `protected` ve `private` belirleyicileri kalıtımda önemli rol oynar.
~~~~~~~
<?php

class Animal {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    protected function getName() {
        return $this->name;
    }
}

class Dog extends Animal {
    public function describe() {
        // The getName method is protected and can be subclassed
        return "This is a dog named " . $this->getName();
    }
}

$dog = new Dog("Rex");
echo $dog->describe();
?>
~~~~~~~
> Output: This is a dog named Rex

***
#### Encapsulation | Kapsülleme
+ Bir class'ın verilerini (özelliklerini) ve bu veriler üzerinde işlem yapan yöntemlerini (metotlarını) bir arada tutar ve bu verilere dışarıdan doğrudan erişimi kontrol eder.
+ Veri gizliliğini ve veri bütünlüğünü korur, class'ın dışındaki kodun class'ın iç işleyişine müdahale etmesini engeller.
+ Özellikler ve yöntemler `private`, `protected` ve `public` erişim belirleyicileri ile kontrol edilir.
~~~~~~~
<?php

class Car {
    private $color;
    private $model;

    public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
    }

    public function getColor() {
        return $this->color;
    }

    public function setColor($color) {
        $this->color = $color;
    }
}
?>
~~~~~~~

#### Kapsüllemenin Avantajları
+ `Veri Gizliliği:` Verilere dışarıdan doğrudan erişimi kısıtlayarak veri gizliliğini sağlar.
+ `Veri Bütünlüğü:` Verilerin class içindeki belirli yöntemler aracılığıyla değiştirilmesini sağlayarak veri bütünlüğünü korur.
+ `Esneklik ve Bakım Kolaylığı:` Class'ın iç yapısını değiştirmeden dış arayüzü koruyarak esneklik ve bakım kolaylığı sağlar.

#### Kapsülleme ve Get/Set Yöntemleri
+ Veri gizliliğini sağlamak için private veya protected özelliklere erişimi `get/set` yöntemleri aracılığıyla kontrol edilir. Bu yöntemler, özelliklerin değerlerini almak ve ayarlamak için kullanılır, böylece veri doğrulama ve iş mantığı eklemek mümkündür.
~~~~~~~
<?php

class User {
    private $name;
    private $age;

    public function getName() {
        return $this->name;
    }

    public function setName($name) {
        if (is_string($name) && !empty($name)) {
            $this->name = $name;
        } else {
            throw new Exception("Invalid name.");
        }
    }

    public function getAge() {
        return $this->age;
    }

    public function setAge($age) {
        if (is_int($age) && $age > 0) {
            $this->age = $age;
        } else {
            throw new Exception("Invalid age.");
        }
    }
}

$user = new User();
$user->setName("Zehra Şeren");
$user->setAge(29);
echo $user->getName();
echo $user->getAge();
?>
~~~~~~~
> Output: Zehra Şeren

> Output: 29

***
#### Polymorphism | Polimorfizm | Çok Biçimlilik
+ Aynı isimli method'ların farklı şekillerde kullanılabilmesidir yani bir nesnenin farklı şekillerde davranabilme yeteneğini ifade eder.
+ Aynı yöntemi farklı şekillerde uygulayan veya aynı adı taşıyan farklı yöntemleri olan class'lar arasında ilişki kurulmasını sağlar.
~~~~~~~
<?php

class Car {
    public function startEngine() {
        echo "Starting car engine";
    }
}

class ElectricCar extends Car {
    public function startEngine() {
        echo "Starting electric car engine";
    }
}

$myCar = new Car();
$myCar->startEngine();

$myElectricCar = new ElectricCar();
$myElectricCar->startEngine();
?>
~~~~~~~
> Output: Starting car engine

> Output: Starting electric car engine

##### Türleri
1. `Statik Polimorfizm (Compile-Time Polymorphism):` Derleme zamanında belirlenen polimorfizm türüdür. `Overloading` ve `operator overloading` bu tür polimorfizmin örnekleridir. `PHP'de, method overloading bulunmamaktadır` ancak operator overloading bazı durumlarda kullanılabilir.
+ `Method Overloading:` Aynı adı taşıyan ancak farklı parametre listelerine sahip yöntemlerin bulunması.
+ `Operator Overloading:` Operatörlerin farklı bağlamlarda farklı davranışlar sergilemesi.

2. `Dinamik Polimorfizm (Run-Time Polymorphism):` Çalışma zamanında belirlenen polimorfizm türüdür. Kalıtım ve interface'ler üzerinde çalışır. Çalışma zamanında nesne türüne bağlı olarak farklı yöntemlerin çağrılmasını sağlar.
+ `Method Overriding:` Üst class'ta tanımlı bir yöntemin alt class'ta aynı adla ve aynı parametre listesiyle yeniden tanımlanması.
+ `Late Binding (Geç Bağlama):` Hangi yöntemin çağrılacağının çalışma zamanında belirlenmesi.

###### Dinamik Polimorfizm (Method Overriding)
~~~~~~~
<?php

class Animal {
    public function makeSound() {
        echo "Some generic animal sound";
    }
}

class Dog extends Animal {
    public function makeSound() {
        echo "Bark";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "Meow";
    }
}

// Calling the makeSound method of different subclasses with a superclass reference
$animals = array(new Dog(), new Cat());
foreach ($animals as $animal) {
    $animal->makeSound();
    echo "<br>";
}
?>
~~~~~~~
> + `Animal` class'ının `makeSound` method'u, hem `Dog` hem de `Cat` class'larında yeniden tanımlanmıştır. Bu durum, aynı method'un farklı alt class'lar tarafından farklı şekillerde uygulanabileceğini gösterir.

###### Dinamik Polimorfizm (Method Overriding ve Late Binding)
~~~~~~~
<?php

class Animal {
    public function makeSound() {
        echo "Some generic animal sound";
    }
}

class Dog extends Animal {
    public function makeSound() {
        echo "Bark";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "Meow";
    }
}

// Calling the makeSound method of different subclasses with late binding
function describeAnimal(Animal $animal) {
    echo "This animal says: ";
    $animal->makeSound();
    echo "<br>";
}

$dog = new Dog();
$cat = new Cat();

describeAnimal($dog);
describeAnimal($cat);
?>
~~~~~~~
> Output: This animal says: Bark

> Output: This animal says: Meow

> + `describeAnimal` fonksiyonu, bir `Animal` nesnesi aldığı için bu fonksiyona farklı alt class nesnelerini geçmek mümkündür. Fonksiyonun içinde çağrılan `makeSound` method'u, çalışma zamanında nesnenin türüne göre belirlenir.

***
#### Abstraction | Soyutlama
+ Nesnelerin karmaşıklığını gizleme ve yalnızca gerekli bilgileri gösterme işlemidir. PHP'de abstract class ve interface kullanılarak yapılır.
~~~~~~~
<?php

abstract class Car {
    abstract public function startEngine();
}

class ElectricCar extends Car {
    public function startEngine() {
        echo "Starting electric car engine";
    }
}

$myElectricCar = new ElectricCar();
$myElectricCar->startEngine();
?>
~~~~~~~
> Output: Starting electric car engine

##### Abstract Classes (Soyut Sınıflar)
+ Doğrudan örneklenemeyen ve diğer class'lar için bir temel olarak hizmet veren class'lardır. Abstract class'lar, bir veya daha fazla soyut yöntem (abstract methods) içerebilir. Soyut yöntemler, yalnızca bildirimden ibarettir ve alt class'lar tarafından gerçekleştirilmelidir.

###### Abstract Class Tanımlama
+ Bir class'ı soyut yapmak için `abstract` keyword kullanılır.
~~~~~~~
<?php

abstract class Animal {
    // Abstract method (no implementation)
    abstract protected function makeSound();

    // Normal method (has implementation)
    public function sleep() {
        echo "Sleeping...";
    }
}
?>
~~~~~~~

###### Abstract Class'ı Kullanma
+ Abstract class'lar doğrudan örneklenemez, ancak bu class'ları miras alan alt class'lar oluşturulabilir ve kullanılabilir.
~~~~~~~
<?php

class Dog extends Animal {
    // Implementing the abstract method in the subclass
    public function makeSound() {
        echo "Bark";
    }
}

$dog = new Dog();
$dog->makeSound();
$dog->sleep();
?>
~~~~~~~
> Output: Bark

> Output: Sleeping...

###### Interfaces (Arayüzler)
+ Class'ların gerçekleştirmesi gereken yöntemlerin bir listesini tanımlar. Interface'ler, herhangi bir uygulama (implementation) içermez, sadece hangi yöntemlerin bulunması gerektiğini belirtir. Bir class birden fazla interface uygulayabilir.

###### Interface Tanımlama
~~~~~~~
<?php

interface AnimalInterface {
    public function makeSound();
}
?>
~~~~~~~

###### Interface Kullanma
+ Bir class, implements keyword ile bir veya daha fazla interface uygulayabilir.
~~~~~~~
<?php

class Cat implements AnimalInterface {
    public function makeSound() {
        echo "Meow";
    }
}

$cat = new Cat();
$cat->makeSound();
?>
~~~~~~~
> Output: Meow

##### Abstraction Kullanımının Avantajları
+ `Karmaşıklığın Gizlenmesi:` Kullanıcıya sadece gerekli olan bilgileri sunarak detayları gizler.
+ `Kodun Yeniden Kullanımı:` Abstract class'lar ve interface'ler, farklı class'ların ortak davranışlarını belirlemek için kullanılabilir.
+ `Esneklik:` Bir class'ın birden fazla interface'i uygulayabilmesi, esnek ve modüler bir tasarıma olanak tanır.
+ `Bakım ve Güncelleme Kolaylığı:` Abstraction, kodun daha kolay bakım ve güncelleme yapılabilir olmasını sağlar.

###### Araç Abstract Class ve Farklı Araçlar
~~~~~~~
<?php

// Abstract class
abstract class Vehicle {
    protected $color;

    public function __construct($color) {
        $this->color = $color;
    }

    // Abstract method
    abstract protected function startEngine();

    public function getColor() {
        return $this->color;
    }
}

// Car class
class Car extends Vehicle {
    public function startEngine() {
        echo "Car engine started";
    }
}

// Motorcycle class
class Motorcycle extends Vehicle {
    public function startEngine() {
        echo "Motorcycle engine started";
    }
}

$car = new Car("Red");
echo $car->getColor();
$car->startEngine();

$motorcycle = new Motorcycle("Blue");
echo $motorcycle->getColor();
$motorcycle->startEngine();
?>
~~~~~~~
> Output: Red

> Output: Car engine started

> Output: Blue

> Output: Çıktı: Motorcycle engine started

#### OOP'nin Avantajları
+ `Kodun Yeniden Kullanılabilirliği:` Class'lar ve nesneler, kodun yeniden kullanılmasını sağlar.
+ `Modülerlik:` Kod modüllere ayrılarak daha yönetilebilir hale gelir.
+ `Bakım Kolaylığı:` Kapsülleme ve modülerlik sayesinde kodun bakımı ve güncellenmesi kolaylaşır.
+ `Gelişmiş Güvenlik:` Kapsülleme, veriyi gizleyerek yetkisiz erişimden korur.

> PHP'de OOP, class'lar ve nesneler kullanarak daha modüler, esnek ve bakımının kolay olduğu yazılımlar geliştirmeye olanak tanır. OOP'nin temel kavramları olan classes, objects, encapsulation, inheritance, polymorphism ve abstraction, kodun organizasyonunu ve yönetimini kolaylaştırır. OOP yaklaşımı, büyük ve karmaşık projelerde kodun daha okunabilir, yeniden kullanılabilir ve sürdürülebilir olmasını sağlar.

***
### Static Properties & Methods In Object Oriented (nesne yönelimli statik özellikler ve yöntemler)
+ PHP'de statik özellikler (static properties) ve yöntemler (static methods), belirli bir class'a ait olan ancak bu class'ın nesnelerine bağlı olmayan özellikler ve yöntemlerdir.
+ Statik özellikler ve yöntemler, class'ın tüm örnekleri tarafından paylaşılır ve class adı kullanılarak doğrudan erişilebilir.

#### Static Properties (Statik Özellikler)
+ `static` keyword ile kullanılarak tanımlanır. Statik özellikler, belirli bir class'ın tüm örnekleri tarafından paylaşılır ve class'ın herhangi bir örneği oluşturulmadan da kullanılabilir.
~~~~~~~
<?php

class MyClass {
    public static $myStaticProperty = "This is a static property.";

    public static function showStaticProperty() {
        echo self::$myStaticProperty;
    }
}

// Access a static property using a class name
echo MyClass::$myStaticProperty;

// Access a static property from a static method inside the class
MyClass::showStaticProperty();
?>
~~~~~~~
> Output: This is a static property.

> Output: This is a static property.

#### Static Methods (Statik Yöntemler)
+ `static` keyword ile kullanılarak tanımlanır. Statik method'lar, class'ın örneği olmadan çağrılabilir ve sadece class'ın statik özelliklerine ve diğer statik yöntem'lerine erişebilir.
~~~~~~~
<?php

class MyClass {
    public static function myStaticMethod() {
        echo "This is a static property.";
    }
}

// Accessing a static method using a class name
MyClass::myStaticMethod();
?>
~~~~~~~
> Output: This is a static property.

###### Static properties ve methods bir class'ta nasıl tanımlanıp kullanıldığına dair bir örnek 
~~~~~~~
<?php

class Counter {
    private static $count = 0;

    public static function increment() {
        self::$count++;
    }

    public static function getCount() {
        return self::$count;
    }
}

// Static methods and properties can be used without an instance of the class
Counter::increment();
Counter::increment();
echo Counter::getCount();
?>
~~~~~~~
> Output: 2

#### Static Properties & Methods'un Avantajları
+ `Bellek Tasarrufu:` Statik özellikler ve yöntemler, class'ın örneklerine bağlı olmadığı için bellek tasarrufu sağlar.
+ `Kolay Erişim:` Statik yöntemler ve özellikler, class adı kullanılarak doğrudan erişilebilir.
+ `Ortak Veri:` Tüm class örnekleri arasında ortak veri paylaşımı sağlar.

#### Static Properties & Methods'un Sınırlamaları
+ `Class Düzeyinde Kullanım:` Statik özellikler ve yöntemler, class'ın örneklerine özgü değildir.
+ `Sadece Statik Üyelere Erişim:` Statik yöntemler, sadece statik özelliklere ve diğer statik yöntemlere erişebilir.

> PHP'de statik özellikler ve yöntemler, belirli bir class'a ait olan ancak class'ın nesnelerine bağlı olmayan özellikler ve yöntemlerdir. Statik üyeler, class adı kullanılarak doğrudan erişilebilir ve class'ın herhangi bir örneği oluşturulmadan kullanılabilir. Bu, belleği daha verimli kullanmayı ve class'ın tüm örnekleri arasında ortak veri paylaşımını sağlar. Statik özellikler ve yöntemler, yazılım geliştirme sürecinde belirli senaryolarda oldukça kullanışlı olabilir.

***
### Interface (arayüz) nedir?
+ Nesne yönelimli programlamanın (OOP) bir özelliğidir ve class'lar arasında bir sözleşme veya belirli bir davranışı tanımlar.
+ Interface'ler, class'ların belirli yöntemleri uygulamasını ve bu yöntemleri sağlamasını zorunlu kılar.
+ PHP'de bir class birden fazla interface'si uygulayabilir, böylece çoklu kalıtımın bir şekilde taklit edilmesine olanak tanır.

##### Interface Tanımlama
+ Interface'ler, `interface` keyword kullanılarak tanımlanır. Interface'lerin yöntemleri sadece bildirimlerden oluşur, yani yöntemlerin gövdesi (implementasyonu) bulunmaz.
~~~~~~~
<?php

interface Logger {
    public function log($message);
}

interface Formatter {
    public function format($data);
}
?>
~~~~~~~

> + `Logger` ve `Formatter` adında iki interface tanımlanmıştır. Her ikisi de bir yöntem içerir, ancak bu yöntemlerin herhangi bir gövdesi yoktur.

##### Interface Uygulama (Implementing an Interface)
+ Bir class, bir veya daha fazla interface'i implements keyword kullanarak uygulayabilir. Interface'de belirtilen tüm yöntemlerin class tarafından tanımlanması gerekmektedir.
~~~~~~~
<?php

class FileLogger implements Logger {
    public function log($message) {
        echo "Logging message to file: $message";
    }
}

class JsonSerializer implements Formatter {
    public function format($data) {
        return json_encode($data);
    }
}
?>
~~~~~~~
> + `FileLogger` class'ı `Logger` interface'ni uygularken, `JsonSerializer` class'ı ise `Formatter` interface'ni uygulamaktadır. Her iki class da interface'de tanımlanan yöntemleri (log ve format) uygulamak zorundadır.

##### Birden Fazla Interface'in Uygulanması
+ Bir class, birden fazla interface uygulayabilir. Interface'ler virgülle ayrılarak sıralanır.
~~~~~~~
<?php

class DatabaseLogger implements Logger, Formatter {
    public function log($message) {
        echo "Logging message to database: $message";
    }

    public function format($data) {
        return serialize($data);
    }
}
?>
~~~~~~~
> + `DatabaseLogger` class'ı hem `Logger` hem de `Formatter` interface'lerini uygulamaktadır.

##### Arayüzün Kalıtımı (Extending an Interface)
+ Bir interface, başka bir interface'den kalıtım alabilir. Kalıtım alınan interface'de tanımlanan yöntemler, kalıtım alan interface'de de bulunur.
~~~~~~~
<?php

interface Formatter {
    public function format($data);
}

interface JsonSerializer extends Formatter {
    public function toJson($data);
}
?>
~~~~~~~
> + `JsonSerializer` interface'si `Formatter` interface'inden kalıtım almıştır. Bu durumda, `JsonSerializer` interface'İnde hem `format` yöntemi hem de `toJson` yöntemi bulunmalıdır.

##### Interface'in Kullanımı
+ Class'lar arasında bir sözleşme sağlar ve kodun daha modüler, esnek ve yeniden kullanılabilir olmasını sağlar.
+ Bir interface'i uygulayan class'lar, belirli bir davranışı sağlayacağından emin olur. Interface'ler ayrıca çoklu kalıtımın eksikliğini telafi eder ve class'ların aynı anda birden fazla sözleşmeyi sağlamasına olanak tanır.
~~~~~~~
<?php

function processData($formatter, $data) {
    echo $formatter->format($data);
}

$serializer = new JsonSerializer();
$data = ['name' => 'Zehra', 'age' => 29];
processData($serializer, $data); // Process data by converting to Json format
?>
~~~~~~~
> + `processData` fonksiyonu, bir `Formatter` interface'i uygulayan herhangi bir nesneyi alabilir. Bu, fonksiyonun esnek bir şekilde farklı veri biçimlerini işleyebilmesini sağlar.

> PHP'de interface'ler, bir class'ın belirli bir davranışı sağlamasını zorunlu kılar. Interface'ler, class'lar arasında bir sözleşme sağlar ve kodun daha modüler, esnek ve yeniden kullanılabilir olmasını sağlar. Bir class bir interface'i implements keyword ile uygular ve interface'de belirtilen tüm yöntemleri tanımlamak zorundadır. Bir class birden fazla interface'i uygulayabilir ve bir interface başka bir interface'den kalıtım alabilir. Interface'ler, genellikle nesnelerin davranışlarını tanımlamak ve değişikliklere karşı esneklik sağlamak için kullanılır.
