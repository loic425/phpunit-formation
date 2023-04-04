# Create invalid book

```php {all|7|9|10|13|14|15|16-21}
// tests/Functional/api/BookApiTest.php

final class BookApiTest extends ApiTestCase
{
    public function testCreateInvalidBook(): void
    {
        static::createClient()->request('POST', '/api/books', ['json' => []]);

        $this->assertResponseStatusCodeSame(422);
        $this->assertResponseHeaderSame('content-type', 'application/ld+json; charset=utf-8');

        $this->assertJsonContains([
            '@context' => '/api/contexts/ConstraintViolationList',
            '@type' => 'ConstraintViolationList',
            'hydra:title' => 'An error occurred',
            'violations' => [
                [
                    'propertyPath' => 'name',
                    'message' => 'This value should not be blank.',
                ],
            ],
        ]);
    }
}    
```
