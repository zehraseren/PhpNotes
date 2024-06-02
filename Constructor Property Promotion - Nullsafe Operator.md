### Constructor Property Promotion (Yapıcı Özellik Tanıtımı) nedir?
***
+ Class yapıcılarında (constructor) özelliklerin daha kısa ve daha okunabilir bir şekilde tanımlanmasını sağlar.
+ Class yapıcılarına parametre olarak geçirilen özelliklerin doğrudan class özellikleri olarak tanımlanmasını ve başlatılmasını sağlar, böylece tekrarlayan kod miktarını azaltır.

##### Geleneksel Yöntem ile Özellik Tanımlama ve Başlatma
+ PHP 8'den önce, class özelliklerini tanımlamak ve bunları yapıcı içinde başlatmak için kod yazmak biraz daha uzun ve tekrarlayıcı şekildedir.
~~~~~~~
<?php
class Car {
    private $color;
    private $model;
    private $year;

    // Constructor
    public function __construct($color, $model, $year) {
        $this->color = $color;
        $this->model = $model;
        $this->year = $year;
    }

    public function getInfo() {
        return "Color: {$this->color}, Model: {$this->model}, Year: {$this->year}";
    }
}

$car = new Car('Vantablack', 'Volvo XC90', 2024);
echo $car->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack, Model: XC90, Year: 2024

##### Constructor Property Promotion Kullanımı
+ PHP 8 ile gelen Constructor Property Promotion sayesinde, yukarıdaki kodu daha kısa ve okunabilir hale getirebilir.
~~~~~~~
<?php
class Car {
    // Constructor with property promotion
    public function __construct(
        private string $color,
        private string $model,
        private int $year
    ) {}

    public function getInfo() {
        return "Color: {$this->colır}, Model: {$this->model}, Year: {$this->year}";
    }
}

$car = new Car('Vantablack', 'Volvo XC90', 2024);
echo $car->getInfo();
?>
~~~~~~~
> Output: Color: Vantablack, Model: XC90, Year: 2024

#### Constructor Property Promotion Avantajları
+ `Daha Az Kod:` Özellik tanımlamaları ve yapıcı içinde atamalar tek satırda yapılır, böylece kod miktarı azalır.
+ `Daha Okunabilir Kod:` Kodun okunabilirliği artar ve gereksiz tekrarlardan kaçınılır.
+ `Bakım Kolaylığı:` Daha az kod yazıldığından, kodun bakımı ve güncellenmesi daha kolay olur.

#### Detaylar ve Kurallar
+ Özellikler, doğrudan yapıcı parametreleri olarak tanımlanır ve başlatılır.
+ Tanımlanan her parametre, yapıcı içinde otomatik olarak özellik olarak tanımlanır ve başlatılır.
+ `Her özellik için erişim belirleyici (public, protected, private) belirtilmelidir.`

###### Farklı Veri Türleri ve Varsayılan Değerler İçeren Bir Örnek
~~~~~~~
<?php
class Person {
    public function __construct(
        private string $name,
        private int $age,
        private string $gender = 'Unspecified'
    ) {}

    public function getInfo() {
        return "Name: {$this->name}, Age: {$this->age}, Gender: {$this->gender}";
    }
}

$person = new Person('Zehra', 29);
echo $person->getInfo();
?>
~~~~~~~
> Output: Name: Zehra, Age: 29, Gender: Unspecified

> Constructor Property Promotion, PHP 8 ile gelen ve `class yapıcılarında özellik tanımlama ve başlatma işlemlerini basitleştiren güçlü bir özelliktir.` Bu özellik sayesinde, daha az kod yazarak daha okunabilir ve bakım kolaylığı sağlayan class'lar oluşturabilir. Bu yenilik, özellikle büyük ve karmaşık projelerde kodun sadeleşmesine ve hataların azalmasına yardımcı olur.

***
### Nullsafe operator | `?->` nedir?
+ `null` olabilecek nesneler üzerinde güvenli bir şekilde method çağırmak veya özelliklere erişmek için kullanılır.
+ `null` olabilecek bir nesne üzerinde zincirleme işlemler yaparken, null kontrolü yapma ihtiyacını ortadan kaldırarak kodu daha temiz ve okunabilir hale getirir.
+ Bir nesnenin `null` olup olmadığını kontrol eder ve eğer nesne null ise, işlemi güvenli bir şekilde durdurur ve `null` döner. Eğer nesne null değilse, normal `->` operatörü gibi çalışır ve methıd'u çağırır veya özelliğe erişir.

