+ `nelmio_api_doc` konfigürasyonu, Symfony uygulamadaki API dokümantasyonunu tanımlamak için kullanılır.
+ Bu konfigürasyon ile `NelmioApiDocBundle` tarafından üretilen API dokümantasyonu yapılandırılabilir. 

##### 1. Dökümantasyon Bilgileri (`info`)
~~~~~~~
info:
    title: 'Awesome Composer and Symphonies App API'
    description: 'API documentation for the Awesome Composer and Symphonies application.'
    version: '1.0.0'
~~~~~~~
+ `title`: API'ın adı veya başlığı
+ `description`: API'ın kısa açıklaması
+ `version`: API'ın sürüm numarası

##### 2. Alanlar (`areas`)
~~~~~~~
areas:
    default:
        path_patterns: 
            - '^/' 
        host_patterns: []
~~~~~~~
+ `default`: Bu alandaki konfigürasyonlar, varsayılan alan için geçerlidir.
  - `path_patterns`: API rotalarını tanımlayan düzenli ifadelerdir. Bu örnekte `^/` ile başlayan tüm rotaları içerir.
  - `host_patterns`: API'ın barındırıldığı ana bilgisayar desenleridir (örneğin, `example.com`). Boş bırakıldığında tüm ana bilgisayarlar için geçerlidir.

##### 3. Rotalar (`routes`)
~~~~~~~
routes:
    path_patterns: 
        - '^/'
    exclude:
        - '^/_'
        - '^/docs'
        - '^/docs.json'
        - '^/error'
~~~~~~~
+ `path_patterns`: Dokümante edilecek API rotalarını tanımlayan düzenli ifadelerdir.
+ `exclude`: Dokümantasyonun dışında bırakılacak rotaları tanımlayan düzenli ifadelerdir. Bu örnekte `_`, `docs`, `docs.json`, `error` ile başlayan rotalar dışında kalan tüm rotaları dökümante edecektir.

> Bu yapılandırma, `NelmioApiDocBundle` tarafından taranan ve dökümanı oluşturulan rotaları ve bu rotalar için belirtilen bilgileri tanımlar. Bu şekilde, API'ın dışarı açık bir şekilde nasıl belgelendirileceği kontrol edilebilir. Gerçek rotaları ve dökümantasyon ihtiyaçlarına göre bu yapılandırmayı uygun şekilde güncellenmelidir.
