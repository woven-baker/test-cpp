---
Concept: C++ Virtual Functions
Required: [ C++ Functions, C++ Object Oriented Programming, C++ Classes, C++ Inheritance, C++ Pointers, C++ References ]
Notes: 
- Currently using pieces from learncpp.com, which I will make more my own.
---

# Virtual Functions

A **virtual function** is a special type of function that, when called, resolves to the most-derived version of the function that exists between the base and derived class. This capability is known as **polymorphism**. A derived function is considered a match if it has the same signature (name, parameter types, and whether it is const) and return type as the base version of the function. Such functions are called **overrides**.

Let's take a look at an example. What is the output of the following code:

```cpp
#include <iostream>
#include <string_view>

class Base {
public:
    virtual ~Base() = default;
    virtual std::string_view getName() const { // note the use of the virtual keyword here
        return "Base";
    }
};

class Derived: public Base {
public:
    std::string_view getName() const {
        return "Derived";
    }
};

int main() {
    Derived derived {};
    Base& rBase{ derived };
    std::cout << "rBase is a " << rBase.getName() << '\n';

    return 0;
}
```

---

```
Derived
```

Because `rBase` is a reference to the base portion of a derived object, when `rBase.getName()` is evaluated, it would normally resolve to `Base::getName()`. However, `Base::getName()` is `virtual`, which tells the program to go look and see if there are any `override`d versions of the function available between Base and Derived. In this case, it will resolve to `Derived::getName()`!

With this in mind. Let's try another example. What is the output of the following code:

```cpp
#include <iostream>
#include <string_view>

class A {
public:
    virtual std::string_view getName() const { return "A"; }
};

class B: public A {
public:
    virtual std::string_view getName() const { return "B"; }
};

class C: public B {
public:
    virtual std::string_view getName() const { return "C"; }
};

class D: public C {
public:
    virtual std::string_view getName() const { return "D"; }
};

int main() {
    C c {};
    A* rBase{ &c };
    std::cout << "rBase is a " << rBase->getName() << '\n';

    return 0;
}
```

---

```
rBase is a C
```

You may have noticed that in both examples above, a virtual member function is called through either a pointer or a reference to the base class. This is not an accident.

> **Virtual function resolution only works when a member function is called through a pointer or reference to a class object**.

Additionally, in the examples above, note that `Derived::getName()` is not marked as virtual and `C::getName()` is marked as `virtual`, yet they both perform the same way.

> If a function is virtual, all matching overrides in derived classes are implicitly virtual.

# Motivational Problems

What does all of this help us with?

Let's consider the following code:

```cpp
#include <iostream>
#include <string>
#include <string_view>

class Animal {
public:
    const std::string& getName() const { return m_name; }
    virtual std::string_view speak() const { return "???"; }

protected:
    Animal(std::string_view name) : m_name{ name } {}

    std::string m_name {};
};

class Cat: public Animal {
public:
    Cat(std::string_view name) : Animal{ name } {}
    virtual std::string_view speak() const { return "Meow"; }
};

class Dog: public Animal {
public:
    Dog(std::string_view name) : Animal{name} {}
    virtual std::string_view speak() const { return "Woof"; }
};
```

We have a base class `Animal` and two derived classes `Cat` and `Dog`. Notice that `Animal`'s constructor is `protected` to prevent someone from creating an instance of an `Animal` directly -- they must create an instance of one of the derived classes instead.

## Extensibility

One thing it lets us do is write functions like `report()`:

```cpp
void report(const Animal& animal) {
    std::cout << animal.getName() << " says " << animal.speak() << '\n';
}

int main() {
    Cat cat{ "Fred" };
    Dog dog{ "Garbo" };

    report(cat);
    report(dog);

    return 0;
}
```

If we ever want to extend our code to support additional derived classes of `Animal`, we don't need to make any changes to our `report()` function.

Go ahead and create a new derived class `Bird`, then make a new call to `report()`, passing in an instance of `Bird`.

---

```cpp
class Bird: public Animal {
public:
    Bird(std::string_view name) : Animal{ name } {}
    virtual std::string_view speak() const { return "Gawk"; }
};

int main() {
    Cat cat{ "Fred" };
    Dog dog{ "Garbo" };
    Bird bird{ "Barry" };

    report(cat);
    report(dog);
    report(bird);

    return 0;
}
```

