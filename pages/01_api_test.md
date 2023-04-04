# API test

```php {all|11|11,7|13|13,8|14|14,9}
// tests/Functional/api/BookApiTest.php

declare(strict_types=1);

namespace App\Tests\Functional\Api;

use ApiPlatform\Symfony\Bundle\Test\ApiTestCase;
use Zenstruck\Foundry\Test\Factories;
use Zenstruck\Foundry\Test\ResetDatabase;

final class BookApiTest extends ApiTestCase
{
    use Factories;
    use ResetDatabase;
}
```
