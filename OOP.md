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
#### Objects | Objects
+ Class'lardan türetilen varlıklardır. Class'ların somut örnekleridir.
~~~~~~~
<?php

$myCar = new Car("vantablack", "Volvo XC90");
echo $myCar->message();
?>
~~~~~~~
> Output: My car is a vantablack Volvo XC90.

***
#### Properties | Objects
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
+ Bir class'ın başka bir class'tan özellik ve yöntemleri miras almasını sağlar.
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
#### Polymorphism | Polimorfizm
+ Aynı isimli method'ların farklı şekillerde kullanılabilmesidir.
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
> Output:

> Output:

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

###### Soyut Sınıf Tanımlama
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
> Output: 

> Output: 

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
+ Bir class, implements keyword ile bir veya daha fazla interface's uygulayabilir.
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
+ `Kodun Yeniden Kullanımı:` Soyut class'lar ve interface'ler, farklı class'ların ortak davranışlarını belirlemek için kullanılabilir.
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

// Bike class
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
+ `Sınıf Düzeyinde Kullanım:` Statik özellikler ve yöntemler, class'ın örneklerine özgü değildir.
+ `Sadece Statik Üyelere Erişim:` Statik yöntemler, sadece statik özelliklere ve diğer statik yöntemlere erişebilir.

> PHP'de statik özellikler ve yöntemler, belirli bir class'a ait olan ancak class'ın nesnelerine bağlı olmayan özellikler ve yöntemlerdir. Statik üyeler, class adı kullanılarak doğrudan erişilebilir ve class'ın herhangi bir örneği oluşturulmadan kullanılabilir. Bu, belleği daha verimli kullanmayı ve class'ın tüm örnekleri arasında ortak veri paylaşımını sağlar. Statik özellikler ve yöntemler, yazılım geliştirme sürecinde belirli senaryolarda oldukça kullanışlı olabilir.
