PHP notes
=

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