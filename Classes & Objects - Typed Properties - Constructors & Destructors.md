### Objects & Classes nedir?
+ PHP'de objects ve classes, nesne yönelimli programlama (OOP) paradigmasının temel yapı taşlarıdır.
+ `OOP`, yazılım geliştirmede veri ve işlevselliği bir arada tutarak daha modüler, okunabilir ve bakımı kolay kod yazmayı amaçlar. 

#### Classes
+ Nesnelerin (objects) şablonlarıdır.
+ Bir class, veri üyeleri (properties) ve yöntemler (methods) içerir. Properties, class'ın sahip olduğu özellikleri, methods ise bu özellikler üzerinde gerçekleştirilebilen işlemleri tanımlar.
+ Bir class'ı tanımlamak için `class` keyword'ü kullanılır.
~~~~~~~
<?php
class Car {
    // Properties
    public $color;
    public $model;

    // Methods
    public function color($color) {
        $this->color = $color;
    }

    public function color() {
        return $this->color;
    }

    public function set_model($model) {
        $this->model = $model;
    }

    public function get_model() {
        return $this->model;
    }
}
?>
~~~~~~~

#### Objects
+ Class'lardan türetilen varlıklardır.
+ Bir class'tan object (nesne) oluşturulduğunda, o class'ın özelliklerine ve yöntemlerine sahip bir örnek (instance) elde edilir. Nesne oluşturmak için `new` keyword'ü kullanılır.
~~~~~~~<?php
// Let's assume the car class as we defined it above

// Create a new object
$myCar = new Car();

// Set the properties of the object
$myCar->set_color('Vantablack');
$myCar->set_model('Volvo XC90');

// Get the properties of the object and print them to the screen
echo "Color of the car: " . $myCar->get_color();
echo "Model of the car: " . $myCar->get_model();
?>
~~~~~~~
> Output: Color of the car: Vantablack

> Output: Model of the car: Volvo XC90

----
### Class properties (sınıf özellikleri) nedir?
+ Class properties, bir class'ın veri üyeleridir ve class'ın sahip olduğu verileri tanımlar. Bu özellikler, `class'ın içinde tanımlanır ve belirli erişim belirleyicileri (access modifiers) ile korunur.`
+ Erişim belirleyicileri, bu özelliklerin sınıfın içinde mi yoksa dışında mı erişilebilir olduğunu belirler.

##### Access Modifiers (Erişim Belirleyicileri)
+ PHP'de üç ana erişim belirleyicisi vardır:
  - `public:` Property veya method herkese açıktır ve class'ın dışından erişilebilir.
  - `protected:` Property veya method sadece class'ın içinde ve bu class'tan türetilen sub class'larda erişilebilir.
  - `private:` Property veya method sadece class içinde erişilebilir.
~~~~~~~
<?php
class Car {
    // Public property
    public $color;

    // Protected property
    protected $model;

    // Private property
    private $engineDisplacement;

    // Public method
    public function setEngineVolume($displacement) {
        $this->engineDisplacement = $displacement;
    }

    // Public method
    public function getEngineDisplacement() {
        return $this->engineDisplacement;
    }
}
?>
~~~~~~~

##### Class Özelliklerine Erişim
+ Class özelliklerine erişmek için genellikle nesneler oluşturulur ve bu nesneler üzerinden özellikler çağrılır. 
~~~~~~~
<?php
// Let's assume the car class as we defined it above

// Create a new object
$myCar = new Car();

// Direct access to Public property
$myCar->color = 'Vantablack';
echo "Color of the car: " . $my->color;

// Direct access to Private property (will give an error)
// $myCar->engineDisplacement = 2.5; // Fatal error: Uncaught Error: Cannot access private property

// Indirect access to private property (using getter and setter methods)
$myCar->setEngineDisplacement(2.5);
echo "Engine displacement: " . $myCar->getEngineDisplacement();
?>
~~~~~~~
> Output: Color of the car: Vantablack

> Output: Engine displacement: 2000

