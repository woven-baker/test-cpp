# Classes

## **Organizing Related Data and Functions**

Imagine you are creating a program to manage a car dealership. You need to keep track of cars, their make, model, price, and other details. Additionally, you have to perform actions like selling cars, calculating taxes, and searching for cars.

You could use separate variables and functions for each car and its attributes, but this approach would quickly become unwieldy and difficult to maintain. Classes offer a more organized and efficient way to manage related data and functions by allowing us to **encapsulate** them in a single place.

## **What is a Class?**

A class is a user-defined type, like a struct, that acts as a blueprint for creating objects. An object is an actual instance of the class that holds the data and functions defined in the class.

Let's create a simple `Car` class to represent cars in our dealership:

```cpp
#include <iostream>
#include <string>

class Car {
public:
    Car(std::string make, std::string model, int year, double price)
        : make(make), model(model), year(year), price(price) {
        if (price < 0) {
            std::cerr << "Negative Car Price!" << "\n";
        }
    }
		
    void setPrice(double price) {
        printPriceChange(this->price, price);
        this->price = price;
    }

    std::string getMake() const { return make; }
    std::string getModel() const { return model; }
    int getYear() const { return year; }
    double getPrice() const { return price; }

    void displayInfo() const {
        std::cout << year << " " << make << " " << model << " - $" << price << "\n";
    }

private:
    void printPriceChange(double from, double to) const {
        std::cout << "Changing price from " << from << " to " << to << "\n";
    }

    std::string make;
    std::string model;
    int year;
    double price;
};
```

## **Creating Objects and Using Methods**

Now that we have our `Car` class defined, let's see how we can create objects (instances of the class) and use their methods.

```cpp
int main() {
    Car car1("Toyota", "Camry", 2020, 25'000);
    Car car2("Honda", "Civic", 2021, 23'000);
    Car car3("Ford", "Mustang", 2019, 28'000);

    std::array<Car, 3> cars {car1, car2, car3};

    for (const Car& car : cars) {
        car.displayInfo();
    }
    
    car1.setPrice(26'000);
    std::cout << car1.getPrice() << "\n";
}
```

If we run the above code, what output would we expect?

---

```
2020 Toyota Camry - $25000
2021 Honda Civic - $23000
2019 Ford Mustang - $28000
Changing price from 25000 to 26000
26000
```

## Anatomy of a Class

This `Car` class demonstrates a variety of features of C++ classes. Let’s go more into detail about each one.

### **Class definition**

```cpp
class Car {
// ...
};
```

This code defines a new class named **Car**.

### **Access specifiers**

```cpp
class Car {
public:
		// ...
private:
    // ...
};
```

`public` and `private` are access specifiers that determine the visibility of class members (both data and functions). Members declared under `public` are accessible from outside the class, while members declared under `private` are only accessible within the class itself. The members within a class are `private` by default, even if no access specifiers are used.

Knowing this, what output would we expect if we add the following code to the end of main:

```cpp
std::cout << car1.make << "\n";
```

---

```cpp
class.cpp:49:23: error: 'make' is a private member of 'Car'
    std::cout << car1.make << "\n";
                      ^
class.cpp:30:17: note: declared private here
    std::string make;
                ^
1 error generated.
```

We get a compiler error complaining that we are trying to access a private member of Car.

What output would we expect if we add the following code instead:

```cpp
car1.printPriceChange(25'000, 26'000);
```

---

```cpp
class.cpp:48:10: error: 'printPriceChange' is a private member of 'Car'
    car1.printPriceChange(25'000, 26'000);
         ^
class.cpp:25:10: note: declared private here
    void printPriceChange(double from, double to) const {
         ^
1 error generated.
```

Here too we get a similar compiler error complaining that we are trying to access a private member from outside of the class.

We can, however, see that `printPriceChange` is called from within `setPrice` just fine. The private member variables are also accessed by every method just fine.

Why are access specifiers important?

---

Access specifiers are important because they allow you to tightly control the interface to a class. They allow you to hide implementation details, ensuring that the class's internal state cannot be manipulated directly by external code. This level of encapsulation helps to prevent unintended side effects and makes it easier to modify the class implementation without affecting other parts of the code.

Imagine if the `price` member were publicly accessible, and this member was heavily accessed directly throughout a large codebase. Now, imagine you want to modify the `Car` class to add a `discount` member that would be subtracted from the base price. How would you make this change?

