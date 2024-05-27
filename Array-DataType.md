***
### Array tanımlama yöntemleri nelerdir?
***
#### 1. Indexed arrays & accessing elements nedir?
+ Elemanların otomatik olarak sayısal index'ler (anahtarlar) ile sıralandığı dizilerdir.
+ Bu dizilerde her eleman bir index numarası ile tanımlanır ve bu numaralar kullanılarak elemanlara erişilir.
~~~~~~~
#array = ["apple", "pear", "banana", "cherry"];
echo $array[1];
~~~~~~~
> Output: pear

1.1. Array'de eleman güncelleme
~~~~~~~
$array[3] = "strawberry";
echo $array[3];
~~~~~~~
> Output: strawberry

1.2. Array'e eleman ekleme
~~~~~~~
#array[] = "melon";
echo #array[4];
~~~~~~~
> Output: melon

1.3. Döngüler ile elemanlara erişim
+ ```for``` döngüsü
~~~~~~~
$array = ["apple", "pear", "banana", "cherry"]

for ($i = 0; i < count($array); $i++) {
  echo $array[$i]." ";
}
~~~~~~~
> Output: apple pear banana cherry

+ ```foreach``` döngüsü
~~~~~~~
$array = ["apple", "pear", "banana", "cherry"]

foreach ($array as $fruit) {
  echo $fruit." ";
}
~~~~~~~
> Output: apple pear banana cherry

