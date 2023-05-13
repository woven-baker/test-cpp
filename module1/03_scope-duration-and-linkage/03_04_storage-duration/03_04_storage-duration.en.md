# Storage Duration
Storage duration refers to the lifetime of a variable or object in a C++ program. It determines the period during which the memory for variables remains allocated. The main types of storage durations in C++ are:

- Automatic Storage Duration
- Static Storage Duration
- Dynamic Storage Duration

Let's explore them one by one.

## Automatic Storage Duration
Variables with automatic storage duration are created at the point of definition and destroyed when the control flow leaves the scope in which they are defined. The most common examples are local variables and function parameters.

```cpp
void myFunction(int x) {
    int y { 5 + x };
    std::cout << y << std::endl;
}

int main() {
    int a;
    myFunction(a);
}
```

In this example, both `x` and `y` are created when `myFunction()` is called and destroyed when `myFunction()` ends. Similarly `a` is created when `main()` is called and destroyed when `main()` ends.

## Static Storage Duration
Variables with static storage duration are created when the program starts and destroyed when the program ends. They are initialized only once, and they are zero-initialized by default. These include global variables, static local variables, and static data members of classes. 

```cpp
#include <iostream>

constexpr int globalVar;

void myFunction() {
    static int staticLocalVar;
    std::cout << staticLocalVar << std::endl;
}

int main() {
    std::cout << globalVar << std::endl;
    myFunction();
}
```

What is the output of this program?

---

```
0
0
```

In this example, `globalVar` and `staticLocalVar` are created only once and they last until the end of the program. Because static variables have storage duration until program termination, `staticLocalVar` retains its value between calls to `myFunction()`.

Given that, what is the output of the following program:

```cpp
#include <iostream>

void myFunction() {
    static int staticLocalVar;
    std::cout << staticLocalVar++ << std::endl;
}

int main() {
    myFunction();
    myFunction();
    myFunction();
    myFunction();
    myFunction();
}
```

---

```
0
1
2
3
4
```

## Dynamic Storage Duration
Dynamic storage duration applies to variables that are allocated and deallocated manually, using `new` and `delete` (or `new[]` and `delete[]` for arrays). These variables exist until they are explicitly deallocated.

```cpp
#include <iostream>

int main() {
    int* dynamicVar = new int(5);
    std::cout << *dynamicVar << std::endl;
    delete dynamicVar;
}
```

In the above example, dynamicVar is a pointer to an integer allocated on the heap. The integer it points to has dynamic storage duration and exists until delete dynamicVar is executed.

# Exercises

## Exercise 1
Which of the following variables have automatic storage duration?

```cpp
#include <iostream>

constexpr int a { 10 };

void myFunction() {
    static int b { 20 };
    int c { 30 };
    int* d = new int(40);
}

int main() {
    myFunction();
}
```

```
A) a
B) b
C) c
D) d
```

---

```
D) d
```

`localVar` is the only variable with automatic storage duration in this example. It's created when `myFunction()` is called and destroyed when `myFunction()` ends.

## Exercise 2
What will the following code print?

```cpp
#include <iostream>

void myFunction() {
    static int count = 0;
    std::cout << ++count << std::endl;
}

int main() {
    myFunction();
    myFunction();
    myFunction();
    return 0;
}
```

```
A) 0 0 0
B) 1 1 1
C) 1 2 3
D) 3 3 3
```

---

```
C) 1 2 3
```

The variable `count` in `myFunction()` is a static local variable. It's created and initialized only once, and retains its value between calls to `myFunction()`. So it gets incremented each time `myFunction()` is called.

## Exercise 3
What is the storage duration of the variable `a` in the following program?

```cpp
int main() {
    int* a = new int;
    return 0;
}
```

```
A) Automatic
B) Static
C) Dynamic
```

---

```
D) Dynamic
```

The variable `a` is a pointer to an integer allocated on the heap using `new`. This integer has dynamic storage duration, and exists until it's explicitly deallocated. Please note that in the above example, we didn't deallocate the memory, which leads to a memory leak. To avoid it, `delete a;` should be called before the program ends.