Since the rest of the codebase is accessing the `price` member directly, the rest of the codebase won’t see the discount reflected, so every piece of code that uses `price` directly must be changed. This could be a massive amount of work.

Ideally, we make discounts transparent to the user of the `Car` class. We can do that by hiding `price` by making it private and adding logic to `getPrice()` to calculate the new price:

```cpp
double getPrice() const { return price - discount; }
```

As long as other code calls `getPrice()`, we are free to change the implementation in only one place, the `Car` class, and no other code needs to change.

### **Constructor**

A constructor is a function with the same name as the class with no return type. We use its parameters to initialize the class:

```cpp
Car(std::string make, std::string model, int year, double price)
        : make(make), model(model), year(year), price(price) {
    if (price < 0) {
        std::cerr << "Negative Car Price!" << "\n";
    }
}
```

The `Car` constructor is called when a new `Car` object is created. It first initializes the corresponding member variables using an **initializer list**:

```cpp
: make(make), model(model), year(year), price(price)
```

Then the body of the constructor is executed:

```cpp
{
    if (price < 0) {
        std::cerr << "Negative Car Price!" << "\n";
    }
}
```

Here we are doing a check to make sure that the car object is initialized with valid data, and logging an error if it isn’t. Constructor should be used to check for invariants, making sure that the state of the object after construction is valid.

You can define multiple constructors for a class, depending on how you want an object of the class to be able to be created:

```cpp
class Car {
public:
    Car() : make("Toyota"), model("Camry"), year(2020), price(25'000) {}

    Car(std::string make, std::string model, int year, double price) : make(make), model(model), year(year), price(price) {
        if (price < 0) {
            std::cerr << "Negative Car Price!" << "\n";
        }
    }

    Car(std::string make, std::string model, int year) : Car(make, model, year, 0) {}

    // ...
};
```

The first constructor is called a **default constructor** because it can be called with no arguments. We can create an instance of the `Car` class by calling this constructor like so:

```cpp
int main() {
    auto car = Car();
    car.displayInfo();
}
```

Or, if we simply declare a variable of type `Car`, the compiler will automatically invoke the default constructor, so the following code is identical:

```cpp
int main() {
    Car car;
    car.displayInfo();
}
```

What is the output of this previous code?

---

```
2020 Toyota Camry - $25000
```

The second constructor takes four arguments, one for each member variable, allowing someone to have full control over how the entire state of the object is initialized:

```cpp
int main() {
    auto car = Car("Toyota", "Camry", 2020, 25'000);
    car.displayInfo();
}
```

The third constructor just takes three arguments, and leaves the price out. We can see here that in the initializer list, instead of initializing the members, we are using **constructor delegation** to call the second constructor. This can be very useful when we don't want to duplicate the constructor body logic across different constructors:

```cpp
int main() {
    auto car = Car("Toyota", "Camry", 2020);
    car.displayInfo();

    car.setPrice(25'000);
    car.displayInfo();
}
```

What is the output of this previous code?

---

```
2020 Toyota Camry - $0
Changing price from 0 to 25000
2020 Toyota Camry - $25000
```

### Member variables

The `Car` class has four member variables: `make`, `model`, `year`, and `price`. These variables store the state of the object and are initialized in the constructor.

### **Member functions**

The `Car` class has several member functions, including `setPrice`, `getMake`, `getModel`, `getYear`, `getPrice`, `displayInfo`, and `printPriceChange`.

All member functions of a class have implicit access to all member variables of that class.

### **const member functions**

```cpp
std::string getMake() const { return make; }
std::string getModel() const { return model; }
int getYear() const { return year; }
double getPrice() const { return price; }
```

Functions like `getMake`, `getModel`, `getYear`, `getPrice`, `displayInfo`, and `printPriceChange` are declared as `const`. This keyword means that these functions don't modify the state of the object and can be called on `const` objects.

Member functions should be marked as `const` when they are not intended to modify the state of the object on which they are called. Using `const` with member functions provides several benefits:

1. **Indicate intent**: Marking a member function as `const` communicates to other developers that this function is read-only and doesn't modify the object's state. This makes the code more readable and easier to understand.
2. **Error prevention**: If a `const` member function tries to modify a member variable, the compiler will generate an error, preventing unintentional changes to the object's state.
3. **Support for const objects**: `const` member functions can be called on `const` objects, which are read-only instances of the class. If a member function is not marked as `const`, it cannot be called on a `const` object, as it is assumed to potentially modify the object's state.
4. **Optimization**: Marking member functions as `const` can enable compiler optimizations. The compiler can make assumptions about the constness of objects and their member functions, which may result in better performance.

