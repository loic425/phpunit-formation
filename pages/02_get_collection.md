# Get collection

```php {all|10|12|12,4|14-15|17|18-19}
// tests/Functional/api/BookApiTest.php

// [...]
use App\Factory\BookFactory;

final class BookApiTest extends ApiTestCase
{
    // [...]
    
    public function testGetCollection(): void
    {
        BookFactory::createMany(100);

        // The client implements Symfony HttpClient's `HttpClientInterface`, and the response `ResponseInterface`
        $response = static::createClient()->request('GET', '/api/books');

        $this->assertResponseIsSuccessful();
        // Asserts that the returned content type is JSON-LD (the default)
        $this->assertResponseHeaderSame('content-type', 'application/ld+json; charset=utf-8');
    }
}
```

---
transition: fade
---

# Get collection

```php {all|10|11|12|13|14|16|17|18|19|20}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{   
    public function testGetCollection(): void
    {
        // [...]
        
        // Asserts that the returned JSON is a superset of this one
        $this->assertJsonContains([
            '@context' => '/api/contexts/Book',
            '@id' => '/api/books',
            '@type' => 'hydra:Collection',
            'hydra:totalItems' => 100,
            'hydra:view' => [
                '@id' => '/api/books?page=1',
                '@type' => 'hydra:PartialCollectionView',
                'hydra:first' => '/api/books?page=1',
                'hydra:last' => '/api/books?page=4',
                'hydra:next' => '/api/books?page=2',
            ],
        ]);
    }
```

---
transition: fade
---

# Get collection

```php {all|9-10|12-13|15-18}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{   
    public function testGetCollection(): void
    {
        // [...]
        
        // Because test fixtures are automatically loaded between each test, you can assert on them
        $this->assertCount(30, $response->toArray()['hydra:member']);

        $members = $response->toArray()['hydra:member'];
        $this->assertEquals(['@id', '@type', 'name'], array_keys($members[0]));

        // Asserts that the returned JSON is validated by the JSON Schema generated for this resource by API Platform
        // This generated JSON Schema is also used in the OpenAPI spec!
        $this->assertMatchesResourceCollectionJsonSchema(Book::class);
    }
```
