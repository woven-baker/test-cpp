---
Concept: C++ Class Templates
Required:
  - C++ Functions
  - C++ Templates
  - C++ Classes
  - C++ Object Oriented Programming
---

# C++ Class Templates
Just as function templates allow you to create reusable functions for different data types, class templates enable you to create reusable classes that can work with different types. Class templates are especially useful when you need to define a container or data structure that can store different types of data.

Consider the following example of a simple Stack class:

```cpp
#include <iostream>
#include <vector>

class IntStack {
private:
    std::vector<int> data;

public:
    void push(int value) {
        data.push_back(value);
    }

    int pop() {
        if (!data.empty()) {
            int value = data.back();
            data.pop_back();
            return value;
        } else {
            std::cerr << "Error: Stack is empty.\n";
            exit(EXIT_FAILURE);
        }
    }

    bool empty() const {
        return data.empty();
    }
};

int main() {
    IntStack stack;
    stack.push(1);
    stack.push(2);
    stack.push(3);

    while (!stack.empty()) {
        std::cout << stack.pop() << '\n';
    }

    return 0;
}
```

The IntStack class can only handle integers. If we want to create a similar stack for other data types, we would have to create separate classes for each type, which would result in a lot of code duplication.

Instead, we can create a class template for the Stack that works with any data type:

```cpp
#include <iostream>
#include <vector>

template<typename T>
class Stack {
private:
    std::vector<T> data;

public:
    void push(T value) {
        data.push_back(value);
    }

    T pop() {
        if (!data.empty()) {
            T value = data.back();
            data.pop_back();
            return value;
        } else {
            std::cerr << "Error: Stack is empty.\n";
            exit(EXIT_FAILURE);
        }
    }

    bool empty() const {
        return data.empty();
    }
};

int main() {
    Stack<int> intStack;
    intStack.push(1);
    intStack.push(2);
    intStack.push(3);

    while (!intStack.empty()) {
        std::cout << intStack.pop() << '\n';
    }

    Stack<double> doubleStack;
    doubleStack.push(1.1);
    doubleStack.push(2.2);
    doubleStack.push(3.3);

    while (!doubleStack.empty()) {
        std::cout << doubleStack.pop() << '\n';
    }

    Stack<std::string> stringStack;
    stringStack.push("Hello");
    stringStack.push("World");
    stringStack.push("!");

    while (!stringStack.empty()) {
        std::cout << stringStack.pop() << '\n';
    }

    return 0;
}
```

In this example, we replaced the `IntStack` class with a class template `Stack` that has a template parameter `T`. This allows us to create stack objects for different data types, such as `int`, `double`, and `std::string`, or our own user-defined types without having to write separate classes for each type.

## Class Template Specialization

There might be situations when you want to provide a specialized implementation for a specific data type while using a class template. In these cases, you can create a class template specialization.

Let's say we want to create a specialized version of the `Stack` class template for `bool` type, where we store the boolean values as bits to save memory. We can create a class template specialization like this:

```cpp
template <>
class Stack<bool> {
private:
    std::vector<unsigned char> data;
    size_t bitIndex;

public:
    Stack() : bitIndex(0) {}

    void push(bool value) {
        if (bitIndex == 0) {
            data.push_back(0);
        }

        if (value) {
            data.back() |= (1 << bitIndex);
        }

        bitIndex = (bitIndex + 1) % 8;
    }

    bool pop() {
        if (!data.empty()) {
            bool value = data.back() & (1 << (bitIndex - 1));
            bitIndex = (bitIndex + 7) % 8;

            if (bitIndex == 7) {
                data.pop_back();
            }

            return value;
        } else {
            std::cerr << "Error: Stack is empty. Cannot pop a value." << '\n';
            return false;
        }
    }
};
```

Now we have a specialized version of the Stack class template that is optimized for storing boolean values as bits. This version of the Stack class will be used when we create a stack of bool values:

