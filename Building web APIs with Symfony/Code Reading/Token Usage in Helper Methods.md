+ HTTP isteklerini yaparken kod tekrarını önlemek ve istekleri daha okunabilir hale getirmek amacıyla oluşturulmuş yardımcı method'lardır.
+ Bu method'ları Symfony'nin `WebTestCase` class'ına dahil ederek, testlerde HTTP isteklerini daha kolay bir şekilde yapılabilir.
~~~~~~~
protected function post(string $uri, array $data = [], ?string $token = null)
{
    $headers = ['CONTENT_TYPE' => 'application/json'];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('POST', $uri, [], [], $headers, json_encode($data));
}

protected function get(string $uri, ?string $token = null)
{
    $headers = [];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('GET', $uri, [], [], $headers);
}

protected function put(string $uri, array $data = [], ?string $token = null)
{
    $headers = ['CONTENT_TYPE' => 'application/json'];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('PUT', $uri, [], [], $headers, json_encode($data));
}

protected function delete(string $uri, ?string $token = null)
{
    $headers = [];
    if ($token) {
        $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
    }

    return $this->client->request('DELETE', $uri, [], [], $headers);
}
~~~~~~~

#### POST
+ Bu method, JSON formatında veri gönderir ve isteği yetkilendirmek için bir token kullanabilir.

##### Method'un Parametreleri
+ `$uri:` İsteğin yapılacağı URL.
+ `$data:` Gönderilecek veriler. Bu veriler JSON formatına dönüştürülecektir.
+ `$token:` (Opsiyonel) Yetkilendirme token'ı. Eğer sağlanırsa, isteğin `Authorization` başlığında gönderilecektir.

##### Method'un İçeriği
###### 1. Başlıkları Ayarlama
~~~~~~~
$headers = ['CONTENT_TYPE' => 'application/json'];
if ($token) {
    $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
}
~~~~~~~
+ `CONTENT_TYPE` başlığı, isteğin JSON formatında olduğunu belirtir.
+ Eğer bir token sağlanmışsa, `Authorization` başlığı eklenir ve token bu başlığa dahil edilir.

###### 2. POST İsteği Yapma
~~~~~~~
return $this->client->request('POST', $uri, [], [], $headers, json_encode($data));
~~~~~~~
+ `POST` HTTP method'u kullanılarak istek yapılır.
+ İstek yapılacak URL, `POST` method'unun ikinci parametresi olarak belirtilir.
+ `[]` ve `[]` parametreleri, sırasıyla dosya ekleme ve ek istek seçeneklerini belirtir; bu örnekte bunlar boş bırakılmıştır.
+ `json_encode($data)`, gönderilecek verileri JSON formatına dönüştürür.

#### GET
+ Bu method, isteği yetkilendirmek için bir token kullanabilir.

##### Method'un Parametreleri
+ `$uri:` İsteğin yapılacağı URL.
+ `$token:` (Opsiyonel) Yetkilendirme token'ı. Eğer sağlanırsa, isteğin `Authorization` başlığında gönderilecektir.

##### Method'un İçeriği
###### 1. Başlıkları Ayarlama
~~~~~~~
$headers = [];
if ($token) {
    $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
}
~~~~~~~
+ Eğer bir token sağlanmışsa, `Authorization` başlığı eklenir ve token bu başlığa dahil edilir.

###### 2. GET İsteği Yapma
~~~~~~~
return $this->client->request('GET', $uri, [], [], $headers);
~~~~~~~
+ `GET` HTTP method'u kullanılarak istek yapılır.
+ İstek yapılacak URL, `GET` method'unun ikinci parametresi olarak belirtilir.
+ `[]` ve `[]` parametreleri, sırasıyla dosya ekleme ve ek istek seçeneklerini belirtir; bu örnekte bunlar boş bırakılmıştır.

#### PUT
+ Bu method, isteği yetkilendirmek için bir token kullanabilir.

##### Method'un Parametreleri
+ `$uri:` İsteğin yapılacağı URL.
+ `$data:` Gönderilecek veriler. Bu veriler JSON formatına dönüştürülecektir.
+ `$token:` (Opsiyonel) Yetkilendirme token'ı. Eğer sağlanırsa, isteğin `Authorization` başlığında gönderilecektir.

##### Method'un İçeriği
###### 1. Başlıkları Ayarlama
~~~~~~~
$headers = ['CONTENT_TYPE' => 'application/json'];
if ($token) {
    $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
}
~~~~~~~
+ `CONTENT_TYPE` başlığı, isteğin JSON formatında olduğunu belirtir.
+ Eğer bir token sağlanmışsa, `Authorization` başlığı eklenir ve token bu başlığa dahil edilir.

###### 2. PUT İsteği Yapma
~~~~~~~
return $this->client->request('PUT', $uri, [], [], $headers, json_encode($data));
~~~~~~~
+ `PUT` HTTP method'u kullanılarak istek yapılır.
+ İstek yapılacak URL, `PUT` method'unun ikinci parametresi olarak belirtilir.
+ Gönderilecek veri (`$data`) JSON formatına dönüştürülerek isteğin gövdesine (`body`) eklenir.
+ `[]` ve `[]` parametreleri, sırasıyla dosya ekleme ve ek istek seçeneklerini belirtir; bu örnekte bunlar boş bırakılmıştır.
+ `json_encode($data)`, gönderilecek verileri JSON formatına dönüştürür.

#### DELETE
+ Bu method, isteği yetkilendirmek için bir token kullanabilir.

##### Method'un Parametreleri
+ `$uri:` İsteğin yapılacağı URL.
+ `$token:` (Opsiyonel) Yetkilendirme token'ı. Eğer sağlanırsa, isteğin `Authorization` başlığında gönderilecektir.

##### Method'un İçeriği
###### 1. Başlıkları Ayarlama
~~~~~~~
$headers = [];
if ($token) {
    $headers['HTTP_AUTHORIZATION'] = 'Bearer ' . $token;
}
~~~~~~~
+ Eğer bir token sağlanmışsa, `Authorization` başlığı eklenir ve token bu başlığa dahil edilir.

###### 2. DELETE İsteği Yapma
~~~~~~~
return $this->client->request('DELETE', $uri, [], [], $headers);
~~~~~~~
+ `DELETE` HTTP method'u kullanılarak istek yapılır.
+ İstek yapılacak URL, `DELETE` method'unun ikinci parametresi olarak belirtilir.
+ `[]` ve `[]` parametreleri, sırasıyla dosya ekleme ve ek istek seçeneklerini belirtir; bu örnekte bunlar boş bırakılmıştır.
