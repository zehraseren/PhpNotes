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
### Creating multiple objects of the same class (aynı class'tan birden fazla object oluşturmak) nedir?
+ Class'ın farklı örneklerini (instances) yaratmak anlamına gelir.
+ Her bir object'in aynı class'tan özelliklerine ve yöntemlerine sahip olduğu, ancak kendi bağımsız durumlarına sahip olduğu anlamına gelir.
+ PHP'de bir class'tan birden fazla nesne oluşturmak oldukça basittir. `new` keyword'ü kullanarak class'ın her bir örneği ayrı ayrı oluşturabilir.

###### Aynı Sınıftan Birden Fazla Nesne Oluşturma
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

// Create multiple objects
$car1 = new Car('Vantablack', 'Volvo XC90');
$car2 = new Car('Dark Blue', 'BMW iX');
$car3 = new Car('Dark Grey', 'Mini Cooper Countryman');

echo $car1->getInfo();
echo $car2->getInfo();
echo $car3->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack Model: Volvo XC90'

> Output: Color: Dark Blue Model: BMW iX

> Output: Color: Dark Grey Model: Mini Cooper Countryman

###### Aynı Sınıftan Oluşturulan Nesneler
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

// Create multiple objects
$car1 = new Car('Red', 'Volvo XC90');
$car2 = new Car('Dark Blue', 'BMW iX');

// Change the properties of the car1 object
$car1->color = 'Vantablack';

// the properties of the car2 object remain unchanged
echo $car1->getInfo();
echo $car2->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack Model: Volvo XC90

> Output: Color: Dark Blue Model: BMW iX

#### Avantajları ve Kullanım Alanları
+ `Nesne Odaklı Programlama (OOP):` Nesne yönelimli programlama prensiplerine uygun olarak, aynı class'tan farklı nesneler oluşturmak, kodun modüler ve yönetilebilir olmasını sağlar.
+ `Bağımsız Durumlar:` Aynı class'tan oluşturulan nesneler, birbirlerinden bağımsız durumlardadır. Bu, bir nesnenin özelliklerinin diğerlerini etkilemediği anlamına gelir.
+ `Yeniden Kullanılabilirlik:` Aynı class'tan birden fazla nesne oluşturarak, class'ın işlevselliğini birden fazla yerde tekrar tekrar kullanabilir.

#### Pratik Kullanım Örnekleri
+ `Veritabanı Kayıtları:` Bir database tablosundaki her satır için bir nesne oluşturabilir.
+ `Oyun Geliştirme:` Bir oyundaki her oyuncu veya her düşman için ayrı nesneler oluşturabilirsiniz.
+ `Form İşleme:` Birden fazla form elemanı veya kullanıcı girişi için nesneler oluşturabilir.

> `Aynı sınıftan birden fazla nesne oluşturmak, PHP'de nesne yönelimli programlamanın temel bir özelliğidir.` Bu, kodu daha modüler, esnek ve yeniden kullanılabilir hale getirir. Her bir nesne kendi bağımsız durumuna sahiptir, bu da kodun bakımını ve anlaşılmasını kolaylaştırır.

----
###  Class destructor (yıkıcı method) nedir?
+ Bir nesnenin yaşam döngüsünün sonuna ulaşıldığında veya bellekten temizlenmeden önce otomatik olarak çağrılan özel bir method'tur.
+ `Destructor`, genellikle class'ın kaynaklarını serbest bırakmak, bağlantıları kapatmak veya temizleme işlemleri yapmak için kullanılır.
+ `__destruct` adıyla tanımlanır. Destructor'lar, class'ın bir örneği bellekten temizlendiğinde otomatik olarak çağrılır. `Bir destructor'ın manuel olarak çağrılması gerekmez; PHP'nin çöp toplayıcısı tarafından otomatik olarak çağrılır.`
~~~~~~~
<?php
class FileOperations {
    private $fileName;
    private $file;

    // Constructor
    public function __construct($fileName) {
        $this->fileName = $fileName;
        $this->file = fopen($fileName, 'w');
        echo "File created: {$this->fileName}\n";
    }

    // Destructor
    public function __destruct() {
        if ($this->file) {
            fclose($this->file);
            echo "File closed: {$this->fileName}\n";
        }
    }

    public function writeToFile($content) {
        fwrite($this->file, $content);
    }
}

// Create a new object and perform file operations
$file = new FileOperations('example.txt');
$file->writeToFile("This is a sample content.");

// The destructor will be called when the object expires or the script ends
?>
~~~~~~~
> + `FileOperations` class'ı, dosya oluşturma ve dosyaya yazma işlemlerini içerir.
> + `__constructor` method'u, dosya açma işlemini gerçekleştirir.
> + `__destruct` method'u, dosya kapatma işlemini gerçekleştirir. Class'ın bir örneği sona erdiğinde veya script tamamlandığında otomatik olarak çağrılır.

#### Destructor'ın Çağrılması
+ `Nesne Yok Edildiğinde:` Bir nesne `unset()` fonksiyonu ile yok edildiğinde veya referansı bittiğinde.
+ `Script Sonlandığında:` Script'in çalışması sona erdiğinde ve tüm nesneler bellekten temizlendiğinde.

