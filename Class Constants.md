### Class constants (sınıf sabitleri) nedir?
+ Class düzeyinde tanımlanan ve class'ın tüm örnekleri tarafından paylaşılabilen değişmez değerlerdir.
+ Class sabitleri, class içindeki veya dışındaki kod tarafından erişilebilir ve değiştirilemez.
+ Class sabitleri genellikle sabit verileri saklamak için kullanılır ve `const` keyword ile tanımlanır.

#### Class Sabitlerinin Özellikleri
+ `Sabit Değerler:` Tanımlandıktan sonra değeri değiştirilemez.
+ `Class Düzeyinde Erişim:` Class adı aracılığıyla erişilir.
+ `Erişim Belirleyicileri:` Varsayılan olarak `public` erişime sahiptir, `private` veya `protected` olarak da tanımlanabilir.
+ `Veri Tipi:` Basit veri tiplerini (int, string, float, bool) veya array'leri içerebilir.

#### Class Sabitlerinin Tanımlanması ve Kullanımı
+ Class sabitleri `const` keyword ile  tanımlanır. Class sabitlerine slass adı ile erişilir.

###### Class Sabiti Tanımlama ve Kullanma
~~~~~~~
<?php

class MyClass {
    const MY_CONSTANT = 'Constant Value';

    public function printConstant() {
        echo self::MY_CONSTANT;
    }
}

// Access class constant by class name
echo MyClass::MY_CONSTANT;

// Accessing a class constant from an instance of the class
$instance = new MyClass();
$instance->printConstant();
?>
~~~~~~~
> Output: Constant Value

> Output: Constant Value

> + `self` keyword ile mevcut class ifade edilir. `self::MY_CONSTANT` ile `MY_CONSTANT` sabitine erişilir.

#### Erişim Belirleyicileri
+ Class sabitleri varsayılan olarak `public` erişime sahiptir. Ancak, erişim belirleyicileri kullanılarak erişim seviyesi ayarlanabilir.
~~~~~~~
<?php

class MyClass {
    public const PUBLIC_CONSTANT = 'Everyone';
    protected const PROTECTED_CONSTANT = 'Protected';
    private const PRIVATE_CONSTANT = 'Special';

    public function printConstants() {
        echo self::PUBLIC_CONSTANT;
        echo self::PROTECTED_CONSTANT;
        echo self::PRIVATE_CONSTANT;
    }
}

$instance = new MyClass();
echo MyClass::PUBLIC_CONSTANT;
$instance->printConstants();
?>
~~~~~~~
> Output: Everyone

> Output: EveryoneProtectedSpecial

#### Geçersiz Kılma (Overriding) Yapılamaz
+ Class sabitleri miras alınan class'larda geçersiz kılınamaz. Ancak, alt class kendi sabitini tanımlayabilir.
~~~~~~~
<?php

class BaseClass {
    const MY_CONSTANT = 'Base Value';
}

class SubClass extends BaseClass {
    const MY_CONSTANT = 'Sub Value';
}

echo BaseClass::MY_CONSTANT;
echo SubClass::MY_CONSTANT;
?>
~~~~~~~
> Output: Base Value

> Output: Sub Value

#### Dinamik Class Sabitleri
+ Class sabitlerine dinamik olarak erişmek için `static` kelimesi ve `::` operatörü kullanılabilir.
~~~~~~~
<?php

class MyClass {
    const MY_CONSTANT = 'Constant Value';

    public static function getConstant() {
        return static::MY_CONSTANT;
    }
}

echo MyClass::getConstant();
?>
~~~~~~~
> Output: Constant Value

#### Sabit Array'ler
+ Class sabitleri, basit veri tipleri dışında array'ler de içerebilir.
~~~~~~~
<?php

class MyClass {
    const MY_ARRAY = ['value1', 'value2', 'value3'];

    public function printArray() {
        foreach (self::MY_ARRAY as $value) {
            echo $value . ' ';
        }
    }
}

$instance = new MyClass();
$instance->printArray();
?>
~~~~~~~
> Output: value1 value2 value3

> PHP'de class sabitleri, class düzeyinde tanımlanan ve tüm class örnekleri tarafından paylaşılabilen değişmez değerlerdir. `const` keyword ile tanımlanırlar ve class adı ile erişilirler. Class sabitleri, değiştirilemez olduklarından sabit verileri saklamak için kullanışlıdır ve erişim belirleyicileri ile erişim seviyeleri ayarlanabilir. Ayrıca, sabit array'ler tanımlanabilir ve dinamik olarak erişim sağlanabilir. Class sabitleri, kodun daha okunabilir, düzenli ve bakımının kolay olmasına katkıda bulunur.