##### Geleneksel Yöntem ile Null Kontrolü
+ PHP 8'den önce, bir nesne null olabileceği durumlarda null kontrolü yaparak işlemler gerçekleştirilir.
~~~~~~~
<?php
class Adress {
    public $city;

    public function __construct($city) {
        $this->city = $city;
    }
}

class Person {
    public $name;
    public $address;

    public function __construct($name, $address = null) {
        $this->name = $name;
        $this->address = $address;
    }
}

$kisi = new Person('Ali', new Adress('Istanbul'));

// Direct access without null check may cause an error
if ($person->address !== null) {
    $city = $person->address->city;
    echo $city;
}

// Secure access with null check
$person2 = new Person('Zehra');
$city2 = $person2->address !== null ? $person2->address->city : null;
echo $city2;
?>
~~~~~~~
> Output: İstanbul

> Output: null

##### Nullsafe Operatörü ile Null Kontrolü
+ PHP 8'in Nullsafe operatörü sayesinde yukarıdaki kontrol çok daha temiz bir şekilde yapılabilir.
~~~~~~~
<?php
class Address {
    public $city;

    public function __construct($city) {
        $this->city = $city;
    }
}

class Person {
    public $name;
    public $address;

    public function __construct($name, $address = null) {
        $this->name = $name;
        $this->address = $address;
    }
}

$person = new Person('Zehra', new Adress('Istanbul'));
$city = $person->address?->city;
echo $city;

$person2 = new Person('Büşra');
$city2 = $person2->address?->city;
echo $city2;
?>
~~~~~~~
> Output: İstanbul

> Output: null

> + `Address` class bir `city` özelliğine sahiptir.
> + Yapıcı method (__construct), `city` parametresi alır ve bu değeri `city` özelliğine atar.
> + `Person` class, name ve address olmak üzere iki özelliğe sahiptir.
> + Yapıcı method (__construct), `name` ve opsiyonel olarak `adress` parametrelerini alır. `address` parametresi verilmezse, varsayılan olarak `null` olur.
> + Nesnelerin Oluşturulması:
>   - `Address` class'ından bir nesne oluşturulur ve `city` özelliği 'Istanbul' olarak atanır.
>   - Bu `Address` nesnesi, `Person` class'ının yapıcı method'una `address` parametresi olarak verilir.
>   - `Person` class'ından oluşturulan `person` nesnesinin `name` özelliği 'Zehra', `address` özelliği ise `Address` nesnesidir.
>   - `Person` class'ından bir başka nesne oluşturulur, ancak bu sefer `address` parametresi verilmez. Dolayısıyla, `address` özelliği null olur.
> + Nullsafe Operatörü Kullanımı:
>   - `$person->address` ifadesi bir `Address` nesnesine işaret eder. Bu nedenle `?->` operatörü `city` özelliğine erişir ve 'Istanbul' değerini alır.
>   - Ekrana 'Istanbul' yazdırılır.
>   - `$person2->address` ifadesi `null` olduğu için, `?->` operatörü `city` özelliğine erişmeye çalışmaz ve `null` değeri döner.
>   - Ekrana `null` yazdırılır.

#### Nullsafe Operatörünün Avantajları
+ `Daha Temiz Kod:` Null kontrolü yapma ihtiyacını ortadan kaldırarak kodu daha temiz ve okunabilir hale getirir.
+ `Hata Önleme:` Null referans hatalarını önler, çünkü null bir nesneye erişmeye çalışmak yerine güvenli bir şekilde null döner.
+ `Zincirleme İşlemler:` Birden fazla seviyede null kontrolü yaparak karmaşık nesne yapılarında güvenli erişim sağlar.

#### Zincirleme Kullanım
~~~~~~~
<?php
class Country {
    public $name;
    public $capital;

    public function __construct($name, $capital) {
        $this->name = $name;
        $this->capital = $capital;
    }
}

class Person {
    public $name;
    public $address;
    public $country;

    public function __construct($name, $address = null, $country = null) {
        $this->name = $name;
        $this->address = $address;
        $this->country = $country;
    }
}

$country = new Country('Türkiye', 'İstanbul');
$person = new Person('Zehra', null, $country);

// Using the nullsafe operator with chained operations
$capital = $name->country?->capital;
echo $capital;

$person2 = new Person('Büşra');
$capital2 = $person2->country?->capital;
echo $capital2;
?>
~~~~~~~
> Output: İstanbul

> Output: null
