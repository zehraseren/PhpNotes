+ Symfony uygulamasında `IsGranted` özniteliğini kullanılarak erişim kontrolü eklemek, kullanıcıların belirli rollere sahip olmalarını gerektiren işlemler için oldukça etkili bir yöntemdir.
+ Aşağıda, `ComposerController` class'ıa eklenecek kodun tamamlanmış ve düzenlenmiş hali verilmiştir:
~~~~~~~
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Http\Attribute\IsGranted;
use App\Entity\Composer;
use App\Repository\ComposerRepository;
use Doctrine\ORM\EntityManagerInterface;

class ComposerController extends AbstractController
{
    private $entityManager;
    private $composerRepository;

    public function __construct(EntityManagerInterface $entityManager, ComposerRepository $composerRepository)
    {
        $this->entityManager = $entityManager;
        $this->composerRepository = $composerRepository;
    }

    #[Route('/composer', name: 'composer_index', methods: ['GET'])]
    #[IsGranted('ROLE_USER')]
    public function index(): Response
    {
        $composers = $this->composerRepository->findAll();

        return $this->json($composers);
    }

    #[Route('/composer/{id}', name: 'composer_show', methods: ['GET'])]
    #[IsGranted('ROLE_USER')]
    public function show(int $id): Response
    {
        $composer = $this->composerRepository->find($id);

        if (!$composer) {
            throw $this->createNotFoundException('Composer not found');
        }

        return $this->json($composer);
    }

    #[Route('/composer', name: 'composer_create', methods: ['POST'])]
    #[IsGranted('ROLE_ADMIN')]
    public function create(Request $request): Response
    {
        $data = json_decode($request->getContent(), true);

        $composer = new Composer();
        $composer->setName($data['name']);

        $this->entityManager->persist($composer);
        $this->entityManager->flush();

        return $this->json($composer, Response::HTTP_CREATED);
    }

    #[Route('/composer/{id}', name: 'composer_update', methods: ['PUT'])]
    #[IsGranted('ROLE_ADMIN')]
    public function update(int $id, Request $request): Response
    {
        $data = json_decode($request->getContent(), true);

        $composer = $this->composerRepository->find($id);

        if (!$composer) {
            throw $this->createNotFoundException('Composer not found');
        }

        $composer->setName($data['name']);

        $this->entityManager->flush();

        return $this->json($composer);
    }

    #[Route('/composer/{id}', name: 'composer_delete', methods: ['DELETE'])]
    #[IsGranted('ROLE_ADMIN')]
    public function delete(int $id): Response
    {
        $composer = $this->composerRepository->find($id);

        if (!$composer) {
            throw $this->createNotFoundException('Composer not found');
        }

        $this->entityManager->remove($composer);
        $this->entityManager->flush();

        return $this->json(null, Response::HTTP_NO_CONTENT);
    }
}
~~~~~~~

##### 1. index ve show Method'ları
+ Bu method'lar `ROLE_USER` rolüne sahip kullanıcılar tarafından erişilebilir.
  - `index` method'u tüm composer'ları döner.
  - `show` method'u belirli bir composer'ı döner.

##### 2. create, update ve delete Method'ları
+ Bu method'lar `ROLE_ADMIN` rolüne sahip kullanıcılar tarafından erişilebilir.
  - `create` method'u yeni bir composer oluşturur.
  - `update` method'u mevcut bir composer'ı günceller.
  - `delete` method'u mevcut bir composer'ı siler.

#### Test Method'ları
+ Test method'larında bu erişim kontrollerini doğrulamak için uygun token'lar ile istekler gönderilmelidir. Örneğin:
~~~~~~~
public function testCreateComposer()
{
    $response = $this->post('/composer', [
        'name' => 'Test Composer'
    ], self::$adminToken);

    $this->assertEquals(201, $response->getStatusCode());
    // Other test validations...
}

public function testIndexComposer()
{
    $response = $this->get('/composer', self::$userToken);

    $this->assertEquals(200, $response->getStatusCode());
    // Other test validations...
}
~~~~~~~

> Bu yapılandırma, Symfony uygulamasındaki çeşitli işlemler için rol tabanlı erişim kontrolünü sağlamlaştırır ve testlerde doğru rol ile doğrulamalar yapılmasını sağlar.
