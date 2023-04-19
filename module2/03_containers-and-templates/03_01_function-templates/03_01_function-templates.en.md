---
Concept: C++ Function Templates
Required:
  - C++ Functions
  - C++ Function Overloading
  - C++ Templates
---

# C++ Function Templates
At some point during development, we might find that we have a lot of code that looks identical with the exception of the data type on which we are operating:

```cpp
#include <iostream>
#include <string>

int addInts(int a, int b) {
    return a + b;
}

double addDoubles(double a, double b) {
    return a + b;
}

std::string concatenateStrings(std::string a, std::string b) {
    return a + b;
}

int main() {
    std::cout << "Adding ints: " << addInts(3, 5) << '\n';
    std::cout << "Adding doubles: " << addDoubles(3.5, 1.2) << '\n';
    std::cout << "Adding strings: " << concatenateStrings("Hello", "World") << '\n';

    return 0;
}
```

Each of the functions defined here have exactly the same logic. The only difference between them is the data types that are being added together.

All of this code duplication is a violation of the DRY principle, so there must be a way we can clean up this code.

In C++, function templates are a way to create reusable functions that can work with different data types. They allow you to define a single function that can be used with various types, without having to write a separate function for each type.

Our first step in using function templates is to create a new function with a good name that encompasses the common usage of all the functions we are cleaning up. Next, we remove all of the changing types in the function signature and replace them with a placeholder type.

If we do that for the previous code, we should have something like this:

```cpp
T add(T a, T b) {
    return a + b;
}
```

'add' is a perfectly good name for what all these functions do, and since the return type and the two argument types change and are all the same, I have used the placeholder type `T` in all of those places.

If you try and compile this code right now, you will encounter a compiler error because we have not provided a declaration for `T`.

The next step is to declare `T` using a template head:

```cpp
template<typename T>
T add(T a, T b) {
    return a + b;
}
```

