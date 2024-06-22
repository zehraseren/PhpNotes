+ Bu class, bir `Composer` varlığının nasıl doğrulanacağını tanımlar ve her bir özelliğe doğrulama kuralları ekler.
+ `Symfony\Component\Validator\Constraints` kütüphanesinden alınan bu doğrulama kuralları, verilerin doğru formatta ve gerekli olduğu durumlarda boş olmadığından emin olunmasını sağlar.

##### 1. `$firstName`
+ `#[Assert\NotBlank]:` Bu kural, `firstName` (isim) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
~~~~~~~
#[Assert\NotBlank]
$firstName;
~~~~~~~

##### 2. `$lastName`
+ `#[Assert\NotBlank]:` Bu kural, `lastName` (soyisim) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
~~~~~~~
#[Assert\NotBlank]
$lastName;
~~~~~~~

##### 3. `$dateOfBirth`
+ #[Assert\NotBlank]: Bu kural, `dateOfBirth` (doğum günü) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
~~~~~~~
#[Assert\NotBlank]
$dateOfBirth;
~~~~~~~

##### 4. `$countryCode`
+ `#[Assert\NotBlank]:` Bu kural, `countryCode` (ülke kodu) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
+ `#[Assert\Country]:` Bu kural, `countryCode` alanının geçerli bir ülke kodu olması gerektiğini belirtir. Bu doğrulama kuralı, iki harfli ISO 3166-1 alfa-2 ülke kodlarını kullanır (örneğin, `US` Amerika Birleşik Devletleri için).
~~~~~~~
#[Assert\NotBlank]
#[Assert\Country]
$countryCode;
~~~~~~~
