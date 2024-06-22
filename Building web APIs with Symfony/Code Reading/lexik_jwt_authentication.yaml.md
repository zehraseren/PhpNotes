+ Bu Symfony güvenlik konfigürasyonu, modern bir API tabanlı uygulama için `JWT (JSON Web Token)` tabanlı kimlik doğrulama ve yetkilendirme sağlar. 

#### Encoders
~~~~~~~
encoders:
    App\Entity\User:
        algorithm: auto
~~~~~~~
+ Bu bölümde, `App\Entity\User` class'ı için hangi şifreleme algoritmasının kullanılacağı belirtilir. `algorithm: auto` değeri, `User` entity class'ında tanımlanan şifreleme algoritmasını otomatik olarak seçmesini sağlar.

#### Providers
~~~~~~~
providers:
    users_in_database:
        entity:
            class: App\Entity\User
            property: username
~~~~~~~
+ `users_in_database` provider'ı, kullanıcıların database'den alınacağını belirtir.
+ `entity` seçeneğiyle birlikte, kullanıcıların hangi class'tan (`App\Entity\User`) ve hangi özellikle (`username`) tanımlandığı belirtilir. Burada `username` özelliği, kullanıcıların benzersiz kimliklerini temsil eder.

#### Firewalls
##### Login Firewall
~~~~~~~
firewalls:
    login:
        pattern:  ^/api/login
        stateless: true
        anonymous: true
        json_login:
            check_path:               /api/login_check
            username_path:            username
            password_path:            password
            success_handler:          lexik_jwt_authentication.handler.authentication_success
            failure_handler:          lexik_jwt_authentication.handler.authentication_failure
~~~~~~~
+ `login` firewall'i, kullanıcıların kimlik doğrulaması için kullanılır.
+ `pattern` ile hangi URL desenlerinin bu firewall altında çalışacağı belirtilir (`^/api/login`).
+ `stateless: true` ile her isteğin ayrı ayrı kimlik doğrulaması yapılacağı belirtilir, oturum bilgisi (session) kullanılmaz.
+ `anonymous: true` ile kimlik doğrulaması yapmadan da erişime izin verilir.
+ `json_login` ile kullanıcıların JSON formatında giriş yapabileceği belirtilir.
  - `check_path` ile kullanıcı giriş isteğinin işleneceği URL (`/api/login_check`) belirtilir.
  - `username_path` ve `password_path` ile giriş isteğinde kullanılacak kullanıcı adı ve şifre parametrelerinin adları belirtilir.
  - `success_handler` ve `failure_handler` ile başarılı ve başarısız kimlik doğrulaması durumlarında hangi işlevlerin çalıştırılacağı belirtilir.

##### API Firewall
~~~~~~~
    api:
        pattern:   ^/api
        stateless: true
        guard:
            authenticators:
                - lexik_jwt_authentication.jwt_token_authenticator
~~~~~~~
+ `api` firewall'i, API'nin geri kalan bölümü için kullanılır.
+ `pattern` ile API isteklerinin hangi URL deseni altında işleneceği belirtilir (`^/api`).
+ `stateless: true` ile her isteğin ayrı ayrı kimlik doğrulaması yapılacağı belirtilir.
+ `guard` ile Symfony Guard bileşeni kullanılarak kimlik doğrulama ve yetkilendirme sağlanır.
+ `authenticators` ile kullanılacak kimlik doğrulayıcılar (`authenticators`) belirtilir. Burada sadece JWT token doğrulayıcı (`lexik_jwt_authentication.jwt_token_authenticator`) kullanılır.

#### Access Control
~~~~~~~
access_control:
    - { path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
    - { path: ^/api, roles: IS_AUTHENTICATED_FULLY }
~~~~~~~
+ `access_control` bölümü, belirli URL desenlerine ve rollerine göre erişim kontrolü sağlar.
+ `{ path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }` ile `/api/login` yoluna kimlik doğrulaması yapılmadan erişim izni verilir (`IS_AUTHENTICATED_ANONYMOUSLY` rolü).
+ `{ path: ^/api, roles: IS_AUTHENTICATED_FULLY }` ile `/api` altındaki tüm yollara sadece tam kimlik doğrulaması yapılmış kullanıcılar erişebilir (`IS_AUTHENTICATED_FULLY` rolü).

###### Özet
+ Bu konfigürasyon, Symfony ile geliştirilen bir API uygulamasında JWT tabanlı kimlik doğrulama ve yetkilendirme sağlar. Kullanıcıların veritabanından alınması ve giriş yapılması, API isteklerinin korunması ve erişim kontrolü gibi işlevleri yönetir. Bu yapı, modern ve güvenli bir API mimarisi için uygun bir temel oluşturur.
