# TDD-with-PHPUnit


### Unit testing definition:
>testing isolated units of code(one single function or method) to ensure that they perform as expected.

### Test Driven Development definition:
>the iterative methodology used to develop new features, by first writing tests for that particular feature.  

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
To load the new autoload configuration, run in terminal:
`composer dump-autoload`

### A simple test (check if a method returns a value equal to expected)
The main ideea is that we need a class to test and a test class.  
* the test class is named `ClassTest`, where **Class** is the name of the class under test
* the test class has to extend the abstract class `TestCase` from phpunit.
* the test class has a method called `testMethod` where **Method** is the name of the method under test.
* we use `assertEquals` method inherited from `TestCase` class, which inherits it from the `Assert` class in phpunit.
* `assertEquals` parameters:
  * expected value
  * actual value (results from expression)
  * a message that is displayed in case the assertion fails

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
            10,
            $Math->add([0,1,3,6]),
            "When summing the total should equal 15."
        );
    }
}
```

### Running the test
In the terminal run the command:  
- run all tests: `phpunit tests/`
- run all tests in a specific file: `phpunit tests/MathTest.php`
- run a specific test method: `phpunit tests --filter=testAdd` or `phpunit tests --filter=MathTest::testAdd`
Also, an XML config file (phpunit.xml in app root) can be used to better organize tests:
```xml
<phpunit
    colors="true"
    >
    <testsuites>
        <testsuite name="app">
            <directory>tests</directory>
        </testsuite>
        <testsuite name="math">
            <directory>tests</directory>
            <exclude>tests/OtherTest.php</exclude>
        </testsuite>
    </testsuites>
</phpunit>
```
Commands:
- run tests according to the xml file: `phpunit`
- run test suites: `phpunit --testsuite="math"`
- run test suites and filter: `phpunit --testsuite="math" --filter="testAdd"`

### "Arrange, Act, Assert" (aka "AAA") is a very simple way to structure tests.
 * arrange all conditions and inputs
 * act on the method under test
 * assert
 
**Arrange**: the `TestCase` class has 2 methods that we need to implement:  
`setUp()` is called before each test.  
`tearDown()` is called after each test (if your setUp() just creates plain PHP objects, you can generally ignore tearDown()).
Also, we have a `$input` variable.  
**Act**:  
The `$output` comes from calling the tested method with the `$input` given.  
**Assert**  
Finally, `assertEquals()`.

```php
namespace TDD\Test;

use PHPUnit\Framework\TestCase;
use TDD\Math;

class MathTest extends TestCase {
    public function setUp()
    {
        $this->Math = new Math();
    }

    public function tearDown()
    {
        unset($this->Math);
    }

    public function testAdd()
    {
        $input = [0,1,3,6];
        $output = $this->Math->add($input);
        $this->assertEquals(
            10,
            $output,
            "When summing the total should equal 10."
        );
    }
}
```

### Adding a second test (for a method that calculates the arithmetic mean)
In true TDD fashion, first create the test method in the `MathTest` class:
```php
public function testCalcArithmeticMean()
{
    $itemsInput = [0,1,3,6];
    $countInput = 4;
    $output = $this->Math->calcArithmeticMean($itemsInput, $countInput);
    $this->assertEquals(
        2.5,
        $output,
        "When averaged the result should be 2.5."
    );
}
```
After that, create the method in the `Math` class:
```php
public function calcArithmeticMean(array $items, int $count)
{
    return array_sum($items)/$count;
}
```
Finally, run the test.  
