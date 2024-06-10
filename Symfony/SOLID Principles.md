### SOLID Principles nedir?
+ SOLID prensipleri, nesne yönelimli yazılım geliştirme için beş temel prensipten oluşur. Bu prensipler, yazılım tasarımının esnek, sürdürülebilir ve genişletilebilir olmasını sağlar.
+ SOLID, beş prensibin baş harflerinden oluşan bir kısaltmadır:
1. Single Responsibility Principle (SRP) | Tek Sorumluluk Prensibi
2. Open/Closed Principle (OCP) | Açık/Kapalı Prensibi
3. Liskov Substitution Principle (LSP) | Liskov Yerine Geçme Prensibi
4. Interface Segregation Principle (ISP) | Arayüz Ayrımı Prensibi
5. Dependency Inversion Principle (DIP) | Bağımlılıkların Tersine Çevrilmesi Prensibi

#### 1. Single Responsibility Principle (SRP)
+ Bir class, yalnızca tek bir sorumluluğa sahip olmalıdır. Başka bir deyişle, bir class'ın yalnızca bir değişim sebebi olmalıdır.
~~~~~~~
<?php

class User {
    private $username;

    public function __construct($username) {
        $this->username = $username;
    }

    public function getUsername() {
        return $this->username;
    }
}

// Incorrect
class User {
    private $username;

    public function __construct($username) {
        $this->username = $username;
    }

    public function getUsername() {
        return $this->username;
    }

    public function save() {
        // Saving to the database
    }

    public function sendWelcomeEmail() {
        // Sending a welcome email
    }
}

// Correct
class User {
    private $username;

    public function __construct($username) {
        $this->username = $username;
    }

    public function getUsername() {
        return $this->username;
    }
}

class UserRepository {
    public function save(User $user) {
        // Saving to the database
    }
}

class EmailService {
    public function sendWelcomeEmail(User $user) {
        // Sending a welcome email
    }
}
?>
~~~~~~~

#### 2. Open/Closed Principle (OCP)
+ Bir yazılım varlığı (class, modül, fonksiyon, vb.) genişletilmeye açık ancak değiştirilmeye kapalı olmalıdır.
~~~~~~~
<?php

// Incorrect
class Rectangle {
    public $width;
    public $height;
}

class AreaCalculator {
    public function calculate($shape) {
        if ($shape instanceof Rectangle) {
            return $shape->width * $shape->height;
        }
        // It will be necessary to intervene in this method to add new shapes
    }
}

// Correct
interface Shape {
    public function area();
}

class Rectangle implements Shape {
    public $width;
    public $height;

    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }

    public function area() {
        return $this->width * $this->height;
    }
}

class Circle implements Shape {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function area() {
        return pi() * $this->radius * $this->radius;
    }
}

class AreaCalculator {
    public function calculate(Shape $shape) {
        return $shape->area();
    }
}
?>
~~~~~~~

#### 3. Liskov Substitution Principle (LSP)
+ Bir türetilmiş class, türediği temel class'ın yerine geçebilmelidir ve temel class'ın nesneleri yerine kullanılabilmelidir.
~~~~~~~
<?php

class Bird {
    public function fly() {
        // Flying process
    }
}

class Penguin extends Bird {
    public function fly() {
        throw new Exception("Penguins can't fly");
    }
}

// Incorrect
function makeBirdFly(Bird $bird) {
    $bird->fly();
}

$penguin = new Penguin();
makeBirdFly($penguin); // Error: Penguins can't fly

// Correct
abstract class Bird {
    abstract public function move();
}

class Sparrow extends Bird {
    public function move() {
        echo "Flying";
    }
}

class Penguin extends Bird {
    public function move() {
        echo "Swimming";
    }
}

function makeBirdMove(Bird $bird) {
    $bird->move();
}

$sparrow = new Sparrow();
makeBirdMove($sparrow);

$penguin = new Penguin();
makeBirdMove($penguin);
?>
~~~~~~~
> Output: Flying

> Output: Swimming

#### 4. Interface Segregation Principle (ISP)
+ Class'lar, kullanmadıkları yöntemleri içeren arayüzleri (interface) uygulamaya zorlanmamalıdır. Daha küçük, amaca yönelik interface'ler oluşturulmalıdır.
~~~~~~~
<?php

// Incorrect
interface Worker {
    public function work();
    public function eat();
}

class HumanWorker implements Worker {
    public function work() {
        // Human labor working process
    }

    public function eat() {
        // Human worker eating process
    }
}

class RobotWorker implements Worker {
    public function work() {
        // Robot worker working process
    }

    public function eat() {
        // Error: Robot worker does not eat
    }
}

// Correct
interface Workable {
    public function work();
}

interface Eatable {
    public function eat();
}

class HumanWorker implements Workable, Eatable {
    public function work() {
        // Human labor working process
    }

    public function eat() {
        // Human worker eating process
    }
}

class RobotWorker implements Workable {
    public function work() {
        // Robot worker working process
    }
}
?>
~~~~~~~ 

#### 5. Dependency Inversion Principle (DIP)
+ Üst seviye modüller, alt seviye modüllere bağlı olmamalıdır. Her iki modül de soyutlamalara bağlı olmalıdır. Soyutlamalar detaylara bağlı olmamalıdır; detaylar soyutlamalara bağlı olmalıdır.
~~~~~~~
<?php

// Incorrect
class PostgreSQLConnection {
    public function connect() {
        // Connecting to PostgreSQL database
    }
}

class PasswordReminder {
    private $dbConnection;

    public function __construct(PostgreSQLConnection $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}

// Correct
interface DBConnection {
    public function connect();
}

class PostgreSQLConnection implements DBConnection {
    public function connect() {
        // Connecting to a PostgreSQL database
    }
}

class SQLiteConnection implements DBConnection {
    public function connect() {
        // Connecting to a SQLite database
    }
}

class PasswordReminder {
    private $dbConnection;

    public function __construct(DBConnection $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}
?>
~~~~~~~