```cpp
int main() {
    Stack<int> intStack;
    intStack.push(42);
    std::cout << intStack.pop() << std::endl;

    Stack<double> doubleStack;
    doubleStack.push(3.14);
    std::cout << doubleStack.pop() << std::endl;

    Stack<std::string> stringStack;
    stringStack.push("Hello");
    stringStack.push("World");
    std::cout << stringStack.pop() << std::endl;
    std::cout << stringStack.pop() << std::endl;

    Stack<bool> boolStack;
    boolStack.push(true);
    boolStack.push(false);
    std::cout << std::boolalpha << boolStack.pop() << std::endl;
    std::cout << std::boolalpha << boolStack.pop() << std::endl;

    return 0;
}
```

# Exercises

## Exercise 1
What is the purpose of using C++ class templates?

```
A. To create reusable classes that can work with different types
B. To create reusable functions for different data types
C. To create specialized classes for specific data types
D. To encapsulate data and provide a single interface for various implementations
```

***

```
A
```

## Exercise 2
Consider the following class template:

```cpp
template<typename T>
class Array {
private:
    T *data;
    size_t size;

public:
    Array(size_t s) : data(new T[s]), size(s) {}

    T& operator[](size_t index) {
        return data[index];
    }

    ~Array() {
        delete[] data;
    }
};
```

Which of the following code snippets will instantiate an Array object that can store 5 float values?

```cpp
A. Array<float> arr(5);
B. Array<float[5]> arr;
C. Array<float, 5> arr;
D. Array<float> arr[5];
```

***

```
A
```

## Exercise 3
Create a class template for a Pair class that can store two values of the same but any type. Implement a member function named print() that prints both values separated by a comma.

```
```

***

```cpp
#include <iostream>

template<typename T>
class Pair {
private:
    T first;
    T second;

public:
    Pair(T firstValue, T secondValue) : first(firstValue), second(secondValue) {}

    void print() const {
        std::cout << first << ", " << second << std::endl;
    }
};

int main() {
    Pair<int> intPair(1, 2);
    intPair.print();

    Pair<double> doublePair(3.14, 1.59);
    doublePair.print();

    Pair<std::string> stringPair("Hello", "World");
    stringPair.print();

    return 0;
}
```

## Exercise 4
Alter the previous class template for a Pair class so that it can store two values of any type, for example an integer and a string together. You'll need to research and apply `default template arguments` to answer this exercise.

```
```

***

```cpp
#include <iostream>

template<typename T, typename U = T>
class Pair {
private:
    T first;
    U second;

public:
    Pair(T firstValue, U secondValue) : first(firstValue), second(secondValue) {}

    void print() const {
        std::cout << first << ", " << second << std::endl;
    }
};

int main() {
    Pair<int> intPair{1, 2};
    intPair.print();

    Pair<double> doublePair{3.14, 1.59};
    doublePair.print();

    Pair<std::string> stringPair{"Hello", "World"};
    stringPair.print();

    Pair intStringPair{1, "Hello, world!"};
    intStringPair.print();

    return 0;
}
```

## Exercise 5
Consider the following class template for a Matrix class:

```cpp
template<typename T, size_t R, size_t C>
class Matrix {
private:
    T data[R][C];

public:
    T& at(size_t row, size_t col) {
        return data[row][col];
    }

    size_t rows() const {
        return R;
    }

    size_t cols() const {
        return C;
    }
};
```

The Matrix class template is using `non-type template parameters` for the number of rows (R) and columns (C). Research non-type template parameters and answer the following questions:

1. What are non-type template parameters and how do they differ from `type template parameters`?
2. Can non-type template parameters be of any data type? If not, what restrictions apply?

```
```

***

```
1. Non-type template parameters are template parameters that represent values rather than types. While type template parameters, like typename T, allow you to create classes or functions that work with different types, non-type template parameters allow you to create classes or functions that work with different constant values.

2. Non-type template parameters cannot be of any data type. They must be one of the following:

  - Integral or enumeration types (e.g., int, size_t, bool, or a user-defined enumeration).
  - Pointer types to objects or functions, including pointers to members.
  - std::nullptr_t (i.e., nullptr).
  - References to objects or functions.
  - std::integral_constant or similar types.

  It is important to note that non-type template parameters must be constant expressions, meaning their values must be known at compile-time.
```
