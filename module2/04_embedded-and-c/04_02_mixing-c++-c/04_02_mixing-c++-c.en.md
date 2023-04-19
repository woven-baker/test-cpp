---
Concept: Mixing C++ and C
Required:
  - C++ Functions
  - C++ Classes
  - C++ References
  - C++ Pointers
  - C++ Structs
  - C Functions
  - C Structs
  - C++ Header Files
  - C++ Compilation
  - C++ Linking
---

# Mixing C++ and C

In embedded software development, you'll often find the need to interface C++ code with existing C code.

## Name Mangling

Name mangling (also known as name decoration) is a technique used by compilers to generate unique names for each function and variable in a program. This is done to support function overloading in languages like C++ where multiple functions can have the same name but different argument types.

However, C does not support function overloading and, as a result, does not need name mangling. This leads to different naming schemes being used by C and C++ compilers.

Suppose we have the following C++ function:

```cpp
// math.cpp
int add(int a, int b) {
    return a + b;
}
```

When the C++ compiler compiles this code, it mangles the function name to include information about the argument types. The mangled name might look something like this: `_Z3addii`. This name encodes the fact that the function `add` takes two `int` arguments.

Now, consider the equivalent C function:

```c
// math.c
int add(int a, int b) {
    return a + b;
}
```

The C compiler does not need to mangle the function name, as C does not support function overloading. The compiled function will have the same name: `add`.

## Inspecting Mangled Names
You can inspect the mangled C++ names and unmangled C names in your program by examining the generated object files with a tool like `nm` (available on Unix-like systems). `nm` is a command-line utility that displays the symbol table of an object file, which includes the names of functions and variables.

Let's use `nm` to inspect the names:

First, compile your C and C++ files into object files.

For the C file, use:

```bash
gcc -c math.c -o math_c.o
```

For the C++ file, use:

```bash
g++ -c math.cpp -o math_cpp.o
```

Then, use `nm` to inspect the object files.

For the C object file, use:

```bash
nm math_c.o
```

This will output something like the following (the exact output may vary):

```bash
0000000000000000 T add
```

The T indicates that `add` is a function with global (external) linkage. The unmangled name add is shown.

For the C++ object file, use:

```bash
nm math_cpp.o
```

This will output something like the following (the exact output may vary):

```bash
0000000000000000 T _Z3addii
```

The T indicates that _Z3addii is a function with global (external) linkage. The mangled C++ name _Z3addii is shown.

Keep in mind that the mangled names are compiler-specific and can differ across different compilers and compiler versions. However, the purpose of mangling remains the same: to generate unique names for functions and variables that can be used during the linking process.

## Introduction to Extern "C"

This difference in naming schemes leads to linkage problems when trying to use C functions in C++ code or vice versa. The C++ code expects a mangled name, but the C function has a non-mangled name. To resolve this issue, we use `extern "C"` to tell the C++ compiler that the specified functions should use the C naming scheme and not mangle the name. This allows the C++ code to link correctly with the C functions.

To use `extern "C"`, we can either

1. prepend it to a C function declaration **from within a cpp file**:

```cpp
// cpp file
extern "C" void function_a(int);
```

2. wrap one or more C function declarations **from within a cpp file**:

```cpp
// cpp file
extern "C" {
    int function_b(double);
    double function_c();
};
```

3. wrap an included header **from within a cpp file**:

```cpp
// cpp file
extern "C" {
    #include "my_c_header.h"
}
```

4. wrap the function declarations **from within the c header**:

```c
// c header
#ifndef MY_C_HEADER_H
#define MY_C_HEADER_H

// other included header files

#ifdef __cplusplus
extern "C" {
#endif

// function declarations

#ifdef __cplusplus
}
#endif

#endif // MY_C_HEADER_H
```

This last solution is the preferred solution.

Why?

```
```

***

Because adding the `extern "C"` logic inside the C header makes it easier for C++ users to #include it into their C++ code without needing modifications on their end, and the header will behave the same everywhere it is included. Since a C compiler won’t understand the `extern "C"` construct, the `extern "C" {` and `}` lines must be wrapped with an `#ifdef` so they will only be seen by C++ compilers and not by normal C compilers.

