---
Concept: C++ Inheritance
Required: [ C++ Functions, C++ Object Oriented Programming, C++ Classes, C++ Inheritance ]
Notes: 
---

# Multiple Inheritance
Multiple inheritance is a feature in C++ that allows a class to inherit from more than one base class. This allows a derived class to inherit attributes and behaviors from multiple parent classes, promoting code reusability and flexibility.

## Basics of Multiple Inheritance
To create a derived class that inherits from multiple base classes, use the `:` symbol followed by the `public` keyword and the name of each base class, separated by commas:

```cpp
class Base1 {
public:
    // ...
};

class Base2 {
public:
    // ...
};

class Derived : public Base1, public Base2 {
public:
    // ...
};
```

In this example, Derived is a derived class that inherits from both Base1 and Base2.

## Constructor and Destructor Order in Multiple Inheritance
When you create an object of a derived class that has multiple base classes, the constructors of the base classes are called in the order they are listed in the class declaration. The derived class's constructor is called after all the base class constructors.

Similarly, when the derived class object is destroyed, the order of destructor calls is the reverse of the constructor calls: first, the derived class destructor is called, followed by the base class destructors in the reverse order of their listing in the class declaration.

Knowing this, what is the output of the following code?

```cpp
class Base1 {
public:
    Base1() { std::cout << "Base1 constructor\n"; }
    ~Base1() { std::cout << "Base1 destructor\n"; }
};

class Base2 {
public:
    Base2() { std::cout << "Base2 constructor\n"; }
    ~Base2() { std::cout << "Base2 destructor\n"; }
};

class Derived : public Base2, public Base1 {
public:
    Derived() { std::cout << "Derived constructor\n"; }
    ~Derived() { std::cout << "Derived destructor\n"; }
};

int main() {
    Derived derived;
    return 0;
}
```

```
```

***

```
Base2 constructor
Base1 constructor
Derived constructor
Derived destructor
Base1 destructor
Base2 destructor
```

## Ambiguity Resolution in Multiple Inheritance
In the following code, we have a derived class that inherits from multiple base classes:

```cpp
class Base1 {
public:
    void print() { std::cout << "Base1 print\n"; }
};

class Base2 {
public:
    void print() { std::cout << "Base2 print\n"; }
};

class Derived : public Base1, public Base2 {
public:
    void printBoth() {
        print();
    }
};

int main() {
    Derived derived;
    derived.printBoth();
    return 0;
}
```

What should be the output of the above code?

```
```

***

```bash
error: member 'print'
    found in multiple base classes of different types
        print();
        ^                                                         
note: member found by                         
        ambiguous name lookup                                       
    void print() { std::cout << "Base1 print\n"; }                
         ^                                                        
note: member found by                        
        ambiguous name lookup                                       
    void print() { std::cout << "Base2 print\n"; }                
         ^                                                        
1 error generated.
```

When a derived class inherits from multiple base classes that have members with the same name, you must explicitly specify which base class member you want to access from the derived class to avoid ambiguity:

```cpp
class Base1 {
public:
    void print() { std::cout << "Base1 print\n"; }
};

class Base2 {
public:
    void print() { std::cout << "Base2 print\n"; }
};

class Derived : public Base1, public Base2 {
public:
    void printBoth() {
        Base1::print();
        Base2::print();
    }
};

int main() {
    Derived derived;
    derived.printBoth();
    return 0;
}
```

The output of the above code is, perhaps not surprisingly:

```cpp
Base1 print
Base2 print
```

# Problems With Multiple Inheritance

## Diamond Problem

What is the output of the following code:

```cpp
class Base {
public:
    Base() {
        std::cout << "Base constructor" << "\n";
    }
};

class Derived1 : public Base {
public:
    Derived1() {
        std::cout << "Derived1 constructor" << "\n";
    }
};

class Derived2 : public Base {
public:
    Derived2() {
        std::cout << "Derived2 constructor" << "\n";
    }
};

class MostDerived : public Derived1, public Derived2 {
public:
    MostDerived() {
        std::cout << "MostDerived constructor" << "\n";
    }
};

int main() {
    MostDerived derived;
    return 0;
}
```

```
```

***

```
Base constructor
Derived1 constructor
Base constructor
Derived2 constructor
MostDerived constructor
```

Wow, that's likely not what we intended to have happen, is it?

To prevent duplicate construction of this base class, we can utilize virtual inheritance, but this will be left as an exercise for the reader.

## Exponential Complexity
```cpp
#include <iostream>

class USBDevice {
private:
    long id {};

public:
    USBDevice(long id)
        : id { id }
    {
    }

    long getID() const { return id; }
};

class NetworkDevice {
private:
    long id {};

public:
    NetworkDevice(long id)
        : id { id }
    {
    }

    long getID() const { return id; }
};

class WirelessAdapter: public USBDevice, public NetworkDevice {
public:
    WirelessAdapter(long usbId, long networkId)
        : USBDevice { usbId }, NetworkDevice { networkId }
    {
    }
};

int main() {
    WirelessAdapter c54G { 5442, 181742 };
    std::cout << c54G.getID(); // Which getID() do we call?

    return 0;
}
```

## Summary

Multiple inheritance is a feature in C++ that allows a class to inherit from more than one base class. This enables a derived class to inherit attributes and behaviors from multiple parent classes. However, despite this being a feature of C++, I would encourage you to stay away from it due to the exponential complexity that often results from its use. Often times composition is better suited for expressing relationships that at first sight might appear to require multiple inheritance.

# Exercises

## Exercise 1
Consider the following class hierarchy:

```cpp
class Memory {
public:
    long getID() const;
};

class Serial {
public:
    long getID() const;
};

class SerialFlash : public Memory, public Serial {
};
```

What would happen if an object of the SerialFlash class is created and the getID() function is called?

```
A: The getID() function of the Memory class will be called.

B: The getID() function of the SerialFlash class will be called.

C: A compiler error occurs due to ambiguity.

D: The getID() function of the SerialFlash class will be called.
```

***

```
C: A compiler error occurs due to ambiguity.
```

## Exercise 2
A coworker created the following diagram of the relationship between three classes, `Engine`, `Transmission`, and `Vehicle`:

```
Engine (Base class)      Transmission (Base class)
        ^                      ^
         \                    /
         Vehicle (Derived class)
```

What corrections should you make to this diagram?

```
A: The arrows should be pointed in the opposite direction.
B: Transmission should inherit from Engine, and Vehicle should inherit from Transmission.
C: No corrections.
D: It would be more appropriate to use composition instead of inheritance.
```

***

```
D: It would be more appropriate to use composition instead of inheritance.
```
