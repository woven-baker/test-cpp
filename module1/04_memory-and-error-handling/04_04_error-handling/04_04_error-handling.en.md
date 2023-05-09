---
Guidelines:
- Don't overuse try. In well-designed code, try blocks are rare.
- Whenever we define a function, we should consider its preconditions and whether or not to test them. Do the arguments to the function make sense? If not, what should we do?
- Formulating invariants helps us to understand what we want.
- There are a variety of approaches to error handling.

Error Codes:
- A failure is normal and expected (e.g. request to open a file).
- An immediate caller can reasonably be expected to handle the error.
- The system running the code has so little memory that exceptions are not feasible.
  
Exceptions:
- An error is rare enough that a programmer would likely forget to check for it.
- An error cannot be handled by an immediate caller.
- Allow new kinds of errors to be added at a lower level so that higher-level modules don't need to be written to handle them.
- No suitable return path for errors are available.
- The return path would be made more complicated by adding error codes.

Terminate:
- Cannot recover (e.g. memory exhaustion)
- We have some other software monitoring and controlling our program, and it is responsible for restarting it when non-trivial errors occur.
---

# Error Handling
Imagine you're writing a program that reads data from a file, processes it, and then writes the result to another file. What if the input file doesn't exist, or the output file can't be created due to insufficient permissions? What if the input file is corrupted? What if the data in the file isn't in a format your program expects, or there is data missing from one line? Without proper error handling, your program could crash or worse: silently produce incorrect results.

By handling errors in your code, you can ensure that your program behaves gracefully in the face of unexpected situations, providing helpful feedback to users and making it easier to debug and maintain.

## Return Codes
A simple way to handle errors is to use return codes. Functions can return a value indicating the success or failure of an operation. Let's look at an example:

```cpp
#include <iostream>
#include <fstream>

int readFile(const std::string& filename) {
    std::ifstream file(filename);
    
    if (!file.is_open()) {
        return -1;
    }

    // ...

    return 0;
}

int main() {
    int result = readFile("input.txt");
    
    if (result == -1) {
        std::cout << "Error: File not found" << std::endl;
    } else {
        std::cout << "File processed successfully" << std::endl;
    }

    return 0;
}
```

In this example, `readFile()` returns -1 if it fails to open the file and 0 if it succeeds. `main()` then checks the return value and logs an appropriate message.

We can do a bit better by replacing the magic number error codes with named constants:

```cpp
#include <iostream>
#include <fstream>

constexpr int success = 0;
constexpr int file_not_found = -1;

int readFile(const std::string& filename) {
    std::ifstream file(filename);
    
    if (!file.is_open()) {
        return file_not_found;
    }

    // ...

    return success;
}

int main() {
    int result = readFile("input.txt");
    
    if (result == file_not_found) {
        std::cout << "Error: File not found" << std::endl;
    } else {
        std::cout << "File processed successfully" << std::endl;
    }

    return 0;
}
```

This method is simple but has some drawbacks:

- The function can only return a single error code, making it difficult to convey detailed information about the error.
- The caller must remember to check the return value, or the error may not be handled.

## Exceptions
C++ provides a more powerful mechanism for error handling called exceptions. With exceptions, you can "throw" an error when something goes wrong and "catch" it elsewhere in your code to handle the problem.

### Throwing Exceptions
To throw an exception, use the throw keyword followed by an expression, like this:

```cpp
throw "File not found";
```

When an exception is thrown, the normal flow of execution is interrupted, and the program jumps to the nearest exception handler, if one exists, which catches the exception.

### Catching Exceptions
To catch an exception, use a try block followed by one or more catch blocks:

```cpp
try {
    // Code that might throw an exception
} catch (const char* msg) {
    // Handle the exception
    std::cerr << "Error: " << msg << std::endl;
}
```

Here's an example of how we might use exceptions to handle the file reading scenario described previously:

```cpp
#include <iostream>
#include <fstream>
#include <stdexcept>

void readFile(const std::string& filename) {
    std::ifstream file(filename);
    
    if (!file.is_open()) {
        throw std::runtime_error("File not found");
    }

    // Read and process the file
    // ...
}

int main() {
    try {
        readFile("input.txt");
        std::cout << "File processed successfully" << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
```

