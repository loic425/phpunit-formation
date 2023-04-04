# Create book

```php {all|9-11|13|14|16|17|18|20|21}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    // [...]

    public function testCreateBook(): void
    {
        $response = static::createClient()->request('POST', '/api/books', ['json' => [
            'name' => 'Shinning',
        ]]);

        $this->assertResponseStatusCodeSame(201);
        $this->assertResponseHeaderSame('content-type', 'application/ld+json; charset=utf-8');
        $this->assertJsonContains([
            '@context' => '/api/contexts/Book',
            '@type' => 'Book',
            'name' => 'Shinning',
        ]);
        $this->assertMatchesRegularExpression('~^/api/books/\d+$~', $response->toArray()['@id']);
        $this->assertMatchesResourceItemJsonSchema(Book::class);
    }
}    
```
