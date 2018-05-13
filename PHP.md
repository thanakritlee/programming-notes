# PHP notes

## Inheritance

```php
class Pops {
    public function sayHi() {
        echo "Hi, I am pops.";
    }
}

class Child extends Pops {
    protected function sayHi() {
        echo "Hi, I am a child.";
    }
}
```

When overriding, the method has to have at least as much visibility as the one inherited. That means that if we inherited a protected one, we can override it with another protected or a public one, but never with a private one.

---

## Abstract Classes

An abstract class is a class that cannot be instantiated. Its sole purpose is to make sure that its children are correctly implemented. Declaring a class as abstract is done with the keyword `abstract`, followed by the definition of a normal class. We can also specify the methods that the children are forced to implement, without implementing them in the parent class. Those methods are called abstract methods, and are defined with the keyword `abstract` at the beginning. Of course, the rest of the normal methods can stay there too, and will be inherited by its children:

```php
abstract class Customer extends Person {
//...
    abstract public function getMonthlyFee();
    abstract public function getAmountToBorrow();
    abstract public function getType();
//...
}
```

---

## Interface

An interface is an OOP element that groups a set of function declarations without implementing them, that is, it specifies the name, return type, and arguments, but not the block of code. Interfaces are different from abstract classes, since they cannot contain any implementation at all, whereas abstract classes could mix both method definitions and implemented ones. The purpose of interfaces is to state what a class can do, but not how it is done.

Example of an interface:

```php
interface Customer {
    public function getMonthlyFee(): float;
    public function getAmountToBorrow(): int;
    public function getType(): string;
}
```

Implementing an interface means implementing all the methods defined in it, like when we extended an abstract class. It has all the benefits of the extension of abstract classes, such as belonging to that type—useful when type hinting. From the developer's point of view, using a class that implements an interface is like writing a contract: you ensure that your class will always have the methods declared in the interface, regardless of the implementation. Because of that, interfaces only care about public methods, which are the ones that other developers can use.

Example of an interface implement:

```php
class Basic implements Customer {
    public function getMonthlyFee(): float {
        return 5.0;
    }

    public function getAmountToBorrow(): int {
        return 3;
    }

    public function getType(): string {
        return 'Basic';
    }
}
```

The reason to use an interface is thfat you can only `extends` (abstract) from one class, but you can `implements` (interface) multiple instances at the same time.

---

## Traits

Traits are mechanisms that allow you to reuse code from multiple sources at the same time. Traits cannot be instantiated. They are containers of functionality that can be used from other classes.

To define a trait:

- define it like a class.
- use keyword `trait`.
- define its namespace.
- declare its properties and implement its methods.

Example of a trait:

```php
<?php

namespace App\Utils;

trait A {
    public function getA(string $s) {
        return 'A ' . $s;
    }
}

?>
```

To use a trait in a class, add the `use` keyword inside the class, follow by the trait name. To refer to methods of a trait, use the `$this` keyword follow by the trait method's name. 

Example of using a trait in a class:

```php
<?php

use App\Utils\A;

class B {
    // Let the class know that its using the trait A
    use A;

    public string $B = 'B';

    public function getAB() {
        // Refer to trait A method using $this keyword.
        return $this -> getA($this -> B);
    }
}

?>
```



A complicated situation is where a class uses 2 traits that implements the same methods. You have to explicitly choose which trait's method the class is going to use. To choose the method you want to use, use the operator `insteadof`. To use the operator, state the trait name and the method, `Contract::sign`, followed by `insteadof` and the trait that you are rejecting for use, `Communicator`. The result is `Contract::sign insteadof Communicator;`, where the sign method of the trait `Contract` is use instead of `Communitor`'s. Optionally, use the keyword `as` to add an alias so that both methods could be use, `Communicator::sign as makeASign;`.

Example of a class using 2 traits that implements the same method:

```php
<?php

trait Contract {
    public function sign() {
        echo "Signing the contract.";
    }
}

trait Communicator {
    public function sign() {
        echo "Signing to the waitress.";
    }
}

class Manager {
    use Contract, Communicator {
        Contract::sign insteadof Communicator;
        Communicator::sign as makeASign;
}

$manager = new Manager();
$manager->sign(); // Signing the contract.
$manager->makeASign(); // Signing to the waitress.

?>
```

---

## Exceptions

Exception template:

```php
<?php

use Exception;

try {
    throw new Exception('Exception message.');
}
catch (Exception $e) {
    echo $e -> getMessage();
}
finally {
    echo "This block always run, no matter what.";
}

?>
```

You can make your own type of Exception as well, by extending from the base class `Exception`. The Exception class below takes a message (with default `null` value) and assign it to `$message`. Then it calls the constructor for the base class `Exception`, providing the argument `$message`.

Custom Exception type are normally use to implement extra functions when constructed.

Custome Exception template:

```php
<?php

use Exception;

class InvalidIdException extends Exception {
    public function __construct($message = null) {
        $message = $message ?: 'Invalid id provided.';
        parent::__construct($message);
    }
}

?>
```

The order of the catch statement matters. PHP will run the first catch statement that is true.

The exmaple code below will run the InvalidIdException catch block:

```php
<?php

use InvalidIdException;
use Exception;

try {
    throw new InvalidIdException('Invalid id.');
}
catch (InvalidIdException $e) {
    echo $e -> getMessage();
}
catch (Exception $e) {
    echoe $e -> getMessage();
}

?>
```

The exmaple code below will run the Exception catch block, eventhough the type of Exception is `InvalidIdException`. This is because it `extends` from the base class `Exception`, making it an `Exception`.

```php
<?php

use InvalidIdException;
use Exception;

try {
    throw new InvalidIdException('Invalid id.');
}
catch (Exception $e) {
    echoe $e -> getMessage();
}
catch (InvalidIdException $e) {
    echo $e -> getMessage();
}

?>
```

---