In this example, the `readFile()` function throws a `std::runtime_error` exception if it fails to open the file. The `main()` function then uses a try-catch block to handle the exception and output an appropriate message.

## Terminating
Sometimes, when a severe error occurs, it's better to terminate the program instead of attempting to handle the error and continue execution. This can be done using the `std::abort()` function from the `<cstdlib>` header.

```cpp
#include <cstdlib>

// ...

if (severe_error_occurred) {
    std::abort();
}
```

Keep in mind that terminating the program should be reserved for situations where recovery is impossible or undesirable.

## Invariants
Invariants are conditions that must always hold true during the execution of a program or within a specific scope. They are used to establish the correctness of your code by ensuring that certain assumptions are always valid. Invariants can help catch bugs early and make your code more robust and easier to reason about.

Here's an example of an invariant: In a sorted array, every element must be less than or equal to the element that comes after it. If this condition is violated, you can be sure that there is an error in your code.

## Assertions
An assertion is a statement that checks whether a specific condition holds true. If the condition is false, the program will terminate with an error message. Assertions are commonly used during development to ensure that assumptions and invariants hold true. They can help catch bugs early before they cause more significant issues.

In C++, you can use the `assert()` macro from the  `<cassert>` header to create assertions:

```cpp
#include <cassert>

// ...

int divide(int a, int b) {
    assert(b != 0); // Check for division by zero
    return a / b;
}
```

In this example, the `assert()` macro checks whether the divisor `b` is non-zero. If `b` is zero, the program will terminate with an error message.

Note that assertions should not be used for error handling in production code, as they can be disabled globally by defining the `NDEBUG` macro. Assertions are meant to be used during development to catch logic errors early.

## Static Assertions
Static assertions can be used in various contexts, not just with classes or templates. They can help enforce constraints on your code at compile time, making it more robust and easier to maintain. Let's look at an example without using classes or templates.

Imagine you are writing code that needs to run on a specific platform or has strict requirements for data types' sizes. You can use `static_assert` to check if those requirements are met at compile time.

```cpp
#include <cstdint>
#include <limits>

// Check if the platform uses the required integer sizes
static_assert(sizeof(int8_t) == 1, "int8_t must be 1 byte");
static_assert(sizeof(int16_t) == 2, "int16_t must be 2 bytes");
static_assert(sizeof(int32_t) == 4, "int32_t must be 4 bytes");
static_assert(sizeof(int64_t) == 8, "int64_t must be 8 bytes");

// Check if the platform supports a specific range of values for a type
static_assert(std::numeric_limits<float>::min() <= 1e-38, "float must support a minimum positive value of 1e-38");
static_assert(std::numeric_limits<float>::max() >= 1e+38, "float must support a maximum positive value of 1e+38");

int main() {
    // Your code here
    return 0;
}
```

In this example, we use `static_assert` to check if the platform's integer types meet specific size requirements. We also check if the float type supports a specific range of values. If any of these checks fail, the compilation will fail with the corresponding error message, alerting you to the issue before the code is executed.

Remember that `static_assert` can be a valuable tool for catching potential issues at compile time, ensuring that your code is more robust and easier to maintain.

# Exercises

## Exercise

Which of the following is NOT a method for handling errors in C++?

```
a) Return codes
b) Exceptions
c) Recursion
d) Assertions
```

---

```
c) Recursion
```

## Exercise 
Select all the correct statements about static assertions:

```
a) Static assertions are checked at runtime
b) Static assertions can help catch errors before the code is executed
c) static_assert can be used with or without templates and classes
d) The static_assert keyword is followed by a constant expression and an optional error message
```

---

```
b) Static assertions can help catch errors before the code is executed
c) static_assert can be used with or without templates and classes
d) The static_assert keyword is followed by a constant expression and an optional error message
```

## Exercise
To catch an exception, you need to use a ____ block followed by one or more ____ blocks.

```
```

---

```
To catch an exception, you need to use a try block followed by one or more catch blocks.
```

