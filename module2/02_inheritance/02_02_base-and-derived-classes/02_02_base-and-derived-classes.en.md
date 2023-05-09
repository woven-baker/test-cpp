---
Concept: C++ Base and Derived Classes
Required: [ C++ Classes, C++ Functions, C++ Object Oriented Programming ]
Notes:
---

# Base and Derived Classes

To create a derived class from a base class, you need to use the `:` symbol followed by the `public` keyword and the name of the base class:

```cpp
class Base {
public:
    // ...
};

class Derived : public Base {
public:
    // ...
};
```

In this example, `Derived` is a derived class that inherits from the `Base` class.

When you create an object of the derived class, the base class's constructor is called first, followed by the derived class's constructor. Similarly, when the derived class object is destroyed, the derived class's destructor is called first, followed by the base class's destructor.

## Protected Members
Suppose you are designing a simple geometry library. You have a base class called Shape, which contains common properties and methods for all shapes. You also have several derived classes, such as Circle, Rectangle, and Triangle, that inherit from Shape. Each derived class should be able to access and modify the base class's member variables, but external code should not have direct access to these variables.

Initially, you might define the Shape class with public member variables:

```cpp
class Shape {
public:
    int x;
    int y;
};
```

What is wrong with this code?

---

This approach exposes the member variables to any code that uses the Shape class, violating the principle of encapsulation.

Let's use private variables then:

```cpp
class Shape {
private:
    int x;
    int y;
};
```

And let's create a derived type Circle that inherits from Shape and attempts to do some work involving Shape's members:

```cpp
class Circle : public Shape {
public:
    void accessMembers() {
        // ...
        x = 30;
        // ...
    }
};
```

What happens when we try to compile this code?

```bash
In member function 'void Circle::accessMembers()':
error: 'int Shape::x' is private within this context
   11 |         x = 30;
      |         ^
note: declared private here
    3 |     int x;
      |         ^
Compiler returned: 1
```

To solve this issue, you can use the `protected` access specifier to restrict access to the member variables. Protected members are accessible to both the base class and its derived classes, but they are not accessible to other classes.

Here is the modified Shape class with protected member variables:

```cpp
class Shape {
protected:
    int x;
    int y;
    int color;
};
```

Now, let's create some more complete derived classes and see how protected members work:

```cpp
class Circle : public Shape {
public:
    void setCenter(int x, int y) {
        this->x = x;
        this->y = y;
    }
};

class Rectangle : public Shape {
public:
    void setPosition(int x, int y) {
        this->x = x;
        this->y = y;
    }
};
```

Both Circle and Rectangle can access the protected members `x`, `y`, and `color` from the Shape class, allowing them to manipulate these properties as needed.

## Constructors and Destructors in Inheritance
When you create an object of a derived class, the base class constructor is called first, followed by the derived class constructor. The order of destructor calls is the reverse of the constructor calls: first, the derived class destructor is called, and then the base class destructor.

With that in mind, what will be the output of the following code?

```cpp
class Base {
public:
    Base() { std::cout << "Base constructor\n"; }
    ~Base() { std::cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    Derived() { std::cout << "Derived constructor\n"; }
    ~Derived() { std::cout << "Derived destructor\n"; }
};

int main() {
    Derived derived;
    return 0;
}
```

---

```cpp
Base constructor
Derived constructor
Derived destructor
Base destructor
```

From this output we can see that during construction, the base constructor is called first, followed by the derived constructor. During destruction, the derived destructor is called first, followed by the base destructor.

# Exercises

## Exercise 1

What is the output of the following code:

```cpp
#include <iostream>

class Base {
public:
    void print() {
        std::cout << "Base\n";
    }
};

class Derived : public Base {
public:
    void print() {
        std::cout << "Derived\n";
    }
};

int main() {
    Derived derived;
    Base& rBase{ derived };
    rBase.print();

    return 0;
}
```

```
A: Base
B: Derived
C: Compiler Error
```

---

```
A
```

## Exercise 2

What is the output of the following code:

```cpp
#include <iostream>

class Base {
public:
    int value;

    Base(int value)
        : value{ value } {
    }

    void print() {
        std::cout << "Base value: " << value << '\n';
    }
};

class Derived : public Base {
public:
    Derived(int value)
        : Base{ value } {
    }

    void print() {
        std::cout << "Derived value: " << value << '\n';
    }
};

int main() {
    Derived derived{ 5 };
    derived.print();

    return 0;
}
```

```
A: Base value: 5
B: Derived value: 5
C: Compiler Error
```

---

```
B
```

## Exercise 3

What is the output of the following program:

```cpp
#include <iostream>

class Base {
public:
    Base() {
        std::cout << "Base constructor\n";
    }

    ~Base() {
        std::cout << "Base destructor\n";
    }
};

class Derived : public Base {
public:
    Derived() {
        std::cout << "Derived constructor\n";
    }

    ~Derived() {
        std::cout << "Derived destructor\n";
    }
};

int main() {
    Base* ptr = new Derived;
    delete ptr;

    return 0;
}
```

```
A: Base constructor
   Derived constructor
   Base destructor

B: Base constructor
   Derived constructor
   Derived destructor
   Base destructor

C: Base constructor
   Derived constructor
   Derived destructor

D: Compiler error
```

---

```
A
```