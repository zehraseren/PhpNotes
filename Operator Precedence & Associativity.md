***
### Operator Precedence & Associativity (operatör önceliği ve ilişkilendirme) nedir?
***
+ Operatörlerin bir ifade içinde nasıl değerlendirileceğini belirleyen önemli kavramlardır.

#### Operatör Önceliği (Operator Precedence)
----
+ Bir ifade içinde hangi operatörlerin önce değerlendirileceğini belirler.
+ Öncelik, matematiksel operatörlerin öncelik sırasını takip eder.
+ Yüksek önceliğe sahip operatörler, düşük önceliğe sahip operatörlerden önce değerlendirilir.
> Çarpma `*` ve bölme `/` operatörleri, toplama `+` ve çıkarma `-` operatörlerinden daha yüksek önceliğe sahiptir. Bu nedenle, bir ifadede önce çarpma ve bölme işlemleri gerçekleştirilir, ardından toplama ve çıkarma işlemleri yapılır.

#### Operatör İlişkilendirme (Associativity)
+ Operatör ilişkilendirme, aynı önceliğe sahip operatörlerin bir ifade içinde nasıl değerlendirileceğini belirler.
+ İki tür ilişkilendirme vardır:
    - `Sol ilişkilendirme (left associativity):` Operatörlerin soldan sağa doğru değerlendirileceği anlamına gelir.
    > Toplama ve çıkarma operatörleri sol ilişkilidir. Yani, a + b + c ifadesinde işlem, soldan sağa doğru yapılır.
    - `Sağ ilişkilendirme (right associativity):` Operatörlerin sağdan sola doğru değerlendirileceği anlamına gelir.
    > Üs alma operatörü (**) sağ ilişkilidir. Yani, a ** b ** c ifadesinde işlem, sağdan sola doğru yapılır.
