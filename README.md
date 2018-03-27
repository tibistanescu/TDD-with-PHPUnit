# TDD-with-PHPUnit


## Unit testing definition:
Unit testing is testing isolated units of code(one single function or method) to ensure that they perform as expected.

## Test Driven Development definition:
TDD is the iterative methodology used to develop new features, by first writing tests for that particular feature. 
**The iteration pattern is:** write test, run & fail test, write code, repeat step 2 and 3 until the test passes.

## Installation:
    composer require --dev phpunit/phpunit
The `--dev` option tells Composer to add packages to `require-dev` in composer.json.
Also, add autoloading to composer.json for the code to be tested and for the tests. 

## "Arrange, Act, Assert" (aka "AAA") is a very simple way to structure tests.
 * arrange all conditions and inputs
 * act on the method under test
 * assert