----
### Typed properties nedir?
+ PHP 7.4 ile tanıtılan typed properties, class özelliklerine tür belirtme (type hinting) olanağı sağlar.
+ Typed properties, bir class özelliğinin yalnızca belirli bir türde veri tutabileceğini garanti eder. Bu, `kodun daha güvenli ve hatalara karşı daha dayanıklı olmasını sağlar.`
+ Typed properties, özelliğin türünü belirtmek için özellik adının önüne yazılan tür adıyla tanımlanır.
+ Desteklenen türler arasında yerleşik türler (int, float, string, bool), class adları, array, object, callable, iterable ve null olmasına izin vermek için `?` operatörü ile işaretlenen türler yer alır.
~~~~~~~
<?php
class Car {
    // Typed properties
    public string $color;
    protected int $modelYear;
    private float $engineDisplacement;

    // Nullable typed property
    public ?string $ownerName = null;

    public function __construct(string $color, int $modelYear, float $engineDisplacement) {
        $this->color = $color;
        $this->modelYear = $modelYear;
        $this->engineDisplacement = $engineDisplacement;
    }
}
?>
~~~~~~~
###### Typed properties kullanarak object oluşturma ve özelliklere erişim:
~~~~~~~
<?php
// Let's assume the car class as we defined it above

// Create a new object
$myCar = new Car('Vantablack', 2024, 2.5);

// Assign and read values with typed properties
echo "Color of the car: " . $myCar->color;

// Assign value to nullable property
$myCar->ownerName = 'Zehra';
echo "Owner of the car: " . $myCar->ownerName;

// Make nullable property null
$myCar->ownerName = null;
echo "Owner of the car: " . ($myCar->ownerName ?? 'Unknown');

// Assigning the wrong type of value (will give an error)
// $myCar->color = 123; // Fatal error: Uncaught TypeError: Cannot assign int to property Car::$color of type string
?>
~~~~~~~
> Output: Color of the car: Vantablack

> Output: Owner of the car: Zehra

> Output: Owner of the car: Unknown

#### Desteklenen Türler ve Null Atama
+ Typed properties, temel veri türlerini ve class adlarını destekler.
+ Ayrıca, bir özelliğin null olabilmesi için `?` operatörü kullanılır. Örneğin, `?string` hem `string` hem de `null` değerlerini kabul eder.
  - int
  - float
  - string
  - bool
  - array
  - object
  - iterable
  - callable
  - ?Type -> Belirtilen tür veya null

----
### Class constructor, $this, & constructor arguments nedir?

#### Class Constructor
+ `__construct` adında özel bir method ile tanımlanır.
+ Bir class'ın yeni bir örneği oluşturulduğunda çağrılır ve genellikle class'ın başlangıç durumunu ayarlamak için kullanılır.
~~~~~~~
<?php
class MyClass {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }
}

$obj = new MyClass("Zehra");
echo $obj->name;
?>
~~~~~~~
> Output: Zehra

#### $this
+ `$this`, class'ın o anki örneğini (instance) ifade eder.
+ Class'ın property'lerine ve method'larına erişmek için kullanılır.

#### Constructor Arguments (Yapıcı Fonksiyon Argümanları)
+ Constructor method'una geçilen parametrelerdir.
+ Bu argümanlar, nesnenin başlangıç durumunu belirlemek için kullanılır.
~~~~~~~
<?php
class MyClass {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }
}

$obj = new MyClass("Zehra");
echo $obj->getName();
?>
~~~~~~~
> Output: Zehra

> + `__construct` method'u, class'ın yeni bir örneği oluşturulduğunda otomatik olarak çağrılır. `$this` ile class'ın içinden, class'ın özelliklerine ve method'larına erişilir. `__construct` method'una geçirilen `$name` argümanı, class'ın örneği oluşturulurken kullanılır.

----
### Methods nedir?
+ Methods, bir class'ın içinde tanımlanan işlevlerdir.
+ Method'lar, class'ın nesneleri (örnekleri) tarafından çağrılabilir ve class'ın özellikleri üzerinde işlem yapabilirler.
+ Method'lar, bir class'ın davranışlarını tanımlar ve genellikle class'ın veri üyeleri üzerinde operasyonlar gerçekleştirmek için kullanılır.
+ `public`, `protected` veya `private` erişim belirleyicileriyle tanımlanabilir.

