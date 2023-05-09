---
Concept: C++ Abstract Classes
Required: [ C++ Functions, C++ Virtual Functions, C++ Object Oriented Programming, C++ Classes, C++ Inheritance ]
Notes: 
---

# Abstract Functions

So far, all of the virtual functions we have seen and written have a body (a definition). However, C++ allows you to create a special kind of virtual function called a `pure virtual function` (or `abstract function`) that has no body at all! A pure virtual function simply acts as a placeholder that is required to be redefined by derived classes.

To create a pure virtual function, rather than define a body for the function, we simply assign the function the value 0.

```cpp
class Sensor {
public:
    virtual ~Sensor() {}
    bool isActive() const { return active; }
    virtual float readData() = 0;

private:
    bool active = false;
};
```

Let's go ahead and try to create an instance of this Sensor class. What is the output of the following code:

```cpp
int main() {
    Sensor sensor;
    sensor.readData();
}
```

---

```bash
... error: variable type 'Sensor' is an abstract class                                           
    Sensor sensor;                                                                                               
           ^                                                                                                     
... note: unimplemented pure virtual method 'readData' in 'Sensor'                                
    virtual float readData() = 0;                                                                                
                  ^                                                                                              
1 error generated.
```

That's right, you cannot create an instance of a class with a pure virtual function. A class that defines a pure virtual function, or a class that inherits a pure virtual function and doesn't override it with a real definition, becomes an `abstract class`.

With that in mind, let's go ahead and define a couple of classes that inherit from Sensor and override the pure virtual function in the base Sensor class:

```cpp
#include <iostream>
#include <random>

class MockTemperatureSensor : public Sensor {
public:
    float readData() override {
        std::random_device random_device;
        std::mt19937 generator(random_device());
        std::uniform_real_distribution distribution(-40.0, 85.0);
        return distribution(generator);
    }
};

class MockLIDARSensor : public Sensor {
public:
    float readData() override {
        std::random_device random_device;
        std::mt19937 generator(random_device());
        std::uniform_real_distribution distribution(0.0, 100.0);
        return distribution(generator);
    }
};

int main() {
    MockTemperatureSensor temp_sensor;
    MockLIDARSensor lidar_sensor;

    std::cout << "Temperature: " << temp_sensor.readData() << "°C" << std::endl;
    std::cout << "LIDAR distance: " << lidar_sensor.readData() << " meters" << std::endl;
}
```

# Abstract Classes

Abstract classes are used to represent general concepts (for example, Sensor, Shape, Car), which can be used as base classes for concrete classes (for example, Sonar, Circle, Prius). Because abstract classes force derived classes to implement certain functions, they allow us to specify interfaces that can really improve the flexibility and maintainability of our code.

Take for instance the following function that handles our sensor data:

```cpp
float handleData(const MockTemperatureSensor& temperatureSensor) {
    ...

    float sensor_data = temperatureSensor.readData();

    ...

    // some business logic
    return result;
}

float handleData(const MockLIDARSensor& lidarSensor) {
    ...

    float sensor_data = lidarSensor.readData();

    // the same business logic
    ...
    return result;
}
```

What is wrong with this code?

```
A: Nothing
B: There is a lot of code duplication
C: You will need to overload handleData every time you create a new sensor type that inherits from Sensor
```

---

```
B, C
```

We could do better than the previous code. Given that these two overloads of `handleData` do the same thing, we should have an opportunity to clean up all of this duplicated code. Additionally, if `handleData` really is doing something generic that we could apply to data coming from any sensor, then we should prefer to write code at a higher abstraction level so that we don't care what sensor we are passing to `handleData`.

We could refactor this code into the following:

```cpp
float handleData(const Sensor& sensor) {
    ...

    float sensor_data = sensor.readData();

    // the same business logic
    ...
    return result;
}
```

# Exercises

## Exercise 1
Create an abstract class Shape with a pure virtual function `area()`. Derive Rectangle and Circle classes from Shape, and implement the Area() function for each. Calculate and display the area of different shapes using the derived classes.

---

```cpp
#include <iostream>
#include <cmath>

class Shape {
public:
    virtual ~Shape() {}
    virtual double area() const = 0;
};

class Rectangle : public Shape {
public:
    Rectangle(double width, double height) : width(width), height(height) {}

    double area() const override {
        return width * height;
    }

private:
    double width;
    double height;
};

class Circle : public Shape {
public:
    Circle(double radius) : radius(radius) {}

    double area() const override {
        return 3.14159265358979323846 * radius * radius;
    }

private:
    double radius;
};

int main() {
    Rectangle rectangle(5.0, 4.0);
    Circle circle(3.0);

    std::cout << "Rectangle area: " << rectangle.area() << std::endl;
    std::cout << "Circle area: " << circle.area() << std::endl;

    return 0;
}
```

## Exercise 2
***

Create an abstract class BankAccount with pure virtual functions deposit(), withdraw(), and getBalance(). Derive SavingsAccount and CheckingAccount classes from BankAccount, and implement the virtual functions for each. Simulate depositing, withdrawing, and checking the balance for both account types.

---

```cpp
#include <iostream>

class BankAccount {
public:
    virtual ~BankAccount() {}
    virtual void deposit(double amount) = 0;
    virtual bool withdraw(double amount) = 0;
    virtual double getBalance() const = 0;
};

class SavingsAccount : public BankAccount {
public:
    void deposit(double amount) override {
        balance += amount;
    }

    bool withdraw(double amount) override {
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }

    double getBalance() const override {
        return balance;
    }

private:
    double balance = 0.0;
};

class CheckingAccount : public BankAccount {
public:
    void deposit(double amount) override {
        balance += amount;
    }

    bool withdraw(double amount) override {
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }

    double getBalance() const override {
        return balance;
    }

private:
    double balance = 0.0;
};

int main() {
    SavingsAccount savings;
    CheckingAccount checking;

    savings.deposit(1000.0);
    checking.deposit(500.0);

    savings.withdraw(250.0);
    checking.withdraw(100.0);

    std::cout << "Savings account balance: " << savings.getBalance() << std::endl;
    std::cout << "Checking account balance: " << checking.getBalance() << std::endl;

    return 0;
}
```