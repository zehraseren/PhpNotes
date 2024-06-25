+ Symfony uygulamasında `IsGranted` özniteliğini kullanarak `SymphonyController` için erişim kontrolü eklemek, belirli rollere sahip kullanıcıların belirli işlemleri gerçekleştirebilmesini sağlar.
+ Aşağıda, `SymphonyController` class'ına eklenecek kodun tamamlanmış ve düzenlenmiş hali verilmiştir:
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Http\Attribute\IsGranted;
use App\Entity\Symphony;
use App\Repository\SymphonyRepository;
use Doctrine\ORM\EntityManagerInterface;

class SymphonyController extends AbstractController
{
    private $entityManager;
    private $symphonyRepository;

    public function __construct(EntityManagerInterface $entityManager, SymphonyRepository $symphonyRepository)
    {
        $this->entityManager = $entityManager;
        $this->symphonyRepository = $symphonyRepository;
    }

    #[Route('/symphonies', name: 'symphony_index', methods: ['GET'])]
    #[IsGranted('ROLE_USER')]
    public function index(): Response
    {
        $symphonies = $this->symphonyRepository->findAll();

        return $this->json($symphonies);
    }

    #[Route('/symphonies/{id}', name: 'symphony_show', methods: ['GET'])]
    #[IsGranted('ROLE_USER')]
    public function show(int $id): Response
    {
        $symphony = $this->symphonyRepository->find($id);

        if (!$symphony) {
            throw $this->createNotFoundException('Symphony not found');
        }

        return $this->json($symphony);
    }

    #[Route('/symphonies', name: 'symphony_create', methods: ['POST'])]
    #[IsGranted('ROLE_ADMIN')]
    public function create(Request $request): Response
    {
        $data = json_decode($request->getContent(), true);

        $symphony = new Symphony();
        $symphony->setName($data['name']);
        $symphony->setComposer($data['composer']);
        $symphony->setYear($data['year']);

        $this->entityManager->persist($symphony);
        $this->entityManager->flush();

        return $this->json($symphony, Response::HTTP_CREATED);
    }

    #[Route('/symphonies/{id}', name: 'symphony_update', methods: ['PUT'])]
    #[IsGranted('ROLE_ADMIN')]
    public function update(int $id, Request $request): Response
    {
        $data = json_decode($request->getContent(), true);

        $symphony = $this->symphonyRepository->find($id);

        if (!$symphony) {
            throw $this->createNotFoundException('Symphony not found');
        }

        $symphony->setName($data['name']);
        $symphony->setComposer($data['composer']);
        $symphony->setYear($data['year']);

        $this->entityManager->flush();

        return $this->json($symphony);
    }

    #[Route('/symphonies/{id}', name: 'symphony_delete', methods: ['DELETE'])]
    #[IsGranted('ROLE_ADMIN')]
    public function delete(int $id): Response
    {
        $symphony = $this->symphonyRepository->find($id);

        if (!$symphony) {
            throw $this->createNotFoundException('Symphony not found');
        }

        $this->entityManager->remove($symphony);
        $this->entityManager->flush();

        return $this->json(null, Response::HTTP_NO_CONTENT);
    }
}
~~~~~~~

##### 1. index ve show Method'ları
+ Bu method'lar `ROLE_USER` rolüne sahip kullanıcılar tarafından erişilebilir.
  - `index` method'u tüm symphony'leri döner.
  - `show` method'u belirli bir symphony'yi döner.

##### 2. create, update ve delete Method'ları
+ Bu method'lar `ROLE_ADMIN` rolüne sahip kullanıcılar tarafından erişilebilir.
  - `create` method'u yeni bir symphony oluşturur.
  - `update` method'u mevcut bir symphony'yi günceller.
  - `delete` method'u mevcut bir symphony'yi siler.

#### Test Method'ları
+ Test method'larında bu erişim kontrollerini doğrulamak için uygun token'lar ile istekler gönderilmelidir. Örneğin:
~~~~~~~
public function testCreateSymphony()
{
    $response = $this->post('/symphonies', [
        'name' => 'Test Symphony',
        'composer' => 'Test Composer',
        'year' => 2024
    ], self::$adminToken);

    $this->assertEquals(201, $response->getStatusCode());
    // Other test validations...
}

public function testIndexSymphony()
{
    $response = $this->get('/symphonies', self::$userToken);

    $this->assertEquals(200, $response->getStatusCode());
    // Other test validations...
}
~~~~~~~

> Bu yapılandırma, Symfony uygulamasındaki çeşitli işlemler için rol tabanlı erişim kontrolünü sağlamlaştırır ve testlerde doğru rol ile doğrulamalar yapılmasını sağlar.
