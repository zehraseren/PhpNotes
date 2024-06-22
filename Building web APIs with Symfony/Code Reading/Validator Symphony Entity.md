+ `Symfony\Component\Validator\Constraints` class'ını kullanarak bir Symphony class'ındaki özelliklere çeşitli doğrulama kuralları ekler. Bu kurallar, Symphony nesneleri oluşturulurken veya güncellenirken geçerli ve uygun veri sağlamak için kullanılır.

##### 1. `$name`
+ `#[Assert\NotBlank]:` Bu kural, `name` (isim) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
~~~~~~~
#[Assert\NotBlank]
private $name;
~~~~~~~

##### 2. `$finishedAt`
+ `#[Assert\NotBlank]:` Bu kural, `finishedAt` (bitiş tarihi) alanının boş olmaması gerektiğini belirtir. Bu alanın boş bırakılmaması zorunludur.
+ `#[Assert\DateTime(format: 'Y-m-d')]:` Bu kural, ilgili alanın belirli bir tarih formatında (`Y-m-d`) olması gerektiğini belirtir, aksi takdirde doğrulama hatası dönecektir.
~~~~~~~
#[Assert\NotBlank]
#[Assert\DateTime(format: 'Y-m-d')]
private $finishedAt;
~~~~~~~

##### 3. `$createdAt`
+ Bu özellik, `Symphony` oluşturulma tarihini temsil eder. Boş olabilir çünkü `NotBlank` kuralı eklenmemiştir.
+  `#[Assert\DateTime(format: 'Y-m-d')]:` Bu kural, ilgili alanın belirli bir tarih formatında (`Y-m-d`) olması gerektiğini belirtir, aksi takdirde doğrulama hatası dönecektir.
~~~~~~~
#[Assert\DateTime(format: 'Y-m-d')]
private $createdAt;
~~~~~~~
