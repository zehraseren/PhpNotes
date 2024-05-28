***
### Match Expression nedir?
***
+ `switch` ifadesine benzer bir yapıya sahip olan, ancak daha güçlü ve daha esnek olan bir kontrol yapısıdır.
+ `match` ifadesi, belirli bir değere göre bir dizi olası sonucu değerlendirir ve karşılık gelen değeri döndürür.
+ `match` ifadesi, switch ifadesine göre daha kısa ve okunabilir bir alternatif sunar.

~~~~~~~
$day = 'Wednesday';

$result = match ($day) {
    'Monday' => 'Today is Monday.',
    'Tuesday' => 'Today is Tuesday.',
    'Wednesday' => 'Today is Wednesday.',
    'Thursday' => 'Today is Thursday.',
    'Friday' => 'Today is Friday.',
    'Saturday' => 'Today is Saturday.',
    'Pazar' => 'Today is Pazar.',
    default => 'Invalid day.',
};

echo $result;
~~~~~~~
> Output: Today is Wednesday.

~~~~~~~
$score = 85;

$result = match (true) {
    $score >= 90 => 'A',
    $score >= 80 => 'B',
    $score >= 70 => 'C',
    $score >= 60 => 'D',
    default => 'F',
};

echo $result;
~~~~~~~
> Output: B
> + `$score` değişkeninin değeri 85 olduğu için match ifadesi B değerini döndürür. Burada match (true) kullanarak, koşulları değerlendiren ifadeler yazılmıştır.

##### Match expression avantajları
+ `Daha Kısa ve Okunabilir Kod:` switch ifadesine göre daha kısa ve okunabilir bir yapı sağlar.
+ `Tür Güvenliği:` Katı eşitlik `===` kullanarak tür uyumsuzluklarını önler.
+ `Tek Satırda Değerlendirme:` Her zaman tek bir değer döndürdüğü için daha sade ve anlaşılır bir yapı sunar.

#### match vs switch
+ `Eşitlik Kontrolü:` Match katı eşitlik `===` kullanırken, switch gevşek eşitlik `==` kullanır.
+ `Kod Okunabilirliği:` Match ifadesi genellikle daha kısa ve okunabilir kod sağlar.
+ `Değer Döndürme:` Match ifadesi her zaman bir değer döndürür, switch ifadesi ise doğrudan kod bloğunu çalıştırır.
+ `Fall-Through Durumu:` Switch ifadesinde fall-through durumu olabilir, match ifadesinde böyle bir durum yoktur.

> match ifadesi, switch ifadesine göre modern ve daha esnek bir alternatiftir. Özellikle daha kısa ve okunabilir kod yazmak istediğinizde veya tür güvenliği gerektiğinde match ifadesi kullanımı tercih edilir.
