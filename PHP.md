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

## Interface

An interface is an OOP element that groups a set of function declarations without implementing them, that is, it specifies the name, return type, and arguments, but not the block of code. Interfaces are different from abstract classes, since they cannot contain any implementation at all, whereas abstract classes could mix both method definitions and implemented ones. The purpose of interfaces is to state what a class can do, but not how it is done.

---