#### Public Methods
+ `public` method'lar, class'ın her yerinden ve class'ın dışından erişilebilir.
~~~~~~~
<?php
class MyClass {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }

    public function setName($name) {
        $this->name = $name;
    }
}

$obj = new MyClass("Zehra");
echo $obj->getName();
$obj->setName("Büşra");
echo $obj->getName();
?>
~~~~~~~
> Output: Zehra

> Output: Büşra

#### Protected Methods
+ `protect` method'lar, yalnızca class'ın içinde ve bu class'tan türetilmiş (miras alınmış) sub class'lar (alt sınıflar) tarafından erişilebilir.
~~~~~~~
<?php
class MyClass {
    protected $name;

    public function __construct($name) {
        $this->name = $name;
    }

    protected function getName() {
        return $this->name;
    }
}

class ChildClass extends MyClass {
    public function getProtectedName() {
        return $this->getName();
    }
}

$obj = new ChildClass("Zehra");
echo $obj->getProtectedName();
?>
~~~~~~~
> Output: Zehra

#### Private Methods
+ `private` method'lar, yalnızca tanımlandıkları class'ın içinde erişilebilir.
~~~~~~~
<?php
class MyClass {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    private function getName() {
        return $this->name;
    }

    public function getPrivateName() {
        return $this->getName();
    }
}

$obj = new MyClass("John");
echo $obj->getPrivateName();
?>
~~~~~~~
> Output: Zehra

----
### Method chaining (yöntem zincirleme) nedir?
+ Bir object üzerinde birden fazla method'u ardışık olarak çağırma tekniğidir.
+ Bu teknik, kodun daha okunabilir ve kompakt olmasını sağlar.
+ Method chaining, her bir method'un kendisini `$this` döndürmesi sayesinde mümkün olur, bu sayede bir sonraki method çağrısı aynı nesne üzerinde yapılabilir.
+ `Her bir method'un sonunda $this keyword'ü ile mevcut object'i döndürmek gerekir.`
~~~~~~~
<?php
class Car {
    private $color;
    private $model;
    private $speed;

    // Constructor
    public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
        $this->speed = 0;
    }

    // Color determination method
    public function setColor($color) {
        $this->color = $color;
        return $this; // Return the current object for method chaining
    }

    // Model determination method
    public function setModel($model) {
        $this->model = $model;
        return $this; // Return the current object for method chaining
    }

    // Speed determination method
    public function setSpeed($speed) {
        $this->speed = $speed;
        return $this; // Return the current object for method chaining
    }

    // Method to print information on screen
    public function getInfo() {
        return "Color: {$this->color}, Model: {$this->model}, Speed: {$this->speed} km/h";
    }
}

// Create a new object and set properties using method chaining
$myCar = new Car('Red', 'Toyota');
$myCar->setColor('Vantablack')->setModel('Volvo XC90')->setSpeed(120);

echo $myCar->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack, Model: Volvo XC90, Speed: 120 km/h

#### Method Chaining Avantajları
+ `Daha Okunabilir Kod:` Kodun daha okunabilir ve takip edilebilir olmasını sağlar. `Özellikle aynı nesne üzerinde birden fazla işlemin yapılması gerektiğinde yararlıdır.`
+ `Daha Az Kod Tekrarı:` Tek bir satırda birden fazla method'u çağırarak, aynı nesneyi tekrar tekrar belirtmeye gerek kalmaz.
+ `Kompakt Kod:` Uzun kod bloklarını daha kısa ve öz hale getirir.

#### Method Chaining Kullanırken Dikkat Edilmesi Gerekenler
+ `Method'ların Dönüş Değeri:` `Method chaining`'in çalışabilmesi için her bir yöntemin `$this` döndürmesi gerekmektedir.
+ `Hatalar ve Hata Yöntemi:` Method chaining sırasında herhangi bir hata oluşursa, hangi yöntemde hatanın oluştuğunu belirlemek zor olabilir. Bu nedenle, `hata yönetimi stratejilerini iyi belirlemek önemlidir.`
+ `Okunabilirlik:` Her ne kadar method chaining kodu kısaltıp daha kompakt hale getirse de, aşırı kullanımı kodun okunabilirliğini azaltabilir. `Uzun zincirler yerine mantıksal bloklar halinde kullanmak daha iyi olabilir.`
  
