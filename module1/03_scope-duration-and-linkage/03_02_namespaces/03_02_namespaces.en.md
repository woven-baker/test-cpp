# namespaces

Imagine you are programming a device with a motor and an LED:

**motor.h**
```cpp
#pragma once

void turnOn();
void turnOff();
```

**motor.cpp**
```cpp
#include "motor.h"

bool powered_on;

void turnOn() {
    powered_on = true;
}

void turnOff() {
    powered_on = false;
}
```

**led.h**
```cpp
#pragma once

void turnOn();
void turnOff();
```

**led.cpp**
```cpp
#include "led.h"

bool powered_on;

void turnOn() {
    powered_on = true;
}

void turnOff() {
    powered_on = false;
}
```

**main.cpp**
```cpp
#include "led.h"
#include "motor.h"

int main() {
    turnOn();
}
```

What happens when we compile this program:

```bash
g++ -o device motor.cpp led.cpp main.cpp
```

---

We should get output that looks like this (compiled on an Apple M1):

```
duplicate symbol 'turnOn()' in:
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/motor-08ebf7.o
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/led-71aca3.o
duplicate symbol 'turnOff()' in:
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/motor-08ebf7.o
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/led-71aca3.o
duplicate symbol '_powered_on' in:
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/motor-08ebf7.o
    /var/folders/3j/3h01f30d0yx5r54z556l0mym0000gq/T/led-71aca3.o
ld: 3 duplicate symbols for architecture arm64
```

Which part of the compilation process gave us this error?

```
A: Preprocessor
B: Compiler
C: Linker
```

---

```
C: Linker
```

The linker, when it goes to combine the `.o` object files for all three source files finds three duplicate symbols: `turnOn()`, `turnOff()`, and `powered_on`.

This is called a *naming collision*, or a *naming conflict*.

We could rename our functions and our variable so that they do not collide, which would be easy enough for a small program like this, but that would be annoying. However, for much larger projects, the odds of collisions increase significantly, and we can't be expected to constantly rename things in our code just to avoid these collisions.

We can use namespaces to solve this problem.

**motor.h**
```cpp
#pragma once

namespace motor {

void turnOn();
void turnOff();

}
```

**motor.cpp**
```cpp
#include "motor.h"

namespace motor {

bool powered_on;

void turnOn() {
    powered_on = true;
}

void turnOff() {
    powered_on = false;
}

}
```

**led.h**
```cpp
#pragma once

namespace led {

void turnOn();
void turnOff();

}
```

**led.cpp**
```cpp
#include "led.h"

namespace led {

bool powered_on;

void turnOn() {
    powered_on = true;
}

void turnOff() {
    powered_on = false;
}

}
```

All we do here is place our related code into a named namespace, where it is given its own scope in the body of the `{}` curly braces.

To call the led `turnOn()` function and the motor `turnOn()` function, we just need to prepend the name of the function with the name of the namespace, followed by the `::` scope resolution operator:

**main.cpp**
```cpp
#include "led.h"
#include "motor.h"

int main() {
    led::turnOn();
    motor::turnOn();
}
```

In fact, we have already seen this a lot without giving it much thought:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
}
```

The `std::` here in front of `cout` and `endl` tell us that these names are actually in the `std` namespace. In fact, all of the code found in the standard library headers, like `<iostream>`, `<random>`, `<string>`, `<chrono>`, etc. are in the `std` namespace.

# using

You may have seen the following code in some beginner learning material somewhere:

```cpp
#include <iostream>

using namespace std;

int main() {
    cout << "Hello, World!" << endl;
}
```

Notice the use of

```
using namespace std;
```

This is called a [using-directive](https://en.cppreference.com/w/cpp/language/namespace#Using-directives), and it is used to introduce all of the names from the specified namespace into the current scope.

By doing this, we can use `cout` and `endl` without the `std` prefix.

Be careful about using using-directives. In general, do not use them in the global scope (at the top of your file outside of any other scope), and do not use them in your header files.

Why?

If we added using-directives to introduce all of the names from both the `led` and `motor` namespaces into the top of our program, we would be causing the same problems that namespaces were designed to prevent:

**main.cpp**
```cpp
#include "led.h"
#include "motor.h"

using namespace led;
using namespace motor;

int main() {
    turnOn();
    turnOn();
}
```

# Exercises

## Exercise 1
Why are namespaces important?

```
A) They help avoid naming collisions.
B) They provide a way to group related code together.
C) They increase the execution speed of the code.
D) Both A) and B).
```

---

```
D) Both A) and B)
```

Namespaces in C++ are mainly used to avoid naming collisions and provide a way to group related code together. They do not directly increase the execution speed of the code.

## Exercise 2
Consider the following program:

```cpp
#include <iostream>

void hello() {
    std::cout << "Hello from global scope!" << std::endl;
}

namespace greeting {
    void hello() {
        std::cout << "Hello from 'greeting' namespace!" << std::endl;
    }
}

int main() {
    hello();
}
```

Which message will be displayed when this program is run?

```
A) "Hello from global scope!"
B) "Hello from 'greeting' namespace!"
C) Both A) and B)
D) The program will not compile due to a naming collision.
```

---

```
A) "Hello from global scope!"
```

In this code, calling `hello()` without a namespace qualifier from the `main()` function will call the global `hello()` function, not the `hello()` function in the `greeting` namespace. To call the `hello()` function in the `greeting` namespace, we would need to use the scope resolution operator (`::`) like so: `greeting::hello()`.

## Exercise 3
Consider the following program:

```cpp
namespace alpha {
    int value = 1;
}

namespace beta {
    int value = 2;
}

int main() {
    using namespace alpha;
    using namespace beta;
    std::cout << value << std::endl;
}
```

What will happen if you try to compile and run this code?

```
A) It will print 1.
B) It will print 2.
C) It will print 1 and 2.
D) It will cause a compilation error.
```

---
```
D) It will cause a compilation error.
```

In this case, both namespaces `alpha` and `beta` are introduced into the same scope, and they both have a variable named `value`. This causes a naming collision and the compiler cannot decide which value to use, leading to a compilation error.

## Exercise 4
Consider the following program:

```cpp
#include <iostream>

namespace first {
    int num = 5;
}

namespace second {
    double num = 3.14;
}

int main() {
    // TODO
}
```

What line of code would you write in `main()` to print the value of `num` from the `second` namespace?

---

```cpp
#include <iostream>

namespace first {
    int num = 5;
}

namespace second {
    double num = 3.14;
}

int main() {
    std::cout << second::num << std::endl;
}
```