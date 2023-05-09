---
Concept: C++ Variadic Templates
Required:
  - C++ Functions
  - C++ Templates
  - C++ Classes
  - C++ Function Templates
  - C++ Class Templates
  - C++ Recursion
---

# C++ Variadic Templates
Variadic templates are a feature in C++11 and later that allow you to define templates that take a variable number of template arguments. They can be used to create functions and classes that work with an arbitrary number of types or values. Variadic templates can greatly simplify code, making it more generic and reusable.

## Variadic Function Templates
Consider the following example of a variadic function template:

```cpp
#include <iostream>

void print() {
    std::cout << '\n';
}

template<typename T, typename... Args>
void print(const T& value, const Args&... args) {
    std::cout << value;
    if constexpr(sizeof...Args > 0) {
        std::cout << ", ";
    }
    print(args...);
}

int main() {
    print(1, 2.5, "Hello", 3, "World");
    return 0;
}
```

What is the output of this code?

---

```
1, 2.5, Hello, 3, World
```

This print function can print *any number* of values separated by a comma!

The `typename... Args` syntax means that Args (which could have been named anything) is a `template parameter pack` that can accept a variable number of type arguments. The `args...` syntax is used to expand the template parameter pack when recursively calling the print function.

To understand how the recursive call works, you can think of the print function template as a set of overloaded functions:

```cpp
void print();
void print(const int&);
void print(const int&, const double&);
void print(const int&, const double&, const char*);
// ... and so on.
```

The compiler generates these overloads as needed, depending on the arguments provided when calling the print function. In our example, the print function is called with the arguments (1, 2.5, "Hello", 3, "World"). 

This will generate the following sequence of calls:

```cpp
print(1, 2.5, "Hello", 3, "World")
print(2.5, "Hello", 3, "World")
print("Hello", 3, "World")
print(3, "World")
print("World")
print()
```

The base case of the recursion is the void print() function, which does not have any arguments and just prints a newline character.

Let's go ahead and modify this print function so that the output becomes:

```
(1) (2.5) (Hello) (3) (World)
```

---

```cpp
#include <iostream>

void print() {
    std::cout << '\n';
}

template<typename T, typename... Args>
void print(const T& value, const Args&... args) {
    std::cout << '(' << value << ')';
    if (sizeof...(Args) > 0) {
        std::cout << " ";
    }
    print(args...);
}

int main() {
    print(1, 2.5, "Hello", 3, "World");
    return 0;
}
```

## Variadic Class Templates

Variadic templates can also be used in class templates.

Let's say we want to create a simple event system where events can have multiple listeners with varying arguments. We can use a variadic class template to store the listeners and invoke them with any number of arguments:

```cpp
#include <iostream>
#include <vector>
#include <functional>

template<typename... Args>
class Event {
public:
    using Listener = std::function<void(Args...)>;

private:
    std::vector<Listener> listeners;

public:
    void addListener(const Listener& listener) {
        listeners.push_back(listener);
    }

    void notify(Args... args) {
        for (auto& listener : listeners) {
            listener(args...);
        }
    }
};

void onPlayerMoved(int x, int y) {
    std::cout << "Player moved to (" << x << ", " << y << ")" << std::endl;
}

void onPlayerJumped() {
    std::cout << "Player jumped" << std::endl;
}

int main() {
    Event<int, int> playerMovedEvent;
    playerMovedEvent.addListener(onPlayerMoved);

    Event<> playerJumpedEvent;
    playerJumpedEvent.addListener(onPlayerJumped);

    playerMovedEvent.notify(5, 3);
    playerJumpedEvent.notify();

    return 0;
}
```

# Exercises

## Exercise 1
What is the purpose of variadic templates in C++?

```
A. To create templates with a fixed number of arguments
B. To create templates with a variable number of arguments
C. To create specialized versions of templates for specific data types
D. To create class hierarchies and establish relationships between classes
```

---

```
B
```

## Exercise 2
Consider the following variadic function template:

```cpp
template<typename... Args>
void printArguments(Args... args) {
    // ...
}
```

Which of the following calls to `printArguments` are valid?

```
A. printArguments(1, 2, 3);
B. printArguments("hello", "world");
C. printArguments();
D. All of the above
```

---

```
D
```

## Exercise 3
Write a variadic function template called sum that takes any number of integer arguments and returns their sum. You can assume that the arguments are integers and the number of arguments is greater than 0.

---

```cpp
#include <iostream>

template<typename... Args>
int sum(int head, Args... args){
    if constexpr (sizeof...(args) == 0) {
        return head;
    } else {
        return head + sum(args...);
    }
}

int main() {
    std::cout << "Sum of 1, 2, 3: " << sum(1, 2, 3) << std::endl;
    std::cout << "Sum of 4, 5, 6, 7: " << sum(4, 5, 6, 7) << std::endl;
    std::cout << "Sum of 8: " << sum(8) << std::endl;

    return 0;
}
```

## Exercise 4
Create a variadic function template named allTrue that takes a variable number of boolean arguments and returns true if all arguments are true, and false otherwise. If no arguments are given, the function should return true.

Example usage:

```cpp
// Should output: 1 (true)
std::cout << allTrue(true, true, true) << std::endl;
// Should output: 0 (false)
std::cout << allTrue(true, false, true) << std::endl;
// Should output: 1 (true)
std::cout << allTrue() << std::endl;
```

---

```cpp
#include <iostream>

// Base case for the recursion
bool allTrue() {
    return true;
}

// Recursive variadic function template
template<typename... Args>
bool allTrue(bool head, Args... args) {
    return head && allTrue(args...);
}

int main() {
    std::cout << "All true: " << allTrue(true, true, true) << std::endl;
    std::cout << "Not all true: " << allTrue(true, false, true) << std::endl;
    std::cout << "No arguments: " << allTrue() << std::endl;

    return 0;
}
```