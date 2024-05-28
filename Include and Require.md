***
### Include and Require nedir?
***
+ Başka bir dosyanın içeriğini mevcut betiğe dahil etmek için kullanılır.
+ `Include` ve `require` ifade benzer işlevlere sahiptir, ancak önemli bir farkları vardır. Her ikisinde de kodun yeniden kullanılabilirliğini ve modülerliğini artırmak için yaygın olarak kullanılır.

#### Include İfadesi
+ Belirtilen dosyanın içeriğini mevcut betiğe dahil eder.
+ Eğer dosya bulunamazsa, include `bir uyarı (warning) üretir ve betiğin yürütülmesine devam eder.`
+ Daha esnek bir yapıya sahiptir ve genellikle dosya eksikliğinin kritik olmadığı durumlarda kullanılır.

1. main.php
~~~~~~~
echo "This is master file.\n";
include "extra.php";
echo "End of this master file.\n";
~~~~~~~

2. extra.php
~~~~~~~
echo "This is extra file.\n";
~~~~~~~

> `main.php` çalıştırıldığında ekran:
~~~~~~~
This is master file.
This is extra file.
End of this master file.
~~~~~~~

#### Require İfadesi
+ Belirtilen dosyanın içeriğini mevcut betiğe dahil eder.
+ Ancak dosya bulunamazsa, require `bir ölümcül hata (fatal error) üretir ve betiğin yürütülmesini durdurur.`
+ Dosya eksikliğinin kritik olduğu durumlarda kullanılır.

1. main.php
~~~~~~~
echo "This is master file.\n";
require "extra.php";
echo "End of this master file.\n";
~~~~~~~

2. extra.php
~~~~~~~
echo "This is extra file.\n";
~~~~~~~

> `main.php` çalıştırıldığında ekran:
~~~~~~~
This is master file.
This is extra file.
End of this master file.
~~~~~~~
> + ` ekstra.php` dosyası bulunamazsa, require ifadesi fatal error üretecek ve betik duracaktır.

##### include_once ve require_once
+ Belirli bir dosyanın sadece bir kez dahil edilmesini sağlayan `include_once` ve `require_once` ifadelerini de sunar. Bu ifadeler, aynı dosyanın birden fazla kez dahil edilmesini önler.
  
  - `include_once:` Belirtilen dosyayı yalnızca bir kez dahil eder. Dosya daha önce dahil edilmişse, ikinci kez dahil edilmez ve bir `warning` verir. 
  - `require_once:` Belirtilen dosyayı yalnızca bir kez dahil eder. Dosya daha önce dahil edilmişse, ikinci kez dahil edilmez ve bir `fatal error` verir.

##### include vs require
1. Hata Durumu
   - `include:` Dosya bulunamazsa `warning` verir ve betik devam eder.
   - `require:` Dosya bulunamazsa `fatal error` verir ve betik durur.
     
2. Kullanım
   - `include:` Modüler kod yapısına izin verir ve betiğin yürütülmesine devam eder.
   - `require:` Hayati öneme sahip dosyalar için kullanılır ve betiğin durmasını sağlar.
     
3. Çalışma Zamanı
   - `include:` Çalışma zamanında dosya dahil edilir.
   - `require:` Çalışma zamanında dosya dahil edilir.

4. Tekrar Dahil Etme
   - `include:` include_once kullanılarak önlenir.
   - `require:` require_once kullanılarak önlenir.

