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

You may be thinking that this doesn't appear to improve the code much, if at all. You would be correct.

One thing exceptions do for us here, is allow us to provide more information about the error when the error happens, which is usually at a lower level, instead of delegating that responsibility to whatever code ultimately handles the error, which is usually at a higher level.

Notice in the exception case, we let `main()` catch the error and print its contents without needing to care about what the error message ("File not found") was. Detailing the error became the responsibility of `readFile()`, which makes more sense.

In general we want to use exceptions in the following scenarios:
- An error occurs infrequently, making it easy for a programmer to overlook checking for it.
- An error is not manageable by the immediate function caller.
- There is no appropriate way to return error information.
- Introducing error codes would complicate the return path of a function.
- We want to enable the addition of new error types in lower-level modules without requiring modifications to higher-level modules.

Which of these scenarios apply to the previous example?

---

None of these scenarios really apply. So we should feel fine about using return codes here.

Let's take a look at another example:

```cpp
#include <fstream>
#include <iostream>
#include <string>

using ErrorCode = int;
constexpr ErrorCode error_none = 0;
constexpr ErrorCode error_file = -1;
constexpr ErrorCode error_invalid_input = -2;
constexpr ErrorCode error_division_by_zero = -3;

ErrorCode read_int(std::istream& in, int& value) {
    if (!(in >> value)) {
        return error_invalid_input;
    }
    return error_none;
}

ErrorCode divide(int a, int b, float& result) {
    if (b == 0) {
        return error_division_by_zero;
    }
    result = static_cast<float>(a) / static_cast<float>(b);
    return error_none;
}

ErrorCode process_data(const std::string& filename, float& result) {
    std::ifstream file(filename);

    if (!file.is_open()) {
        return error_file;
    }

    int a, b;
    ErrorCode err;

    err = read_int(file, a);
    if (err != error_none) {
        return err;
    }

    err = read_int(file, b);
    if (err != error_none) {
        return err;
    }

    return divide(a, b, result);
}

int main() {
    std::string filename = "input.txt";
    float result;

    ErrorCode err = process_data(filename, result);

    if (err == error_none) {
        std::cout << "The result is " << result << std::endl;
    } else {
        std::cerr << "Error: ";

        switch (err) {
            case error_file:
                std::cerr << "Cannot open file " << filename;
                break;
            case error_invalid_input:
                std::cerr << "Invalid input format";
                break;
            case error_division_by_zero:
                std::cerr << "Division by zero";
                break;
            default:
                std::cerr << "Unknown error";
        }
        std::cerr << std::endl;

        return 1;
    }

    return 0;
}
```

Compare this to a refactored version, where we replace return codes with exceptions:

```cpp
#include <exception>
#include <fstream>
#include <iostream>
#include <stdexcept>
#include <string>

int read_int(std::istream& in) {
    int value;
    if (!(in >> value)) {
        throw std::runtime_error("Invalid input format");
    }   
    return value;
}

float divide(int a, int b) {
    if (b == 0) {
        throw std::runtime_error("Division by zero");
    }   
    return static_cast<float>(a) / static_cast<float>(b);
}

float process_data(const std::string& filename) {
    std::ifstream file(filename);

    if (!file.is_open()) {
        throw std::runtime_error("Cannot open file " + filename);
    }   

    int a { read_int(file) };
    int b { read_int(file) };

    return divide(a, b); 
}

int main() {
    try {
        std::string filename = "input.txt";
        float result { process_data(filename) };
        std::cout << "The result is " << result << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }   

    return 0;
}
```

Which code is cleaner? Easier to understand? Easier to modify? Easier to maintain? Easier to extend?

---

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

What would be an example of such a situation?

---

One example is when the system has run out of memory, there is nothing the program can do to recover.

## Invariants
Invariants are conditions that must always hold true during the execution of a program or within a specific scope. They are used to establish the correctness of your code by ensuring that certain assumptions are always valid. Invariants can help catch bugs early and make your code more robust and easier to reason about.

We used an invariant within `divide()` in the previous code:

