---
Guidelines:
---

# Scope

Consider the following program:

```cpp
#include <iostream>

void printNum() {
    int num { 10 };
    std::cout << num << std::endl;
}

int main() {
    printNum();
    std::cout << num << std::endl;
    return 0;
}
```

When you try to compile and run this program, you'll encounter a compiler error.

Where?

---

`num` is defined and used successfully in the `printNum()` function, so why can't it be accessed in the `main()` function?

## What is Scope?
Scope refers to the region of code where a variable lives and can be accessed. There are two main types of scope:

1. **Local Scope**: A variable that is declared within a function or a block has local scope. It can only be used within that function or block. When execution leaves that function or block, the variable is destroyed and its memory is freed.

2. **Global Scope**: A variable that is declared outside all functions and blocks has global scope. It can be used throughout the lifetime of the program, from the point of declaration to the end of the program file.

So, in the previous code, `num` has what type of scope?

---

`num` has local scope within the `printNum()` function. It exists only within that function, which is why we get a compiler error.

## Global scope
Let's modify our program to illustrate the difference between local and global scope:

```cpp
#include <iostream>

constexpr int num { 10 };

void printNum() {
    std::cout << num << std::endl;
}

int main() {
    printNum();
    std::cout << num << std::endl;
    return 0;
}
```

Here we see `num` being defined in the global scope. It is accessible throughout the program in both `printNum()` and in `main()`.

You'll notice that `num` has been made into a constant. This is a good practice.

## Avoid non-constant global variables

You should avoid non-constant global variables.

Why?

```cpp
#include <iostream>

int mode;

void someFunction() {
    mode = 2;
}

void actionBreak() {
    // ...
}

void actionAccelerate() {
    // ...
}

int main() {
    mode = 1;

    someFunction();

    if (mode == 1) {
        actionBreak();
    } else {
        actionAccelerate();
    }
}
```

A programmer reading `main()` should reasonably expect `actionBreak()` to be called, since `mode` was assigned the value 1 just a few lines beforehand. However, `mode` is reassigned the value 2 in `someFunction()`. There is no obvious connection between `someFunction()` and `mode`. We would expect mode to be passed to the function by reference if it were to modify `mode`. Instead, it happens silently. 

In a large program, with hundreds of functions and thousands of lines of code, imagine trying to track down a bug involving a global variable that appears in 40 different functions.

## Shadowing

Consider the following program:

```cpp
#include <iostream>

int num = 20;

void printNum() {
    int num = 10;
    std::cout << "Local num: " << num << std::endl;
}

int main() {
    printNum();
    std::cout << "Global num: " << num << std::endl;
    return 0;
}
```

Does this code compile? And if so, what would you expect the output to be?

---

The program does compile, and its output is:

```
Local num: 10
Global num: 20
```

The `num` inside `printNum()` and the `num` in `main()` are different variables. In this case, we say that the local `num` *shadows* the `num` in the outer scope. The `num` inside `printNum()` is local to that function and is destroyed when the function finishes execution. The `num` used in `main()` is a global variable, and is accessible throughout the program.

Avoid shadowing. It is easy to get confused about which variable is used, and it makes maintenance of the code more difficult. If you do find yourself shadowing, it is a good indication that your functions are too large or complex.

## Nested Scopes
In C++, we can have nested scopes, i.e., a scope inside another scope. For instance, if we declare a new variable within a block `{}` inside a function, that variable will have a local scope within that block:

```cpp
#include <iostream>

int main() {
    int outer = 1;

    {
        int inner = 2;
        std::cout << "Outer: " << outer << std::endl;
        std::cout << "Inner: " << inner << std::endl;
    }

    std::cout << "Outer: " << outer << std::endl;
    std::cout << "Inner: " << inner << std::endl;

    return 0;
}
```

What happens when we run this code?

---

Here, we get a compiler error because `inner` is only accessible within the block where it is declared. Outside that block, `inner` is not accessible.

# Exercises

## Exercise 1

What will the following code print?

```cpp
#include <iostream>

int value = 10;

void printValue() {
    int value = 20;
    std::cout << value << std::endl;
}

int main() {
    printValue();
    std::cout << value << std::endl;
    return 0;
}
```

---

```
20
10
```

## Exercise 2

What is wrong with the following code?

```cpp
#include <iostream>

int value = 10;

void printValue() {
    int value = 20;
    std::cout << value << std::endl;
}

int main() {
    printValue();
    return 0;
}
```

```
A: It doesn't compile
B: The local variable value shadows the global variable value
C: The local variable value is not constant
D: The global variable value is not constant
```

---

```
B, D
```

## Exercise 3

What will be the output of the following program:

```cpp
#include <iostream>

void func() {
    int y = 20;
    std::cout << y << std::endl;
}

int main() {
    int y = 10;
    func();
    std::cout << y << std::endl;
}
```

```
A) 10
B) 20
C) 20
   10
D) 10
   20
```
---

```
C) 20
   10
```

## Exercise 4

What is the scope of the variable `i` in the following code?

```cpp
#include <iostream>

int main() {
    for (int i = 0; i < 10; ++i) {
        std::cout << i << std::endl;
    }
}
```

---

The scope of the variable `i` in the code is within the for loop. It is not accessible outside the for loop.