## Containers

Lastly, we can put many instances of different derived classes together into a container or array, allowing us to write more generic code that like the previous example is very easy to extend if we need to:

```cpp
int main() {
    Cat fred{ "Fred" };
    Cat misty{ "Misty" };
    Cat zeke{ "Zeke" };

    Dog garbo{ "Garbo" };
    Dog pooky{ "Pooky" };
    Dog truffle{ "Truffle" };

    Bird zana{ "Zana" };
    Bird winnie{ "Winnie" };
    Bird ben{ "Ben" };

    Animal* animals[]{ &fred, &garbo, &misty, &pooky, &truffle, &zeke, &zana, &winnie, &ben };

    for (const auto* animal : animals) {
        std::cout << animal->getName() << " says " << animal->speak() << '\n';
    }
}
```

# Guidelines

## Use the `override` specifier [CG: C.128]
Let's assume you define a struct X:

```cpp
#include <iostream>

struct X {
    virtual void f(int32_t a) {
        std::cout << "X::f(): " << a << '\n';
    }
};
```

Some time later you want to create a new struct Y that extends the functionality of X:

```cpp
struct Y : X {
	virtual void f(int64_t a) {
        std::cout << "Y::f(): " << a << '\n';
    }
};
```

What is the output of the following code:

```cpp
...

X* x = new Y;
x->f(4);

...
```

---

```
X::f(): 4
```

Why did this happen?

If you look carefully, when we created `Y::f()`, we gave the function argument the type `int64_t` when `X::f()` uses `int32_t`. Because the function signature is different between the base and derived structs, the compiler does not override `X::f()` with `Y::f()`. This is a source of some potentially very confusing bugs. What's worse, the compiler doesn't give us any warnings or errors!

Thankfully we can avoid these kinds of bugs by adding the `override` specifier to the end of the function we want to override:

```cpp
struct Y : X {
	virtual void f(int64_t a) override {
        ...
    }
};
```

Now if we try and compile the previous code the compiler complains:

```bash
error: 'virtual void Y::f(int64_t)' marked 'override', but does not override
```

The compiler has caught the inconsistency, and we can replace the `int64_t` with `int32_t` and move on.

# Exercises

## Exercise 1
What is the output of the following code:
```cpp
#include<iostream>
using namespace std;
 
struct Base {
    virtual void show() {
        cout << "Base" << "\n";
    }
};
 
struct Derived : Base {
    void show() override {
        cout << "Derived" << "\n";
    }
};
 
int main() {
    Base *bp = new Derived;
    bp->show();
 
    Base &br = *bp;
    br.show();
}
```

```
A: Derived
   Derived

B: Base
   Derived

C: Derived
   Base

D: Base
   Base

E: Compiler Error
```

---

```
A
```

## Exercise 2
What is the output of the following code:

```cpp
#include <iostream>
#include <string_view>

class A {
public:
    virtual std::string_view getName() const { return "A"; }
};

class B: public A {
public:
    virtual std::string_view getName() const { return "B"; }
};

class C: public B {
public:
    virtual std::string_view getName() const { return "C"; }
};

class D: public C {
public:
    virtual std::string_view getName() const { return "D"; }
};

int main() {
    C c;
    B& rBase{ c };
    std::cout << rBase.getName() << '\n';

    return 0;
}
```

```
A: A

B: B

C: C

D: D

E: Compiler Error
```

---

```
C
```

## Exercise 3
What is the output of the following code:

```cpp
#include <iostream>
#include <string_view>

class A {
public:
    std::string_view getName() const { return "A"; }
};

class B: public A {
public:
    std::string_view getName() const { return "B"; }
};

class C: public B {
public:
    std::string_view getName() const { return "C"; }
};

class D: public C {
public:
    std::string_view getName() const { return "D"; }
};

int main() {
    C c;
    A& rBase{ c };
    std::cout << rBase.getName() << '\n';

    return 0;
}
```

```
A: A

B: B

C: C

D: D

E: Compiler Error
```

---

```
A
```