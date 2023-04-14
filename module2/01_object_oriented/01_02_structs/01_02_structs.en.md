---
learning outcomes:
- Understand what structs are and why they are useful
- Learn how to define and declare structs
- Explore accessing and modifying struct members
- Discover the use of nested structs
---

# Structs

A struct is a user-defined data type that allows you to group related data together. It is particularly useful when you need to manage a set of variables that are related in some way, such as representing a point in 3D space, an RGB color, or a date.

## Defining a struct
To define a struct, you need to use the struct keyword followed by the name you want to give to the struct and then define its members within curly braces {}. Each member has a specific data type and name.

```cpp
struct Point {
  int x;
  int y;
};
```

In this example above, we've defined a Point struct with two integer members, x and y.

## Declaring a struct variable
Once you have defined a struct, you can declare variables of that struct type by using the struct name followed by the variable name:

```cpp
Point myPoint;
```

Note that this is exactly the same process as for the fundamental types such as `int` or `double`. When we define a struct, we are creating a new type. We can then use this type just as we would an `int` or `double`.

## Initialization
There are a couple of ways we can initialize a struct:

```cpp
Point point { 3, 4 };

Point point2 {
  .x = 3,
  .y = 4
};
```

The second initialization uses the `.` operator to explicitly name the members.

## Accessing and modifying struct members
To access or modify a struct member, you can use the dot `.` operator followed by the member's name:

```cpp
myPoint.x = 3;
myPoint.y = 5;

int sum = myPoint.x + myPoint.y;
```

## Nested structs
You can also define structs within structs, which is called nested structs. This can be useful when you need to create more complex data structures:

```cpp
struct Job {
  std::string_view title;
  int monthly_salary;
};

struct Person {
  std::string name;
  Job job;
};
```

## Organize related data

Take the following interface, which is supposed to draw a line from one point to another:

```cpp
void draw(int x, int y, int x2, int y2);
```

How could we use a struct to clean up this interface?

```
```

***

```cpp
void draw(Point from, Point to);
```

Not only have we reduced the number of parameters to this function, which makes our code cleaner, we have made clear the relationship of the arguments to each other by using a struct to bundle the two that belong together under a name.

Additionally, how could we introduce a struct to clean up the following code?

```cpp
#include <iostream>

void make_values(int &value1, int &value2, int &value3) {
    value1 = 1;
    value2 = 2;
    value3 = 3;
}

int main() {
    int a, b, c;
    make_values(a, b, c);
    std::cout << "Values: " << a << ", " << b << ", " << c << std::endl;
    return 0;
}
```

```
```

***

```cpp
#include <iostream>

struct Values {
    int value1;
    int value2;
    int value3;
};

Values make_values() {
    Values result;
    result.value1 = 1;
    result.value2 = 2;
    result.value3 = 3;
    return result;
}

int main() {
    Values values = make_values();
    std::cout << "Values: " << values.value1 << ", " << values.value2 << ", " << values.value3 << std::endl;
    return 0;
}
```

Notice here we use a struct to bundle several values together so that they can be returned as a single object from `make_values`. This is how programmers expect values to be returned from functions, not as out parameters.

# Exercises

## Exercise 1
Define a struct called Car that represents a car's make, model, year, and color.

```
```

***

```cpp
#include <iostream>
#include <string_view>

struct Car {
  std::string_view make;
  std::string_view model;
  int year;
  std::string_view color;
};
```

## Exercise 2
Create a function that takes a Car struct as a parameter and prints its details.

```
```

***

```cpp
void print_car(const Car& car) {
  std::cout << "Make: " << car.make << "\n";
  std::cout << "Model: " << car.model << "\n";
  std::cout << "Year: " << car.year << "\n";
  std::cout << "Color: " << car.color << "\n\n";
}
```

## Exercise 3
Declare an `std::array` of `Car` structs and initialize it with some sample data.

```
```

***

```cpp
#include <array>

int main() {
  std::array<Car> cars = { Car
    {"Toyota", "Camry", 2019, "Red"},
    {"Honda", "Civic", 2020, "Blue"},
    {"Ford", "Mustang", 2021, "Black"},
    {"Tesla", "Model 3", 2022, "White"}
  };
}
```

## Exercise 4
Use a loop to iterate over the array and call the function from step 2 to print the details of each car.

```
```

***

```cpp
#include <iostream>
#include <string_view>
#include <array>

struct Car {
  std::string_view make;
  std::string_view model;
  int year;
  std::string_view color;
};

void print_car(const Car& car) {
  std::cout << "Make: " << car.make << "\n";
  std::cout << "Model: " << car.model << "\n";
  std::cout << "Year: " << car.year << "\n";
  std::cout << "Color: " << car.color << "\n\n";
}

int main() {
  std::array<Car, 4> cars = {
    Car {"Toyota", "Camry", 2019, "Red"},
    Car {"Honda", "Civic", 2020, "Blue"},
    Car {"Ford", "Mustang", 2021, "Black"},
    Car {"Tesla", "Model 3", 2022, "White"}
  };

  for (auto& car : cars) {
    print_car(car);
  }

  return 0;
}
```