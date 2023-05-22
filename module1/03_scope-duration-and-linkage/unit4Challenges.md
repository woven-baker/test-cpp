# Challenges

## Challenge 1

Consider the following code:

**temp_sensor.h**
```cpp
namespace Sensor {
    float readTemperature();
}
```

**temp_sensor.cpp**
```cpp
#include "temp_sensor.h"

float readTemperature() {
    return 25.5;
}
```

**main.cpp**
```cpp
#include <iostream>
#include "temp_sensor.h"

int main() {
    std::cout << "Temperature reading: " << readTemperature() << std::endl;
    return 0;
}
```

Compile and run this program. Fix the code that is causing this error. You should only be adding code to fix the code, not removing code.

---

The issue in this code is related to namespace usage. The function `readTemperature()` is declared inside the `Sensor` namespace in `temp_sensor.h`, but it's defined in the global namespace in `temp_sensor.cpp`. Similarly, in `main.cpp`, it's used as if it's in the global namespace.

The correct code should be:

**temp_sensor.cpp**
```cpp
#include "temp_sensor.h"

namespace Sensor {
    float readTemperature() {
        return 25.5;
    }
}
```

**main.cpp**
```cpp
#include <iostream>
#include "temp_sensor.h"

int main() {
    std::cout << "Temperature reading: " << Sensor::readTemperature() << std::endl;
    return 0;
}
```

## Challenge 2
There is a fourth type of storage duration since C++11. What is it? How would we properly declare an `int` variable `thread_data` such that it has this storage duration, and initialize it with the value 10?

---

The fourth type of storage duration is thread storage duration. Thread storage duration applies to variables that are declared with the `thread_local` specifier. These variables are created when a thread begins and destroyed when it ends. Each thread has its own instance of these variables.

```cpp
#include <thread>

thread_local int thread_data { 10 };
```

## Challenge 3

Create a C++ program that includes a header file `math_operations.h` which contains function declarations for basic mathematical operations (`addition`, `subtraction`, `multiplication`, and `division`) in a namespace `MathOps`. Implement these functions in a separate file `math_operations.cpp`. Use these functions in a main program file `main.cpp`. The program should prompt the user for two numbers and the operation to be performed, then display the result.

### Example 1
input:
```
Enter a calculation: 4 / 5
```
output:
```
Result: 0.8
```

### Example 2
input:
```
Enter a calculation: 24 - 6
```
output:
```
Result: 18
```

---

**math_operations.h:**
```cpp
namespace MathOps {
    int add(int a, int b);
    int subtract(int a, int b);
    int multiply(int a, int b);
    double divide(int a, int b);
}
```

**math_operations.cpp:**
```cpp
#include "math_operations.h"

namespace MathOps {
    int add(int a, int b) {
        return a + b;
    }

    int subtract(int a, int b) {
        return a - b;
    }

    int multiply(int a, int b) {
        return a * b;
    }

    double divide(int a, int b) {
        return double(a) / double(b);
    }
}
```

**main.cpp:**
```cpp
#include <iostream>
#include "math_operations.h"

int main() {
    int a {};
    int b {};
    char operation {};

    for (;;) {
        std::cout << "Enter a calculation: ";
        std::cin >> a >> operation >> b;

        if (operation == '+') {
            std::cout << "Result: " << MathOps::add(a, b) << std::endl;
        } else if (operation == '-') {
            std::cout << "Result: " << MathOps::subtract(a, b) << std::endl;
        } else if (operation == '*') {
            std::cout << "Result: " << MathOps::multiply(a, b) << std::endl;
        } else if (operation == '/') {
            std::cout << "Result: " << MathOps::divide(a, b) << std::endl;
        } else {
            std::cout << "Invalid operation." << std::endl;
        }
    }

    return 0;
}
```