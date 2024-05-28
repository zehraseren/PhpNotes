***
### Switch statement nedir?
***
+ Bir değişkenin farklı değerlere sahip olmasına bağlı olarak çeşitli kod bloklarının yürütülmesini sağlar.  Bu ifade, birden çok koşulu kontrol etmek için if-elseif-else yapısına alternatif olarak kullanılır.
+ `switch` ifadesi, özellikle bir değişkenin birçok olası değeri olduğu durumlarda kodun okunabilirliğini artırır.

~~~~~~~
$day = "Wednesday";

switch ($day) {
    case "Monday":
        echo "Today is Monday.";
        break;
    case "Tuesday":
        echo "Today is Tuesday.";
        break;
    case "Wednesday":
        echo "Today is Wednesday.";
        break;
    case "Thursday":
        echo "Today is Thursday.";
        break;
    case "Friday":
        echo "Today is Friday.";
        break;
    case "Saturday":
        echo "Today is Saturday.";
        break;
    case "Sunday":
        echo "Today is Sunday.";
        break;
    default:
        echo "Invalid day.";
}
~~~~~~~
> Output: Today is Wednesday.

##### switch statament nasıl çalışır?
+ `switch` ifadesi, belirtilen değişkenin değerine göre değerlendirme yapar.
+ Her `case` ifadesi, değişkenin belirli bir değere sahip olup olmadığını kontrol eder.
+ Değişkenin değeri `case` ifadesindeki değere eşitse, o `case` bloğu çalışır ve `break` ifadesine ulaşana kadar devam eder.
+ `break` ifadesi, `switch` yapısını sonlandırır. Eğer `break` ifadesi kullanılmazsa, kod bir sonraki case bloğuna geçer (bu duruma `fall-through` denir).
+ `default`  bloğu, değişkenin hiçbir `case` değeriyle eşleşmemesi durumunda çalışır. `default` bloğu isteğe bağlıdır ancak hiçbir `case` değeriyle eşleşmeyen durumlar için bir güvenlik ağı sağlar ve genellikle kullanılması önerilir.