#### Kullanım Alanları
+ `Kaynak Serbest Bırakma:` Açık dosya tanıtıcıları, database bağlantıları gibi kaynakları serbest bırakmak.
+ `Geçici Dosyaları Temizleme:` Geçici dosyaları veya geçici verileri silmek.
+ `Loglama:` Nesnenin yaşam döngüsünün sonunda loglama yapmak.
+ `Bellek Yönetimi:` Bellekte tutulan büyük veri yapılarını serbest bırakmak.

#### Dikkat Edilmesi Gerekenler
+ `Performans:` Destructor içinde uzun süreli işlemler yapmaktan kaçınılmalıdır. Destructor'lar, nesnenin yaşam döngüsünün sonuna ulaşıldığında çağrıldığı için, uzun süreli işlemler performans sorunlarına yol açabilir.
+ `Ölüm Noktası (Dead Code):` Destructor içindeki kodun bazen çalışmama ihtimali olabilir. Özellikle zorunlu kaynak serbest bırakma işlemleri için kesinlikle güvenilmemelidir.
+ `Manuel Çağrı Yapılmaması:` Destructor, manuel olarak çağrılmamalıdır. PHP'nin bellek yönetimi ve çöp toplayıcısı tarafından otomatik olarak çağrılır.

> PHP'de `destructor (__destruct)`, bir class'ın örneği yok edildiğinde veya script tamamlandığında otomatik olarak çağrılan özel bir method'dur. Destructor'lar, kaynakları serbest bırakmak ve temizleme işlemleri yapmak için kullanılır. Destructor'lar, nesne yaşam döngüsünün sonunda çağrıldıkları için, kaynak yönetimi ve bellek temizliği açısından önemlidirler.

----
### stdClass & object casting (nesneye dönüştürme) nedir?
+ Temel veri yapılarını ve anonim nesneleri kullanmanın yaygın yollarıdır. Bu yöntemler, PHP'de dinamik ve esnek veri manipülasyonları yapmak için kullanılır.

#### stdClass
+ PHP'de anonim nesneler oluşturmak için kullanılan yerleşik bir class'tır.
+ Özel olarak tanımlanmış bir class değildir; daha çok PHP'de `nesne olarak davranacak boş bir konteyner class'ı olarak düşünülebilir.`
~~~~~~~
<?php
// Create an empty object using stdClass
$object = new stdClass();

// Add property
$object->name = "Zehra";
$object->age = 29;
$object->profession = "Software Developer";

// Accessing properties
echo $object->name;
echo $object->age;
echo $object->profession;
?>
~~~~~~~
> Output: Zehra

> Output: 29

> Output: Software Developer

#### Object Casting (Nesneye Dönüştürme)
+ Array veya diğer veri türlerini nesneye dönüştürür.
+ Veri türünün başına `(object)` eklenerek yazılır.

##### Array ile Nesneye Dönüştürme
+ Bir array nesneye dönüştürüldüğünde, array key'leri nesnenin özelliklerine dönüşür ve array değerleri de bu özelliklerin değerleri olur.
~~~~~~~
<?php
$dizi = [
    "name" => "Zehra",
    "age" => 29,
    "profession" => "Software Developer"
];

// Convert array to object
$object = (object) $array;

// Accessing properties
echo $object->name;
echo $object->age;
echo $object->profession;
?>
~~~~~~~
> Output: Zehra

> Output: 29

> Output: Software Developer

##### Diğer Veri Türleri ile Nesneye Dönüştürme
+ Diğer veri türleri de nesneye dönüştürülebilir. Ancak, bir veri türünü nesneye dönüştürürken, türün içeriği ve yapısına dikkat etmek önemlidir:
~~~~~~~
<?php
// Convert string to object
$string = "Hello World";
$object = (object) $string;

// Access object property
echo $object->scalar;

// Convert integer to array
$number = 42;
$object = (object) $number;

// Access object property
echo $object->scalar;
?>
~~~~~~~
> Output: Hello World

> Output: 42

> + `Scalar:` PHP'de scalar veri türleri (string, integer, float, boolean) nesneye dönüştürüldüğünde, dönüştürülen nesne scalar adlı bir özelliğe sahip olur. Bu özellik, orijinal scalar değeri içerir.
> + Bu davranış, PHP'nin scalar değerleri nesneye dönüştürme işleminin doğal bir sonucudur. Scalar değer nesneye dönüştürüldüğünde, bu değerin bir özelliğe atanması gerekmektedir ve PHP bu özelliğe scalar adını vermiştir. Bu nedenle, string, integer, float veya boolean bir değeri nesneye dönüştürdüğünüzde, bu değeri `scalar` özelliği aracılığıyla erişilir.

#### stdClass ve Object Casting Kullanım Alanları
+ `Dinamik Veri Yapıları:` Veritabanından gelen verileri veya JSON API yanıtlarını dinamik olarak işlemek için kullanılır.
+ `Geçici Veri Depolama:` Küçük ve geçici veri saklama işlemleri için kullanışlıdır.
+ `JSON İşleme:` JSON verilerini doğrudan nesne olarak işlemek için kullanılabilir.
~~~~~~~
<?php
$json = '{"name": "Zehra", "age": 29, "profession": "Software Developer"}';

// Convert JSON data to stdClass object
$object = json_decode($json);

// Access property
echo $object->name;
echo $object->age;
echo $object->profession;
?>
~~~~~~~
> Output: Zehra

> Output: 29 

> Output: Software Developer
