# Type Safety


## What is type safety?
Type safety is a concept in programming languages that prevents or minimizes errors related to data types. A programming language is said to be type-safe if it ensures that an operation is working on the correct type of data.

In a type-safe language, operations on data are checked for type compatibility, and type mismatches are detected and handled, often by raising errors. This prevents bugs related to incorrect data types, making the code safer and more reliable.


Because we always have to label our data with data types, this helps the compiler use the correct instructions and operations in every situation. This prevents many types of errors that happen in other programming languages that don't use strict data types, such as Python.

For example, in Javascript, I can use `+` to add two numbers or combine two strings. I can use `/` to divide two numbers, but if I use `/` on two strings an error will occur. However, this error **only happens when the program is executed**. This means that I might not notice the error, and the user (or customer) of my program will have a problem when they try to use my program. In C++, the compiler will catch my error before my finished program goes to the customer. Type safety is very useful.


---
## Strong Typing
C++ is a strongly typed language, meaning that every variable and expression has a known type, and every type is checked at compile time. If you try to assign data of the wrong type to a variable, the compiler will usually give an error. For example, you can't assign a string to an integer variable.

    ```cpp
    int myVar = "Hello";  // This will give a compile error in C++
    ```

---
## Type Conversion
Although C++ is strongly typed, it does allow for type conversion (this is called: type **casting**), where you can manually change a value from one type to another. There are safe ways to do this, such as `static_cast`, which performs compile-time type checking, but there are also unsafe ways, like `reinterpret_cast`, which can lead to type safety issues if misused.

Let's practice. You can try compiling and executing the following code. What do you think the program will print in the terminal?
```cpp

#include <iostream>

int main() {

double myDouble = 3.14;
int myInt;

myInt = static_cast<int>(myDouble);

std::cout << "The number is: " << myInt << std::endl;

return 0
}
```

---
```
The number is: 3
```

---
## Implicit Type Conversion
C++ also allows for implicit type conversion, where the compiler automatically changes one type to another if it thinks it's safe. For example, you can assign an integer to a double variable, and the compiler will automatically convert the integer to a double. However, this can sometimes lead to unexpected results, such as loss of precision or data, so it's important to be aware of when these conversions are happening.

    ```cpp
    int myInt = 10;
    double myDouble = myInt;  // This is fine, the int is implicitly converted to a double
    ```

---
## Be careful with Pointers
C++ allows you to work directly with memory addresses through pointers, which can lead to type safety issues. For example, you can have a pointer to an integer, and then mistakenly use it as a pointer to a character. This can lead to unexpected behavior and bugs.


**Let's review**
Sure, here are three multiple choice questions about type safety:

What does it mean for a language to be type-safe?
    - A. It means the language allows operations on any type of data.
    - B. It means the language checks for type compatibility in operations and prevents or minimizes type-related errors.
    - C. It means the language doesn't allow for any type conversions.
    - D. It means the language can automatically correct type-related errors.

---
```
B. It means the language checks for type compatibility in operations and prevents or minimizes type-related errors.
```

In C++, which of the following will cause a compile-time error due to type mismatch?
    - A. `int myVar = "Hello";`
    - B. `double myDouble = 10;`
    - C. `float myFloat = 10.5756;`
    - D. `char myChar = 'A';`

---
```
A. `int myVar = "Hello";`
```


Which type casting in C++ performs compile-time type checking, making it a safer option for type conversion?
    - A. `dynamic_cast`
    - B. `static_cast`
    - C. `reinterpret_cast`
    - D. `const_cast`

---
```
B. `static_cast`
```

---
In general, C++ provides mechanisms for type safety, but it also gives programmers a lot of flexibility, which can be both powerful and dangerous. It's possible to write very type-safe code in C++, but it's also possible to circumvent the type system and write code that has type safety issues. As a C++ programmer, it's important to understand the type system and use it correctly to write safe and reliable code.