#### 2. Undefined array key nedir?
+ Bir array'in var olmayan (undefined) bir key'e erişmeye çalıştığında, bu bir uyarıya (warning) neden olabilir.
+ Bu durum genellikle array'in belirtilen key'ine erişmeye çalışırken meydana gelir, ancak bu anahtar array'de mevcut değildir.
~~~~~~~
$array = ["name" => "Zehra, "age" => 29];
echo $array["surname"];
~~~~~~~
> Output: Undefined array key "surname" in file.

##### Bunu Uyarıyı Önlemek İçin Kullanılan Yöntemler
2.1. ```array_key_exists``` Fonksiyonu
  - Bir key'in array'de mevcut olup olmadığını kontrol etmek için bu fonksiyon kullanılır.
  - Key'in varlığını kontrol ederken, değerin ```null``` olup olmadığına bakılmaz.
  - Bir key'in değeri ```null``` olsa bile array_key_exists() ```true``` döner.
~~~~~~~
$array = ["name" => "Zehra", "age" => 29];

if (array_key_exists("surname", $array)) {
  echo $array["surname"];
} else {
  echo "Key not available.";
}
~~~~~~~

2.2. ```isset``` Fonksiyonu
  - ```array_key_exists``` fonksiyonu ile benzer şekilde çalışır ve bir key'in dizide mevcut olup olmadığını ve ```null``` olup olmadığını kontrol eder.
  - Eğer key'in değeri ```null``` ise, isset() ```false``` döner.
~~~~~~~
$array = ["name" => "Zehra", "age" => 29];

if (isset($array["surname"])) {
  echo $array["surname"];
} else {
  echo "Key not available.";
}
~~~~~~~

##### Not:
+ Başka bir key kontrol yöntemi ```in_array()```'dir. Bu fonksiyonda key yerine değeri kontrol etmek için kullanılabilir.
~~~~~~~
$array = ["apple", "pear", "banana"];

if(in_array("pear", $array)) {
    echo "The value 'pear' is present in the array.";
} else {
    echo "The value 'pear' is not present in the array.";
}

print_r($array);
~~~~~~~
> Output: The value 'pear' is present in the array.
 
2.3. Null Coalescing Operatörünü Kullanma | PHP 7+
  - PHP 7 ve sonrasında, null coalescing operatörü ```??``` kullanılabilir.
  - Bu operatör, belirtilen key array'de mevcut değilse veya ````null``` ise varsayılan bir değeri döner.    
~~~~~~~
$array = ["name"=> "Zehra", "age" => 25];
echo $array["surname"] ?? "Default value";
~~~~~~~

##### Undefined Array Key Durumunda Kullanım Alanları
+ Endefined array key uyarısı genellikle aşağıdaki durumlarda ortaya çıkar:
  - Array'in key'leri dinamik olarak belirlenirken,
  - Form verileri işlenirken ve bazı alanlar boş bırakılmışsa,
  - API veya database sorguları yaparken dönen veriler beklenen anahtarları içermediğinde.

#### 3. Mutate arrays nedir?
+ Array'leri değştirmek (mutate etmek) veya üzerinde değişilik yapmak, array'in içeriğini güncellemeyi, eklemeyi, çıkarmayı veya değiştirmeyi içerir.
+ Array'leri mutasyona uğratmanın çeşitli yolları vardır ve bunlar genellikle belirli işlevler ve operatörler kullanılarak gerçekleştirilir.

##### Mutate Array Yöntemleri
3.1. Array Elemanını Güncelleme
+ Array'in belirli bir key değeri güncellenebilir.
~~~~~~~
$array = ["apple", "pear", "banana"];
$array[1] = "strawberry";

print_r($array);
~~~~~~~
> Output: Array ([0] => apple [1] => strawberry [2] => banana)

3.2. Array'e Elemen Ekleme
+ Array'e yeni bir eleman eklemek için yeni key belirlenebilir veya otomatik ixdexleme kullanılabilir.
~~~~~~~
$array = ["apple", "pear", "banana"];
$array[] = "berry";

print_r($array);
~~~~~~~
> Output: Array ([0] => apple [1] => pear [2] => banana [3] => berry)

3.3. Array'den Eleman Çıkarma
+ Array'den bir eleman çıkar için ```unset``` fonksiyonu veya belirli fonksiyonlar kullanılabilir.
~~~~~~~
$array = ["apple", "pear", "banana"];
unset{$array[1]);

print_r($array);
~~~~~~~
> Output: Array ([0] => apple [1] => banana)

3.4. ```array_push``` ve ```array_pop``` Fonksiyonları
+ ```array_push```, array'in sonuna bir veya daha fazla eleman ekler.
~~~~~~~
$array = ["apple", "pear", "banana"];

array_push($array, "cherry", "strawberry");
print_r($array);
~~~~~~~
> Output: Array ([0] => apple [1] => pear [2] => banana [3] => cherry [4] => strawberry)

+ ```array_pop```, array'in son elemanını çıkartır ve döner.
~~~~~~~
$array = ["apple", "pear", "banana"];

$last_item = array_pop($array);
echo $last_item; // banana
print_r($array);
~~~~~~~
> Output: Array ([0] => apple [1] => pear [2])

3.5. ```array_shift``` ve ```array_unshift``` Fonksiyonları
+ ```array_shift```, array'in ilk elemanını çıkartır ve döner.
~~~~~~~
$array = ["apple", "pear", "banana"];

$first_item = array_shift($array);
echo $first_item; // apple
print_r($array);
~~~~~~~
> Output: Array ([0] => pear [1] => banana)

+ ```array_unshift```, dizinin başına bir veya daha fazla eleman ekler.
~~~~~~~
$array = ["apple", "pear", "banana"];

array_unshift($array, "cheery", "strawberry");
print_r($array);
~~~~~~~
> Output: Array ([0] => cherry [1] => strawberry [2] => apple [3] => pear [4] => banana)

3.6. ```array_splice``` Fonksiyonu
+ Bir array'in belirli bir bölümünü çıkarmak ve yerine başka elemanlar eklemek için kullanılır.
~~~~~~~
$array = ["apple", "pear", "banana", "cherry"];

array_splice($array, 1, 2, "strawberry", "melon");
print_r($array);
~~~~~~~
> Output: Array ([0] => apple, [1] => strawberry, [2] => melon, [3] => cherry)

##### 4. Name your keys (associative arrays) nedir?
+ Name your keys terimi, array'lerin ilişkilendirici array'ler (associative arrays) olarak da adlandırılan yapısına atıfta bulunur.
+ Associative arrays, her bir elemanın belirli bir key'e sahip olduğu array'lerdir. Bu key'ler her bir elemanın benzersiz bir şekilde tanımlandığı, genellikle bir isim veya tanımlayıcı olduğu bilgisini içerir.
+ Associative arrays'in temel özelliği, elemanlara erişmek için sayısal index'lerin (0, 1, 2, vb.) kullanılması yerine belirli anahtarların kullanılmasıdır. Bu, elemanları daha kolay tanımlanmasına ve onlara erişilmesine olanak sağlar.
~~~~~~~
$student = [
  "name" => "Zehra",
  "surname" => "Şeren",
  "age" => 29,
  "university" => "KTU"
];

echo $student["name"];
echo $student["age"];
~~~~~~~
> + Output: Zehra
> + Output: 29

##### Özellikler
+ ```Keys:``` Associative arrays'in her elemanı bir key'e sahiptir. Bu key'ler elemanların benzersiz bir şekilde tanımlandığı belirleyicilerdir.
+ ```Values:``` Her key ile ilişkilendirilen bir değer vardır. Bu değer, ilgili elemanın içeriğini temsil eder.
+ ```Belirli Sıra Yoktur:``` Associative arrays'de elemanlar belirli bir sıra içinde değildir. Bu nedenle, elemanlara erişirken sayısal index'ler (0, 1, 2, vb.) yerine key'ler kullanılır.
+ ```Esneklik:``` Associative arrays, elemanların belirli bir key ile tanımlanmasını sağladığından, data'ya daha anlamlı bir yapı kazandırır ve elemanlara daha kolay erişim sağlar.
> Associative arrays, özellikle farklı veri türlerini gruplamak ve ilişkilendirmek için kullanışlıdır.

***
### 5. Multi-dimensional arrays nedir?
***
+ Çok boyutlu array'ler (multi-dimensional arrays), diğer array'lerin içinde array'lerin bulunduğu ve verilerin hiyerarşik olarak düzenlendiği özel bir dizi türüdür.
+ Bu array'ler tek bir array içinde birden çok boyuta sahip veri yapıları oluşturulmasına olanak sağlar.
+ Her bir boyut, bir array içindeki iç içe geçmiş bir başka array'e karşılık gelir.
+ Multi-dimensional array'ler, genellikle matrisler, tablolar, ağaçlar gibi data yapılarını temsil etmek için kullanılır. Örneğin, bir matrisi temsil etmek için iki boyutlu bir array kullanılabilir, her bir satırı ve sütunu belirli bir anahtarla erişilebilir.
~~~~~~~
$matris = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

print_r($matris);
~~~~~~~
> Output: Array ( [0] => Array ( [0] => 1 [1] => 2 [2] => 3 ) [1] => Array ( [0] => 4 [1] => 5 [2] => 6 ) [2] => Array ( [0] => 7 [1] => 8 [2] => 9 ) )
> + ```$matris``` adlı iki boyutlu array'de her biri iç içe geçmiş bir array ve satır bulunur.
> + İlk boyut, satırları ve ikinci boyut sütunları temsil eder.
> + ```Bu şekilde, çok boyutlu array'ler karmaşık data yapılarını düzenlemek için kullanılabilir.``` 

##### Örnekler
+ İki Boyutlu Array
~~~~~~~
$two_dimensional = [
  ["Ahmet", "Yılmaz", 25],
  ["Ayşe", "Kara", 30],
  ["Mehmet", "Öztürk", 22]
];

echo($two_dimensional)[0][0];
echo($two_dimensional)[1][1];
~~~~~~~
> Output: Ahmet

> Output: Kara

+ Üç Boyutlu Array
~~~~~~~
$three_dimensional = [
  [
    ["apple", "pear"],
    ["cherry", "strawberry"],
  ],
  [
    ["banana", "pineapple"],
    ["kiwi", "mango"],
  ],
];

echo($three_deminsional)[0][0][1];
echo($three_deminsional)[1][1][0];
~~~~~~~
> Output: pear

> Output: kiwi

> ```Bir array'in belirli bir elemanının değerini yazdırmak için "echo" kullanabilir.```

***
### 6. Array'lerde Duplicate keys & overwriting nedir?
***
+ Array'lerde key'ler benzersiz olmalıdır. Aynı key birden fazla kes tanımlanırsa, bu durumda son tanımlanan değer, o key'in değerini geçersiz kılar ve üzerine yazar. Bu davranışa ```duplicate keys  overwriting (aynı key'lerin tekrar tanımlanması ve üzerine yazılması)``` denir.

~~~~~~~
$array = [
  "name" => "Fatma",
  "surname" => "Şeren",
  "age" => 29,
  "name" => "Zehra"
];

print_r($array);
~~~~~~~
> Output: Array ( [name] => Zehra [surname] => Şeren [age] => 29 )
> + Yukarıdaki örnekte "name" iki kere tanımlanmıştır. İlk değer "Fatma", ikinci değer ise "Zehra" olarak atanmıştır.
> + PHP'de bir array'de aynı key birden fazla kez tanımlandığında, son tanımlanan değer geçerli olur ve önceki değer üzerine yazılır.

***
### 7. Having keys on only some elements & how automatic indexing works (yalnızca bazı öğelerde key olması ve otomatik index'leme nasıl çalışır)?
***
+ Array'ler, aynı anda hem ilişkisel (associative) hemde sayısal (indexed) key'ler içerebilir.
+ Bazı elemanlar belirli key'lerle tanımlanabilirken, diğer elemanlar otomatik olarak index'lenebilir. Bu durum, array elemanlarının karışık şekilde key'lerle ve otomatik index'lerle tanımlanmasını sağlar.
~~~~~~~
$array = [
    "name" => "Zehra",
    "surname" => "Şeren",
    7,
    "age" => 29,
    "city" => "İstanbul",
    19
];

print_r($array);
~~~~~~~
> Output: Array ( [name] => Zehra [surname] => Şeren [0] => 7 [age] => 29 [city] => İstanbul [1] => 19 )
> + Belirli key'lerle tanımlanan elemanlar, array içinde belirtilen key'lerle saklanır.
> + Otomatik index'lenen elemanlar, dizideki son sayısal key baz alınarak sırayla index'lenir. Yukarıdaki örnekte ```5``` elemanı otomatik olarak ```[0]``` anahtarıyla, ```10``` elemanı ise otomatik olarak ```[1]``` saklanmıştır.

##### Otomatik index'leme nasıl çalışır?
1. ```Başlangıç:``` Array'e ilk eleman eklendiğinde ve belirli bir key atanmadığında, bu elemanı ```[0]``` elemanı saklar.
2. ```Sırayla Index'leme:``` Sonraki elemanlar için, eğer belirli bir key atanmazsa, önceki en yüksek sayısal key'in bir fazlasını kullanarak otomatik index'leme yapar.
3. ```Karışık Key'ler:``` Bir array'de belirli key'lerle elemanlar tanımlandığında, otomatik index'leme bu belirli key'lerden bağımsız olarak çalışır.
~~~~~~~
$array = [];
$array[] = "apple";        // Otomatik olarak [0] key'i verilir.
$array["fruit"] = "pear";  // Belirli key: [fruit]
$array[] = "banana";       // Otomatik olarak [1] key'i verilir.
$array[5] = "strawberry";  // Belirli key: [5]
$array[] = "cherry";       // Otomatik olarak [6] key'i verilir.

print_r($array);
~~~~~~~
> Output: Array ( [0] => apple [fruit] => pear [1] => banana [5] => strawberry [6] => cherry )
> + Belirli  key'lerle ve otomatik index'leme ile karışık olarak array'leri yönetlebilir. Bu esneklik, array'lerin karmaşık veri yapılarını temsil etmesini kolaylaştırır.