Template heads are declared using the `template` keyword followed by a set of angle brackets `<>` containing one or more parameters. In this example, `T` is a template type parameter that represents a data type. The `typename` keyword is used here to indicate that T is a type. There are also [template non-type parameters and template template parameters](https://en.cppreference.com/w/cpp/language/template_parameters).

The last step is specifying the template parameter type when you call the function:

```cpp
int main() {
    std::cout << "Adding ints: " << add<int>(4, 5) << '\n';
    std::cout << "Adding doubles: " << add<double>(3.7, 1.2) << '\n';
    std::cout << "Adding strings: " << add<std::string>("Hello", "World") << '\n';

    return 0;
}
```

The output of the previous code is the following:

```
Adding ints: 9
Adding doubles: 4.9
Adding strings: HelloWorld
```

From this output you can see that we are able to write just one function that is able to accommodate many different types! The compiler does the work of generating a specific version of the function based on the data types you provide when you call it.

Let's go ahead and define another function template called `multiply` that returns the product of the two values passed to it and call this function with both int and double arguments:

```
```

***

```cpp
template<typename T>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    std::cout << "Multiplying ints: " << multiply<int>(5, 6) << '\n';
    std::cout << "Multiplying doubles: " << multiply<double>(3.9, 1.3) << '\n';
}
```

## Template argument deduction
Thanks to *[template argument deduction](https://en.cppreference.com/w/cpp/language/template_argument_deduction)*, when possible, the compiler can deduce on its own what types we are passing to a template. Because of this, we can often call function templates in the same way you would normal functions:

```cpp
template<typename T>
void log(T data) {
    std::cout << "Logging data: " << data << '\n';
}

int main() {
    int int_data = 3;
    log(int_data);

    double double_data = 3.14;
    log(double_data);
}
```

```cpp
template<typename T, typename U>
auto max(T x, U y) {
    return (x > y) ? x : y;
}

int main() {
    int int_a = 3;
    int int_b = 5;
    std::cout << max(int_a, int_b) << std::endl;

    int double_a = 3.14;
    int double_b = 5.25;
    std::cout << max(double_a, double_b) << std::endl;
}
```

## Function Template Specialization

Our function template for add simply concatenated the two strings together, which is exactly what we would expect from the + operator. However, what if we wanted to include a space between the two words instead?

To accomplish this, we can go ahead and define a *template specialization* of the add function, where we leave the template parameter declaration empty and specify the types explicitly:

```cpp
template <>
std::string add(std::string a, std::string b) {
    return a + " " + b;
}
```

Now when we compile and run the previous code again:

```cpp
int main() {
    std::cout << "Adding ints: " << add<int>(4, 5) << '\n';
    std::cout << "Adding doubles: " << add<double>(3.7, 1.2) << '\n';
    std::cout << "Adding strings: " << add<std::string>("Hello", "World") << '\n';

    return 0;
}
```

We get:

```
Adding ints: 9
Adding doubles: 4.9
Adding strings: Hello World
```

# Exercises

## Exercise 1
Consider the following function template:

```cpp
template <typename T>
T add(T a, T b) {
    return a + b;
}
```

Which of the following is a correct way to call `add` with two `float` arguments?

```cpp
a) add<float>(3.0, 4.0)
b) add(3.0f, 4.0f)
c) add<float, float>(3.0, 4.0)
d) add(3.0, 4.0f)
```

---

```
a) and b)
```

## Exercise 2
Write a C++ function template called min that accepts two arguments of the same type and returns the smallest value of the two.

```
```

***

```cpp
template<typename T>
T min(T a, T b) {
    return (a < b) ? a : b;
}

int main() {
    std::cout << "Minimum of ints: " << min<int>(3, 5) << '\n';
    std::cout << "Minimum of doubles: " << min<double>(3.5, 1.2) << '\n';

    return 0;
}
```

## Exercise 3
Write a function template called swap that swaps the values of two variables of the same type.

```
```

***

```cpp
template<typename T>
void swap(T &a, T &b) {
    T temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 5;
    int y = 10;
    std::cout << "Before swap: x = " << x << ", y = " << y << '\n';
    swap(x, y);
    std::cout << "After swap: x = " << x << ", y = " << y << '\n';

    return 0;
}
```

## Exercise 4
You have been given the following code:

```cpp
#include <iostream>
#include <vector>

template<typename T>
T sum(const std::vector<T> &data) {
    T result = 0;
    for (const auto &item : data) {
        result += item;
    }
    return result;
}

int main() {
    std::vector<int> intVec = {1, 2, 3, 4, 5};
    std::vector<double> doubleVec = {1.1, 2.2, 3.3, 4.4, 5.5};
    std::vector<std::string> stringVec = {"one", "two", "three"};

    std::cout << "Sum of intVec: " << sum(intVec) << '\n';
    std::cout << "Sum of doubleVec: " << sum(doubleVec) << '\n';
    std::cout << "Sum of stringVec: " << sum(stringVec) << '\n';

    return 0;
}
```

The code is meant to calculate the sum of elements in a vector using a function template. The code will compile as is, but there will be a segmentation fault when the code is executed. Identify the issue, fix it, and provide a working version of the code.

```
```

***

The issue with the code is that the sum function template is initialized with T result = 0;, which is only valid for numeric types. We can fix this by initializing result with the default value of the given type T:

```cpp
template<typename T>
T sum(const std::vector<T> &data) {
    T result = T();
    for (const auto &item : data) {
        result += item;
    }
    return result;
}
```

## Exercise 5
Evaluate the code below and explain if it's a good use case for function templates. If not, suggest a more appropriate alternative.

```cpp
#include <iostream>

template<typename T>
void print(T data) {
    std::cout << data << std::endl;
}

int main() {
    print<int>(42);
    print<double>(3.14);
    print<std::string>("Hello, World!");

    return 0;
}
```

```
```

***

```
Using function templates in this case is beneficial, as it allows you to create a reusable print function that can be applied to different data types without having to write separate implementations for each type. This adheres to the DRY principle and makes the code more maintainable.
```