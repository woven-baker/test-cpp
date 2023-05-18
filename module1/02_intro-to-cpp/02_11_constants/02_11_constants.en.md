# Constants

## What are constants?
In C++, a constant is a value that cannot be altered after it has been initialized. Declaring a constant allows you to assign a value to a variable name and then ensure that this value cannot be changed throughout the code.

Two main advantages of using constants:
1. Preventing accidental modification of values that you don't want to change
   - For example, if you program was doing math with circles. The number pi never changes (it is always 3.14159), so you can make it a constant
2. Sometimes, the compiler can optimize your program by storing constant values in special read-only memory, etc.


## Defining Constants
You define a constant in C++ with the `const` keyword, followed by the data type, the name of the constant, and the value you want to assign. Here's an example:

```cpp
const int myConstant = 10;
```

In this example, `myConstant` is an integer constant with a value of 10. If you try to change the value of `myConstant` later in your code, the compiler will give an error.

---
## Constant Expressions
Now, let's talk about `constexpr`, which is a keyword introduced in C++11. A `constexpr` specifies that the value of a variable or the return value of a function can appear in constant expressions, meaning that the compiler can evaluate them at compile time.

Here's how you would declare a `constexpr`:

```cpp
constexpr double pi = 3.141592653589793;
```

And here's an example of a `constexpr` function:

```cpp
constexpr int square(int x) {
    return x * x;
}
```

The `square` function can be used in a constant expression, like this:

```cpp
constexpr int a = square(10);  // a is now 100
```

Here, `a` is a constant that gets its value from a `constexpr` function. Since `square(10)` can be evaluated at compile time, `a` can be a `constexpr`.

>Note that a `constexpr` function can only contain a single `return` statement, and it can only call other `constexpr` functions. If a `constexpr` function or variable cannot be evaluated at compile time, it behaves like a regular function or variable, but it's always good practice to design `constexpr` functions and variables with compile-time evaluation in mind.

`const` and `constexpr` in C++ allow you to define constants, but `constexpr` has an advantage of enabling more computations to be performed at compile time, which can sometimes lead to more efficient code.