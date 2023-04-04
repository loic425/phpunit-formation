---
transition: fade
---

# Update partial book

```php {all|7|9-10|12|13-15|16-18}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    public function testUpdatePartialBook(): void
    {
        BookFactory::createOne(['name' => 'Shinning']);

        $client = static::createClient();
        $iri = $this->findIriBy(Book::class, ['name' => 'Shinning']);

        $client->request(method: 'PATCH', url: $iri, options: [
            'json' => [
                'name' => 'Carrie',
            ],
            'headers' => [
                'content-type' => 'application/merge-patch+json',
            ],
        ]);
    }
}    
```

---

# Update partial book

```php {all|9|11|12}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    public function testUpdatePartialBook(): void
    {
        // [...]

        $this->assertResponseIsSuccessful();
        $this->assertJsonContains([
            '@id' => $iri,
            'name' => 'Carrie',
        ]);
    }
}    
```
