#### Route Annotation
~~~~~~~
/**
 * @Route("/symphonies/{id}", name="symphony_delete", methods={"DELETE"})
 */
~~~~~~~
> + `@Route("/symphonies/{id}")`: Bu rota, `/symphonies/{id}` URL'i için geçerlidir.
> + `name="symphony_delete"`: Bu rota için bir isimdir.
> + `methods={"DELETE"}`: Bu rota sadece DELETE isteklerini kabul eder.

#### Method Defination | Method Tanımlama
##### 1. Mevcut Symphony Nesnesini Bulma
~~~~~~~
$symphony = $this->symphonyRepository->find($id);

if (!$symphony) {
    return new JsonResponse("Symphony not found", JsonResponse::HTTP_NOT_FOUND);
}
~~~~~~~
> + `$this->symphonyRepository->find($id)`: Veritabanında belirtilen ID'ye sahip `Symphony` nesnesini arar. Eğer bulunamazsa, HTTP 404 hata mesajı ile yanıt döner.

##### 2. Veritabanından Silme
~~~~~~~
$this->symphonyRepository->delete($symphony);
~~~~~~~
> + `$this->symphonyRepository->delete()`: Veritabanından belirtilen `Symphony` nesnesini siler.
>   - `$this->symphonyRepository`: Bu repository, `Symphony` nesneleriyle ilgili veritabanı işlemlerini gerçekleştirir.

##### 3. JsonResponse Döndürme
~~~~~~~
return new JsonResponse(null, JsonResponse::HTTP_NO_CONTENT);
~~~~~~~
> + `new JsonResponse(null, JsonResponse::HTTP_NO_CONTENT)`: Silme işlemi başarılı olduğunda HTTP 204 durumu (No Content) ile yanıt döner. İçeriği olmayan bir JSON yanıtı döner.