If we have access to but are not able to modify the C header file, we should use:

```
A. 1
B. 2
C. 3
D. 4
```

```
```

***

```
C
```

If we do not have access to the C header file, we should use:

```
A. 1
B. 2
C. 3
D. 4
```

```
```

***

```
A or B
```

If we have access to and are able to modify the C header file, we should use:

```
A. 1
B. 2
C. 3
D. 4
```

```
```

***

```
D
```

From this point forward, unless otherwise stated, assume that you have access to and are able to modify the C header file. This is the typical situation.

## Function Interfacing
Let's go through an example of interfacing with a C function from within a C++ program.

Suppose we have the following C function defined in a file called `math_c.c`:

```c
// math_c.c
int math_add(int a, int b) {
    return a + b;
}
```

Now, let's create a header file called `math_c.h` to declare this C function:

```c
// math_c.h
#ifndef MATH_C_H
#define MATH_C_H

int math_add(int a, int b);

#endif // MATH_C_H
```

To use this C function in a C++ file, we'll wrap the function declaration in extern "C":

```cpp
// math_c.h
#ifndef MATH_C_H
#define MATH_C_H

#ifdef __cplusplus
extern "C" {
#endif

int math_add(int a, int b);

#ifdef __cplusplus
}
#endif

#endif // MATH_C_H
```

Now, we can use the add function in a C++ file:

```cpp
// main.cpp
#include <iostream>
#include "math_c.h"

int main() {
    int sum = math_add(3, 4);
    std::cout << "Sum: " << sum << std::endl;
    return 0;
}
```

Let's practice this by creating another translation unit (header and source file) in C called `log_c.h` and `log_c.c` that contains the declaration and definition, respectively, of a `log_print` function that returns void and takes a single integer argument and prints it using `printf`. Wrap the declaration so that it can be called from within the C++ program, call `log_print` from within the main function above, passing in `sum` as the argument.

```
```

***

```c
// log_c.c
#include <stdio.h>

void log_print(int a) {
    printf("%i\n", a);
}
```

```c
// log_c.h
#ifndef LOG_C_H
#define LOG_C_H

#ifdef __cplusplus
extern "C" {
#endif

void log_print(int a);

#ifdef __cplusplus
}
#endif

#endif // LOG_C_H
```

```cpp
// main.cpp
#include <iostream>
#include "math_c.h"
#include "log_c.h"

int main() {
    int sum = math_add(3, 4);
    std::cout << "Sum: " << sum << std::endl;
    log_print(sum);
    return 0;
}
```

## Passing Data from C++ Containers to C Code
C++ containers like `std::array`, `std::vector`, and others are not directly compatible with C functions. However, you can pass the underlying data from these containers to C code by obtaining pointers to their data. Let's go through some examples to demonstrate this process.

### std::array
`std::array` is a fixed-size container that stores its elements in a contiguous block of memory. You can obtain a pointer to the underlying data using the `data()` member function.

Suppose we have the following C function to print an array of integers:

```c
// print_array.c
#include <stddef.h>
#include <stdio.h>

void print_array(const int *arr, size_t size) {
    for (size_t i = 0; i < size; ++i) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}
```

We can use this C function to print the data of an `std::array` from our C++ code:

```cpp
// main.cpp
#include <array>
#include "print_array.h"

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    print_array(arr.data(), arr.size());

    return 0;
}
```

This code works, but the C `print_array` function uses an anti-pattern that we should try to avoid. What is this anti-pattern?

```
```

---

