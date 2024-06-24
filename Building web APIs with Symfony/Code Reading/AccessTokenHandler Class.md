+ `AccessTokenHandler` class, Redis ile etkileşerek kullanıcılar için erişim token'ları oluşturup bu tokenları doğrulayan bir güvenlik bileşeni sağlar.
+ Bu class, bir kullanıcı için erişim token'ı oluşturmak ve bu token'ı kullanarak kullanıcı bilgilerini elde etmek için gerekli işlemleri gerçekleştirebilir.
~~~~~~~
namespace App\Security;

use Symfony\Component\Security\Http\Authenticator\Passport\Badge\UserBadge;
use Symfony\Component\Security\Core\Exception\BadCredentialsException;

class AccessTokenHandler
{
    private $redis;

    public function __construct(\Redis $redis)
    {
        $this->redis = $redis;
    }

    public function createForUser($user)
    {
        $accessToken = session_create_id();
        $this->redis->setex('sessions/' . $accessToken, 10800, $user->getUserIdentifier());
        return $accessToken;
    }

    public function getUserBadgeFrom($accessToken)
    {
        $userId = $this->redis->get('sessions/' . $accessToken);
        if (!$userId) {
            throw new BadCredentialsException('Invalid token');
        }
        return new UserBadge($userId);
    }
}
~~~~~~~

##### 1. Namespace ve Imports
+ `namespace App\Security;`: Bu class'ın `App\Security` namespace'i altında bulunduğunu belirtir.
+ `use` ifadeleri ile Symfony bileşenleri dahil edilmiştir: `UserBadge` ve `BadCredentialsException`.

##### 2. Redis Bağlantısı | AccessTokenHandler Class
+ Bu class, Redis kullanarak erişim token'ları oluşturur ve doğrular.

##### 3. Özellikler
+ `$redis`: Redis bağlantısını tutar. Redis, hızlı ve verimli bir şekilde veriyi saklamak için kullanılır.

##### 4. __construct Method
+ `public function __construct(\Redis $redis)`: Bu method, class'ın başlatıcı method'udur ve bir Redis bağlantısı alır. Bu bağlantı class'ın `$redis` özelliğine atanır.

##### 5. Token Oluşturma | createForUser Method
+ `public function createForUser($user)`: Bu method, belirtilen kullanıcı için bir erişim token'ı oluşturur.
+ `session_create_id()`: PHP'nin yerleşik fonksiyonunu kullanarak benzersiz bir oturum ID'si (`token`) oluşturur.
+ `$this->redis->setex('sessions/' . $accessToken, 10800, $user->getUserIdentifier());`: Bu satır, Redis'e `sessions/<accessToken>` key ile kullanıcı ID'sini 10800 saniye (3 saat) boyunca saklar.
  - `sessions/` ön eki, key'lerin daha kolay yönetilmesi ve kategorize edilmesi için kullanılmıştır.
  - `setex`, Redis'in bir komutudur ve `SET with EXpiration` anlamına gelir. Bu komut, bir key-value çifti oluşturur ve belirli bir süre sonra (belirtilen sürenin bitiminde) bu key otomatik olarak silinir.
  - `getUserIdentifier()` method'u çağrılarak elde edilen değer saklanır.
+ `return $accessToken;`: Oluşturulan erişim token'ını döndürür.

##### 6. Token Doğrulama | getUserBadgeFrom Method
+ `public function getUserBadgeFrom($accessToken)`: Bu method, verilen erişim token'ından bir `UserBadge` oluşturur.
+ `$userId = $this->redis->get('sessions/' . $accessToken);`: Redis'ten `sessions/<accessToken>` key ile kullanıcı ID'sini alır.
+ `if (!$userId) { throw new BadCredentialsException('Geçersiz token'); }`: Eğer token geçersizse veya bulunamazsa, `BadCredentialsException` fırlatır.
+ `return new UserBadge($userId);`: Geçerli bir kullanıcı ID'si ile yeni bir `UserBadge` nesnesi döndürür.

###### Özet
+ Bu class', bir kullanıcı için erişim token'ları oluşturmak ve bu token'ları doğrulamak için Redis kullanır. Bu token'lar, kullanıcı oturumlarını izlemek ve doğrulamak için kullanılır. `createForUser` method'u, yeni bir erişim token'ı oluşturur ve bunu Redis'e kaydederken, `getUserBadgeFrom` method'u, verilen token'ı doğrular ve geçerli bir kullanıcı ID'si ile bir `UserBadge` döndürür. Bu yapı, kullanıcıların oturumlarını güvenli ve verimli bir şekilde yönetmek için kullanılabilir.
