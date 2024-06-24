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

+ `setUp` method'unda, test kullanıcı oluşturulup giriş yapılarak erişim token'ı alınır. Bu token, class'ın tüm test method'larında kullanılabilir.
+ `testSomeEndpoint`, `testInvalidToken` ve `testSomeOtherEndpoint` method'ları, bu yardımcı method'lar kullanılarak HTTP isteklerini yapabilir.
+ `post`, `get`, `put` ve `delete` method'ları, HTTP isteklerini kolayca yapmayı sağlar ve gereken durumlarda yetkilendirme başlıklarını ekler.

> Bu düzenleme, test kodlarını daha temiz ve anlaşılır hale getirir ve kod tekrarını önler.