Separate arguments for a pointer and its size is an anti-pattern and should be avoided. We can do better by using [std::span](https://en.cppreference.com/w/cpp/container/span) (C++ 20) or equivalent on both the C++ side and the C side with accompanying conversion functions that map between the two and provide getters for the pointer and the size.

# Exercises

# Exercise 1
Let's consider an embedded system that controls a simple LED light. The LED can be turned on and off using an embedded C library. We'll create a C++ class that utilizes this C library to control the LED light.

## Embedded C Library
First, let's create an embedded C library that simulates the LED control.

led_controller.c:

```c
#include "led_controller.h"

static bool led_state = false;

void led_init(void) {
    led_state = false;
}

void led_on(void) {
    led_state = true;
}

void led_off(void) {
    led_state = false;
}

bool led_get_state(void) {
    return led_state;
}
```

led_controller.h:

```c
#ifndef LED_CONTROLLER_H
#define LED_CONTROLLER_H

#include <stdbool.h>

void led_init(void);
void led_on(void);
void led_off(void);
bool led_get_state(void);

#endif // LED_CONTROLLER_H
```

Let's first create a static library from the above led_controller code.

First we compile our led_controller.c file using the -c flag, which tells the compiler to compile our `led_controller.c` into an object file without performing linking:

```bash
gcc -c -Wall -Werror -Wextra led_controller.c
```

This will give us the `led_controller.o` object file.

Next, we create a static library using the `ar` command, passing it the object files we would like to include in our library:

```bash
ar -rcs libledcontroller.a led_controller.o
```

This will create the `libledcontroller.a` static library that we can link into our C++ program later.

Once we have written our C++ program, our final step will be to compile our C++ program and link our `libledcontroller.a` static library:

```bash
g++ -o main -L. -lledcontroller main.cpp led_controller.cpp
```

The `-L` flag specifies a path containing the libraries. The `-l` flag specifies the name of the library, omitting the leading 'lib' and trailing '.a'.

## Exercise 2: Wrap the function declarations
Wrap the LED controller C library function declarations using `extern "C"`.

```
```

***

led_controller.h:

```c
#ifndef LED_CONTROLLER_H
#define LED_CONTROLLER_H

#include <stdbool.h>

#ifdef __cplusplus
extern "C" {
#endif

void led_init(void);
void led_on(void);
void led_off(void);
bool led_get_state(void);

#ifdef __cplusplus
}
#endif

#endif // LED_CONTROLLER_H
```

## Exercise 3: Create a C++ LED Controller Class
Create a C++ class called LedController that utilizes the C library to control the LED light. Implement member functions to initialize, turn on, turn off, and get the LED state.

```
```

***

led_controller.hpp:

```cpp
#pragma once

#include "led_controller.h"

class LedController {
public:
    LedController();
    void on();
    void off();
    bool getState();
};
```

led_controller.cpp:

```cpp
#include "led_controller.hpp"

LedController::LedController() {
    led_init();
}

void LedController::on() {
    led_on();
}

void LedController::off() {
    led_off();
}

bool LedController::getState() {
    return led_get_state();
}
```

## Exercise 4: Use the C++ LED Controller Class in a Program
Create a C++ program that uses the LedController class to control the LED light. Turn on the LED, print its state, turn off the LED, and print its state again.

```
```

***

main.cpp:

```cpp
#include <iostream>
#include "led_controller.hpp"

int main() {
    LedController led;

    led.on();
    std::cout << "LED state: " << (led.getState() ? "ON" : "OFF") << std::endl;

    led.off();
    std::cout << "LED state: " << (led.getState() ? "ON" : "OFF") << std::endl;

    return 0;
}
```

## Exercise 5

Let's assume we have the following C header file, and we want to call `double_array` from within our C++ program:

```c
// double_array.h
#ifndef DOUBLE_ARRAY_H
#define DOUBLE_ARRAY_H

#include <stddef.h>

void double_array(int *arr, size_t size);

#endif // DOUBLE_ARRAY_H
```

Wrap the `double_array` declaration within the C header file.

```
```

***

```c
// double_array.h
#ifndef DOUBLE_ARRAY_H
#define DOUBLE_ARRAY_H

#include <stddef.h>

#ifdef __cplusplus
extern "C" {
#endif

void double_array(int *arr, size_t size);

#ifdef __cplusplus
}
#endif

#endif // DOUBLE_ARRAY_H
```

## Exercise 6

Use `double_array` from within a C++ program by calling it on the underlying data from a `std::array`.

```
```

***

```cpp
#include <array>
#include <iostream>
#include "double_array.h"

void print(const auto& array) {
    for (const auto& i : array) {
        std::cout << i << " ";
    }
    std::cout << '\n';
}

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    std::cout << "Original array: ";
    print(arr);

    double_array(arr.data(), arr.size());

    std::cout << "Modified array: ";
    print(arr);

    return 0;
}
```