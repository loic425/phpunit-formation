# Update book

```php {all|7|9|10|12-14|16|18|19}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    public function testUpdateBook(): void
    {
        BookFactory::createOne(['name' => 'Shinning']);

        $client = static::createClient();
        $iri = $this->findIriBy(Book::class, ['name' => 'Shinning']);

        $client->request('PUT', $iri, ['json' => [
            'name' => 'Carrie',
        ]]);

        $this->assertResponseIsSuccessful();
        $this->assertJsonContains([
            '@id' => $iri,
            'name' => 'Carrie',
        ]);
    }
}    
```