In short, always mark member functions `const` if they don’t modify the state of the object.

### **this pointer**

In the `setPrice` function, the `this` pointer is used to access the object's `price` member variable. The `this` pointer is an implicit pointer to the object itself, and it's used to differentiate between the parameter `price` and the member variable `price`.

## Interface and Implementation
We defined the `Car` class above in the same file as `main()`, but it is best to split a class into its interface and its implementation. The interface will go in its own `.hpp` header file, while the implementation will go into its own `.cpp` file and include the corresponding header:

### **car.hpp**
```cpp
#include <string>

class Car {
public:
    Car(std::string make, std::string model, int year, double price);

    void setPrice(double price);

    std::string getMake() const;
    std::string getModel() const;
    int getYear() const;
    double getPrice() const;

    void displayInfo() const;

private:
    void printPriceChange(double from, double to) const;

    std::string make;
    std::string model;
    int year;
    double price;
};
```

### **car.cpp**
```cpp
#include "car.hpp"
#include <iostream>

Car::Car(std::string make, std::string model, int year, double price)
    : make(make), model(model), year(year), price(price) {
    if (price < 0) {
        std::cerr << "Negative Car Price!" << "\n";
    }
}

void Car::setPrice(double price) {
    printPriceChange(this->price, price);
    this->price = price;
}

std::string Car::getMake() const { return make; }
std::string Car::getModel() const { return model; }
int Car::getYear() const { return year; }
double Car::getPrice() const { return price; }

void Car::displayInfo() const {
    std::cout << year << " " << make << " " << model << " - $" << price << "\n";
}

void Car::printPriceChange(double from, double to) const {
    std::cout << "Changing price from " << from << " to " << to << "\n";
}
```

Notice here how we use the name of the class followed by the `::` scope resolution operator to define `Car`'s member functions outside of the class. This is because these functions exist in the scope of the `Car` class, which you can think of as a namespace called `Car`.

### **main.cpp**
```cpp
#include "car.hpp"
#include <array>
#include <iostream>

int main() {
    Car car1("Toyota", "Camry", 2020, 25'000);
    Car car2("Honda", "Civic", 2021, 23'000);
    Car car3("Ford", "Mustang", 2019, 28'000);

    std::array<Car, 3> cars {car1, car2, car3};

    for (const Car& car : cars) {
        car.displayInfo();
    }
    
    car1.setPrice(26'000);
    std::cout << car1.getPrice() << "\n";
}
```

We can then compile and run:

```bash
g++ -std=c++20 -o car main.cpp car.cpp
./car
```

# Exercises

## **Exercise 1**

Create a simple class `Person` with two private member variables: `std::string name` and `int age`. Implement a constructor that takes two arguments (a string for the name and an integer for the age) and initializes the member variables with the given values.

---

```cpp
class Person {
public:
    Person(std::string name, int age) : name(name), age(age) {}

private:
    std::string name;
    int age;
};
```

## Exercise 2

Add a constructor that takes no arguments to the `Person` class that initializes the `name` to `"Unknown"` and the `age` to `0`.

---

```cpp
class Person {
public:
    Person() : name("Unknown"), age(0) {}
    Person(std::string name, int age) : name(name), age(age) {}

private:
    std::string name;
    int age;
};
```

## Exercise 3

Implement a constructor for the `Person` class that takes only the name as an argument and initializes the age to `0`. Use **constructor delegation** to avoid duplicating the code.

---

```cpp
class Person {
public:
    Person() : Person("Unknown", 0) {}
    Person(std::string name) : Person(name, 0) {}
    Person(std::string name, int age) : name(name), age(age) {}

private:
    std::string name;
    int age;
};
```

## Exercise 4

Create a class `Vehicle` with two private member variables: `brand` and `wheels`. Implement a constructor that takes two arguments, one for each member, and initializes the member variables. Also, add a constructor that takes a `Vehicle` object as a `const` reference and initializes the member variables using the values from the given object.

```cpp

```

---

```cpp
class Vehicle {
public:
    Vehicle(std::string brand, int wheels) : brand(brand), wheels(wheels) {}
    Vehicle(const Vehicle& other) : brand(other.brand), wheels(other.wheels) {}

private:
    std::string brand;
    int wheels;
};
```
