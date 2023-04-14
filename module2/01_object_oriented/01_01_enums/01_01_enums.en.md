---
- Silent implicit casting of enums to integers
- cluttered namespace (variables cannot have the same name as an enum, names within unrelated enums cannot be the same)
---

# Enum

Imagine we have the following code full of constants that we are trying to keep track of in our program:

```cpp
constexpr int red{ 0 };
constexpr int green{ 1 };
constexpr int blue{ 2 };

constexpr int small{ 0 };
constexpr int medium{ 1 };
constexpr int large{ 2 };

int main() {
    int carColor{ red };
    int carSize{ large };
}
```

We are using `constexpr`, we are not using all capitals for constant names, we are using braced initialization. Everything looks good.

However, the programmer working with this code will need to remember that `carColor` and `carSize` are meant to have only one of the values from their respective groups of constants.

We could employ `using` and create type aliases for `int` that make it more clear to the reader where the constants are coming from:

```cpp
using Color = int;
constexpr Color red{ 0 };
constexpr Color green{ 1 };
constexpr Color blue{ 2 };

using Size = int;
constexpr Size small{ 0 };
constexpr Size medium{ 1 };
constexpr Size large{ 2 };

int main() {
    Color car_color{ red };
    Size car_size{ large };
}
```

Nevertheless, the following code is still possible:

```cpp
Color car_color{ 10 };
Size shirt_size{ 5 };
shirt_size = 15;
shirt_color = large;
```

## Introducing Enum

Enum, short for "enumerations," are a way to define a type with a set of named integer constants. They provide a way to group related constants together, making it easier to understand and maintain your code. They also help catch the previous errors by restricting the possible values to a predefined set.

Let's replace the previous constants with `enum`s:

```cpp
enum Color { red, green, blue };
enum Size { small, medium, large };

int main() {
    Color car_color{ red };
    Size shirt_size{ large };

    Color shirt_color{ 10 };
    Size shirt_size{ 5 };
    shirt_size = 15;
    shirt_color = large;
}
```

What is the output from the previous code?

```
```

***

```cpp
enum.cpp:20:24: error: cannot initialize a variable of type 'Color' with an rvalue of type
      'int'
    Color shirt_color{ 10 };
                       ^~
enum.cpp:21:22: error: cannot initialize a variable of type 'Size' with an rvalue of type
      'int'
    Size shirt_size{ 5 };
                     ^
enum.cpp:22:18: error: assigning to 'Size' from incompatible type 'int'
    shirt_size = 15;
                 ^~
enum.cpp:23:19: error: assigning to 'Color' from incompatible type 'Size'
    shirt_color = large;
                  ^~~~~
4 errors generated.
```

We get four compiler errors from the statements that were previously allowed! Great!

### Scope
Let's assume that we don't have the same colors available for cars and shirts, so we want to split the previous `enum` into two:

```cpp
enum CarColor { red, green, blue };
enum ShirtColor { red, white, black, grey };
```

Unfortunately, if we try to compile this code, we get the following error:

```cpp
enum.cpp:2:19: error: redefinition of enumerator 'red'
enum ShirtColor { red, white, black, grey };
                  ^
enum.cpp:1:17: note: previous definition is here
enum CarColor { red, green, blue };
                ^
1 error generated.
```

What's happening here?

The problem is that the names within an enumeration exist in the same scope as the enumeration itself. Both `ShirtColor` and `CarColor` are defined in the global scope, so their names are also defined in the global scope, resulting in `red` being redefined, which is not allowed.

In addition to this problem, we can still get away with writing nonsensical code, such as the following:

```cpp
enum Size { small, medium, large };

int add(int a, int b) {
    return a + b;
}

int main() {
    Size shirt_size{ large };
    int result = add(3, shirt_size);
}
```

## Enum Classes

C++11 introduced a new feature called "enum classes" or "scoped enums." Enum classes provide better type safety and avoid name collisions because their constants are scoped within the enum class.

Let's replace the unscoped enums in the previous code with scoped enums:

```cpp
enum class CarColor { red, green, blue };
enum class ShirtColor { red, white, black, grey };
enum class Size { small, medium, large };

int add(int a, int b) {
    return a + b;
}

int main() {
    CarColor car_color{ CarColor::red };
    ShirtColor shirt_color{ ShirtColor::red }
    Size shirt_size{ Size::large };
    int result = add(3, shirt_size);
}
```

Notice that to use an enum class, you need to use the scope resolution operator `::` to access the constants.

### Underlying integer type

Enum classes also allow you to specify the underlying integer type for the enum, which can help save memory or match the size of another type:

```cpp
enum class Color : uint8_t { red, green, blue };
```

However, it is best to not specify an underlying type and use the default type unless you have a good reason not to, such as to forward declare a struct:

```cpp
enum class Color;       // not allowed
enum class Color : int; // allowed
```

# Exercises

## Exercise 1

Define an `enum class` called `Day` that represents the days of the week.

```

```

---

```cpp
enum class Day {
    sunday,
    monday,
    tuesday,
    wednesday,
    thursday,
    friday,
    saturday
};
```

## Exercise 2

Declare a variable of type `Day` and assign a value to it.

```

```

---

```cpp
Day today = Day::wednesday;
```

## Exercise 3

Write a function that takes a `Day` variable as a parameter and prints the name of the day using a `switch` statement.

```

```

---

```cpp
#include <iostream>

void printDay(Day day) {
    switch (day) {
        case Day::sunday:
            std::cout << "Sunday";
            break;
        case Day::monday:
            std::cout << "Monday";
            break;
        case Day::tuesday:
            std::cout << "Tuesday";
            break;
        case Day::wednesday:
            std::cout << "Wednesday";
            break;
        case Day::thursday:
            std::cout << "Thursday";
            break;
        case Day::friday:
            std::cout << "Friday";
            break;
        case Day::saturday:
            std::cout << "Saturday";
            break;
        default:
            std::cout << "Invalid day";
    }
}
```

## Exercise 4

Test your function with various days of the week.

```

```

---

```cpp
int main() {
    Day today = Day::wednesday;
    printDay(today);

    Day tomorrow = Day::thursday;
    printDay(tomorrow);

    Day yesterday = Day::tuesday;
    printDay(yesterday);

    return 0;
}
```
