# Delete book

```php {all|7|9-10|12|14|15-18}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    public function testDeleteBook(): void
    {
        BookFactory::createOne(['name' => 'Shinning']);

        $client = static::createClient();
        $iri = $this->findIriBy(Book::class, ['name' => 'Shinning']);

        $client->request('DELETE', $iri);

        $this->assertResponseStatusCodeSame(204);
        $this->assertNull(
        // Through the container, you can access all your services from the tests, including the ORM, the mailer, remote API clients...
            static::getContainer()->get('doctrine')->getRepository(Book::class)->findOneBy(['name' => 'Shinning'])
        );
    }
}    
```
