***
### Integer tanımlama yöntemleri nelerdir?
***
+ Integer (tam sayı) değerler birkaç farklı şekilde tanımlanabilir.

1. Onluk (Decimal) Sayılar
  - Onluk (decimal) sistemdeki sayılar doğrudan atanabilir.
~~~~~~~
$integer1 = 123;
~~~~~~~
> Output: 123 // Onluk sistemde 123

~~~~~~~
$integer2 = -456;
~~~~~~~
> Output: -456 // Negatif onluk sistemde 456
 
2. Sekizlik (Octal) Sayılar
  - Sekizlik (octal) sistemdeki sayılar, başlarına ```0``` eklenerek tanımlanabilir.
~~~~~~~
$integer3 = 0123;
~~~~~~~
> Output: Sekizlik sistemde 83
> + (0 * 8<sup>4</sup> + 1 * 8<sup>2</sup> + 2 * 8<sup>1</sup> + 3 * 8<sup>0</sup>) -> 0 + 64 + 8 + 3 = 83
 
3. Onaltılık (Hexadecimal) Sayılar
  - Onaltılık (hexadecimal) sistemdeki sayıların, başlarına ```0x``` veya ```0y``` eklenerek tanımlanabilir.
~~~~~~~
#integer4 = 0xFF;
~~~~~~~
> Output: Onaltılık sistemde 255
> + (15 * 16<sup>1</sup> + 15 * 16<sup>0</sup>) -> 240 + 15 = 255
> + ```Onaltılık sistemde kullanılan rakamlar 0-9 ve A-F'dir. A, B, C, D, E ve F sırasıyla 10, 11, 12, 13, 14 ve 15 değerlerine karşılık gelir.```
 
4. İkili (Binary) Sayılar
  - İkili (binary) sistemdeki sayılar, başlarına ```0b``` veya ```0B``` eklenerek tanımlanabilir.
~~~~~~~
$integer5 = 0b1010;
~~~~~~~
> Output: İkili sistemde 10
> + (1 * 2<sup>3</sup> + 0 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 0 * 8<sup>0</sup>) -> 8 + 0 + 2 + 0 = 10
 
5. Sabitler
  - Bazı durumlarda, önceden tanımlanmış sabitler kullanılabilir, örneğin ```PHP_INT_MAX``` ve ```PHP_INT_MIN```, platformdaki en büyük ve en küçük integer değerlerini temsil eder.
~~~~~~~
$maximum_integer = PHP_INT_MAX;
~~~~~~~
> Output: 9223372036854775807
> + 32-bit sistemlerde genellikle bu değer ```2147483647``` (2<sup>31</sup> - 1) olur.
> + 64-bit sistemlerde ise bu değer ```9223372036854775807``` (2<sup>63</sup> - 1) olur.

~~~~~~~
$minimum_integer = PHP_INT_MIN;
~~~~~~~
> Output: -9223372036854775807> 
> + 32-bit sistemlerde genellikle bu değer ```-2147483647``` (2<sup>31</sup> - 1) olur.
> + 64-bit sistemlerde ise bu değer ```-9223372036854775807``` (2<sup>63</sup> - 1) olur.
 
6. Onluk (Decimal) Sayılar
  - Diğer veri türlerini integer'a dönüştürebilir. Örneğin, bir string veya float değeri integer'a dönüştürülebilir.
~~~~~~~
$integer6 = (int) "123";
~~~~~~~
> Output: 123
> + String "123" değerini integer'a dönüştürür.
 
~~~~~~~
$integer7 = (int) 3.14; 
~~~~~~~
> Output: 3
> + Float 3.14 değerini integer'a dönüştürür.