> Method chaining, PHP'de nesne yönelimli programlamayı daha etkili hale getirir ve kodun daha temiz, okunabilir ve yönetilebilir olmasını sağlar. Bu teknik, özellikle karmaşık nesne yapılandırmaları ve ardışık işlem zincirleri için oldukça kullanışlıdır.

----
###  Creating objects using variables (değişken kullanarak nesne oluşturma) nedir?
+ Dinamik olarak class adlarını depolamak ve bu adları kullanarak objects oluşturmak için kullanılır.
+ Özellikle class adlarının dinamik olarak belirlendiği veya koşullara bağlı olarak değişebildiği durumlarda yararlıdır.
+ Bir class adını bir değişkende saklayabilir ve bu değişkeni kullanarak object oluşturabilirsiniz. Bu işlem, class adını depolayan değişkenin önüne `new` keyword'ünü koyarak yapılır.
~~~~~~~
<?php
class Car {
    public $color;
    public $model;

    public function __construct($color, $model) {
        $this->color = $color;
        $this->model = $model;
    }

    public function getInfo() {
        return "Color: {$this->color}, Model: {$this->model}";
    }
}

class Bike {
    public $color;
    public $brand;

    public function __construct($color, $brand) {
        $this->color = $color;
        $this->brand = $brand;
    }

    public function getInfo() {
        return "Color: {$this->color}, Brand: {$this->brand}";
    }
}

// Dynamically set a class name
$className = 'Car';
$object = new $className('Vantablack', 'Volvo XC90');
echo $object->getInfo();

// Use a different class name
$className = 'Bike';
$object = new $className('Red', 'Bianchi');
echo $object->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack, Model: Volvo XC90

> Output: Color: Red, Brand: Bianchi

#### Avantajları
+ `Esneklik:` Dinamik olarak class adlarını belirleyebilir ve bu class'lardan nesneler oluşturabilir.
+ `Modülerlik:` Farklı class türleri tek bir yapı içinde işlenebilir.
+ `Koşullu Nesne Oluşturma:` Koşullara bağlı olarak farklı class'lardan object'ler oluşturabilir.

#### Kullanım Alanları
+ `Fabrika Deseni (Factory Pattern):` Fabrika deseni, hangi class'ın örneğinin oluşturulacağının dinamik olarak belirlendiği bir tasarım deseni oluşturup bu yöntemi kullanarak uygulanabilir.
+ `Koşullu İşlemler:` Uygulamada belirli koşullara göre farklı class'ların örnekleri oluşturulması gerektiğinde kullanılabilir.
+ `Eklenti Tabanlı Sistemler:` Eklenti veya modül tabanlı sistemlerde, class adları dinamik olarak belirlenip kullanılabilir.

#### Dikkat Edilmesi Gerekenler
+ `Geçersiz Class Adları:` Değişkende geçersiz veya var olmayan bir class adı belirtilirse, PHP bir hata (fatal error) verecektir. Bu nedenle class adlarının doğruluğunu kontrol etmek önemlidir.
+ `Güvenlik:` Kullanıcı girişi veya dış kaynaklardan gelen verilerle doğrudan class adları belirlenirken dikkatli olunmalıdır. Güvenlik açıklarına neden olabilir.

> Dinamik nesne oluşturma, PHP'de esnek ve dinamik uygulamalar geliştirmeyi sağlar. Bu teknik, özellikle büyük ve modüler projelerde büyük faydalar sağlayabilir.

----
### Creating multiple objects of the same class nedir?
+
~~~~~~~
~~~~~~~
> Output: 

----
###  Class destructor nedir?
+
~~~~~~~
~~~~~~~
> Output: 

----
### stdClass & object casting nedir?
+
~~~~~~~
~~~~~~~
> Output:
