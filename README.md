# TDD-with-PHPUnit


### Unit testing definition:
Unit testing is testing isolated units of code(one single function or method) to ensure that they perform as expected.

### Test Driven Development definition:
TDD is the iterative methodology used to develop new features, by first writing tests for that particular feature.  
**The iteration pattern is:** write test, run & fail test, write code, repeat step 2 and 3 until the test passes.

### Installation:
    composer require --dev phpunit/phpunit
The `--dev` option tells Composer to add packages to `require-dev` in composer.json.  
Also, add autoloading to composer.json for the code to be tested and for the tests.
```json
{
    "require-dev": {
        "phpunit/phpunit": "^7.0"
    },
    "autoload": {
        "psr-4": {
            "TDD\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "TDD\\Test\\": "tests/"
        }
    }
}
```
### A simple test
The main ideea is that we need a class to test and a test class.  
The test class is named `ClassTest`, has to extend `abstract class TestCase`, has a method called `testMethod` where Method is the name of the method under test.

```php
namespace TDD;

// this is the class we're testing
class Math {
    public function add(array $items = []) {
        return array_sum($items);
    }
}
```
```php
namespace TDD\Test;

use PHPUnit\Framework\TestCase;
use TDD\Math;

class MathTest extends TestCase {
    public function testAdd() {
        $Math = new Math();
        $this->assertEquals(
            15,
            $Math->add([0,2,5,8]),
            "When summing the total should equal 15."
        );
    }
}
```

### "Arrange, Act, Assert" (aka "AAA") is a very simple way to structure tests.
 * arrange all conditions and inputs
 * act on the method under test
 * assert