## Exercise
Write a line of code that throws a std::runtime_error exception with the message "File not found" when a file fails to open.

```cpp
throw std::runtime_error("File not found");
```

## Exercise
Describe the difference between runtime assertions and static assertions. Provide a use case for each.

```
```

---

```
Runtime assertions are used to check conditions at runtime, while static assertions are used to check conditions at compile time.

A use case for runtime assertions is to check for logic errors during development, such as ensuring a divisor is non-zero before performing division. This can help catch bugs early and prevent them from causing issues in the final code.

A use case for static assertions is to enforce constraints on the code at compile time, such as ensuring that certain data types have the correct sizes on a specific platform or that a template function is instantiated with the correct type. This can make the code more robust and easier to maintain.
```

## Exercise
When should you use `std::abort()` to terminate your program?

```
a) When a minor error occurs
b) When a function cannot return a meaningful value
c) When an exception is caught
d) When recovery is impossible or undesirable
```

---

```
d) When recovery is impossible or undesirable
```

## Exercise
Select all the correct statements about custom exception classes in C++:

```
a) Custom exception classes should derive from one of the standard exception classes, such as std::exception
b) Custom exception classes can provide more information about the error
c) Custom exception classes make it more difficult to handle specific errors
d) Custom exception classes can make your error handling code more specific
```

---

```
a) Custom exception classes should derive from one of the standard exception classes, such as std::exception
b) Custom exception classes can provide more information about the error
d) Custom exception classes can make your error handling code more specific
```

## Exercise
The ____ macro is used to create runtime assertions in C++.

```
```

---

```
The assert macro is used to create runtime assertions in C++.
```

## Exercise
Write a line of code that uses `static_assert` to check if the `sizeof(int)` is exactly 4 bytes. Provide an error message "int must be 4 bytes" if the condition is not met.

```
```

---

```cpp
static_assert(sizeof(int) == 4, "int must be 4 bytes");
```

## Exercise
What is an invariant, and why is it important for the correctness of your code?

```
```

---

```
An invariant is a condition that must always hold true during the execution of a program or within a specific scope. Invariants are important for the correctness of your code because they establish certain assumptions that are always valid. This helps to catch bugs early and makes your code more robust and easier to reason about.
```

## Exercise
Complete the following code snippet to catch a `std::runtime_error` exception and print the error message:

```cpp
#include <iostream>
#include <stdexcept>

int main() {
    try {
        // Some code that may throw a std::runtime_error
    }
    catch (____ e) {
        std::cout << ____ << std::endl;
    }
    return 0;
}
```

---

```cpp
catch (std::runtime_error e) {
    std::cout << e.what() << std::endl;
}
</details>
```

## Exercise
Complete the following code snippet to create a runtime assertion that checks if a pointer `p` is not `nullptr`:

```cpp
#include <______>

// ...

int* p = someFunction();
______(p != nullptr);
```

---

```cpp
#include <cassert>

// ...

int* p = someFunction();
assert(p != nullptr);
```

## Exercise
Complete the following code snippet to define a function `safe_divide` that throws a `std::runtime_error` exception with the message "Division by zero" when the divisor is zero:

```cpp
#include <stdexcept>

double safe_divide(double a, double b) {
    if (____) {
        ____ std::runtime_error("Division by zero");
    }
    return a / b;
}
```

---

```cpp
if (b == 0) {
    throw std::runtime_error("Division by zero");
}
```

## Exercise
Modify the following code snippet to use a `static_assert` that checks if sizeof(long) is at least 8 bytes. If the condition is not met, provide an error message "long must be at least 8 bytes".

```cpp
#include <iostream>
#include <cstdint>

int main() {
    long largeNumber = 9223372036854775807L;

    // TODO

    std::cout << "The value of largeNumber is: " << largeNumber << std::endl;
    return 0;
}
```

---

```cpp
#include <iostream>
#include <cstdint>

int main() {
    long largeNumber = 9223372036854775807L;

    static_assert(sizeof(long) >= 8, "long must be at least 8 bytes");

    std::cout << "The value of largeNumber is: " << largeNumber << std::endl;
    return 0;
}
```