***
### Float tanımlama yöntemleri nelerdir?
***
+ Float (ondalık sayı) değerler birkaç farklı şekilde tanımlanabilir.

1. Normal Ondalık Sayılar
  - Ondalık sayılar normal bir şekilde atanabilir.
~~~~~~~
$float1 = 3.14;
~~~~~~~
> Output: 3.14
> + Pi sayısı gibi

~~~~~~~
$float2 = -123.456;
~~~~~~~
> Output: -123.456
> +  Negatif bir ondalık sayı

2. Ondalık İfadesi (Floating-Point Literal)
  - Bilimsel bir ifade olarak da tanımlanabilir.
~~~~~~~
$float3 = 6.02e23;
~~~~~~~
> Output: 6.02 * 10<sup>23</sup>
> + Avogadro sabiti gibi

3. Sabitler
  - Bazı önceden tanımlanmış sabitler aracılığıyla float değerler için özel değerler sağlanır.
~~~~~~~
$infinity = INF;
~~~~~~~
> Output: INF
> + Sonsuz değeri

~~~~~~~
$not_a_number = NAN;
~~~~~~~
> Output:
> + Belirsiz (NaN) değeri

4. Tür Dönüşümü (Type Casting)
  - Diğer veri türlerini float'a dönüştürebilir.
~~~~~~~
$float4 = (float) "3.14";
~~~~~~~
> Output: 3.14
> + String "3.14" değerini float'a dönüştürür.

~~~~~~~
$float5 = (float) 123;
~~~~~~~
> Output: 123
> + Integer 123 değerini float'a dönüştürür.