```cpp
float divide(int a, int b) {
    if (b == 0) {
        throw std::runtime_error("Division by zero");
    }   
    return static_cast<float>(a) / static_cast<float>(b);
}
```

As long as the divisor is not zero, we would expect this function to behave properly, so we maintain the invariant by throwing an exception when the invariant does not hold.

Another example of an invariant would be in a sorted array, where every element must be less than or equal to the element that comes after it. If this condition is violated, you can be sure that there is an error in your code.

Every time we create a function, we should evaluate its preconditions and decide whether to verify them. For the majority of applications, checking basic invariants is a good approach.

## Assertions
An assertion is a statement that checks whether a specific condition holds true. If the condition is false, the program will terminate with an error message. Assertions are commonly used during development to ensure that assumptions and invariants hold true. They can help catch bugs early before they cause more significant issues.

In C++, you can use the `assert()` macro from the  `<cassert>` header to create assertions:

```cpp
#include <cassert>

float divide(int a, int b) {
    assert(b != 0);
    return static_cast<float>(a) / static_cast<float>(b);
}

int main() {
    divide(3, 0);
}
```

Note that assertions should not be used for error handling in production code, as they can be disabled globally by defining the `NDEBUG` macro:

```bash
g++ -DNDEBUG -std=c++20 -o divide divide.cpp
```

## Static Assertions
Static assertions can help enforce constraints on your code at compile time.

For example, say you are writing code that needs to run on a specific platform or has strict requirements for the size of different data types. You can use `static_assert` to check if those requirements are met at compile time:

```cpp
#include <cstdint>
#include <limits>

static_assert(sizeof(int8_t) == 1, "int8_t must be 1 byte");
static_assert(sizeof(int16_t) == 2, "int16_t must be 2 bytes");
static_assert(sizeof(int32_t) == 4, "int32_t must be 4 bytes");
static_assert(sizeof(int64_t) == 8, "int64_t must be 8 bytes");

static_assert(std::numeric_limits<float>::min() <= 1e-38, "float must support a minimum positive value of 1e-38");
static_assert(std::numeric_limits<float>::max() >= 1e+38, "float must support a maximum positive value of 1e+38");

int main() {
    // ...
    return 0;
}
```

If any of these checks fail, the compilation will fail with the corresponding error message, alerting you to the issue before the code is executed.

# Exercises

## Exercise 1

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

## Exercise 2
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

## Exercise 3
To catch an exception, you need to use a ____ block followed by one or more ____ blocks.

---

```
To catch an exception, you need to use a try block followed by one or more catch blocks.
```

## Exercise 4
Write a line of code that throws a std::runtime_error exception with the message "File not found" when a file fails to open.

```cpp
throw std::runtime_error("File not found");
```

## Exercise 5
Describe the difference between runtime assertions and static assertions. Provide a use case for each.

---

```
Runtime assertions are used to check conditions at runtime, while static assertions are used to check conditions at compile time.

A use case for runtime assertions is to check for logic errors during development, such as ensuring a divisor is non-zero before performing division. This can help catch bugs early and prevent them from causing issues in the final code.

A use case for static assertions is to enforce constraints on the code at compile time, such as ensuring that certain data types have the correct sizes on a specific platform or that a template function is instantiated with the correct type. This can make the code more robust and easier to maintain.
```

## Exercise 6
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

## Exercise 7
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

## Exercise 8
The ____ macro is used to create runtime assertions in C++.

---

```
The assert macro is used to create runtime assertions in C++.
```

## Exercise 9
Write a line of code that uses `static_assert` to check if the `sizeof(int)` is exactly 4 bytes. Provide an error message "int must be 4 bytes" if the condition is not met.

---

```cpp
static_assert(sizeof(int) == 4, "int must be 4 bytes");
```

## Exercise 10
What is an invariant, and why is it important for the correctness of your code?

---

```
An invariant is a condition that must always hold true during the execution of a program or within a specific scope. Invariants are important for the correctness of your code because they establish certain assumptions that are always valid. This helps to catch bugs early and makes your code more robust and easier to reason about.
```

## Exercise 11
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

## Exercise 12
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

## Exercise 13
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

## Exercise 14
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