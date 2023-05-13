# Linkage
The concept of linkage in C++ is an important one to understand, especially when you're working with larger code bases or developing libraries. But what exactly is linkage, and why is it important?

Let's say you are working on a big project and you have a variable that needs to be shared across multiple files. For instance, let's say you're developing a game and you have a variable called `playerScore` that needs to be accessed and modified in several different parts of your game code.

You might try to declare playerScore in a header file like this:

**score.h**
```cpp
int playerScore;
```

And then include this header file wherever you need to use `playerScore`.

However, when you compile your project, you encounter multiple definition errors. This is because each source file that includes the header file will have its own copy of playerScore. When the linker tries to combine these separate object files into a single executable, it sees multiple definitions of playerScore and doesn't know which one to use.

That's where the concept of linkage comes in.

## What is Linkage?
Linkage in C++ determines the visibility and lifetime of an entity (variables, functions, etc.) across different translation units (i.e., source files). It's basically a set of rules that tells the compiler and linker how to handle a particular entity when it's used in multiple source files.

There are three types of linkage in C++:

- **No Linkage**: The entity is only visible within its own scope (i.e., where it was declared). For instance, local variables inside a function have no linkage.

- **Internal Linkage**: The entity is visible within its own translation unit (i.e., source file), but not in other translation units. Non-const global variables declared as static and functions declared as static have internal linkage.

- **External Linkage**: The entity is visible across all translation units. Global variables and functions by default have external linkage.

## External Linkage
To solve our `playerScore` problem, we want to give `playerScore` external linkage so that it can be shared across multiple source files.

The way to do this in C++ is to use the `extern` keyword. You would declare `playerScore` in a header file like this:

**score.h**
```cpp
extern int playerScore;
```

This tells the compiler that `playerScore` exists and will be defined somewhere else. Then, you would define `playerScore` in one source file:

**score.cpp**
```cpp
#include "score.h"
int playerScore = 0;
```

Now, any source file that includes `score.h` will have access to the same `playerScore` variable.

## Internal Linkage
Sometimes, you might want to limit the visibility of a global variable or function to just one source file. In that case, you can give the entity internal linkage using the `static` keyword.

For instance, let's say you have a helper function that's only used in one source file. To prevent this function from being visible to other source files, you could declare it as static:

**helpers.cpp**
```cpp
static void myHelperFunction() {
    // ...
}
```

Now, `myHelperFunction()` will only be visible within `helpers.cpp`.

Consider the following program:

**main.cpp**
```cpp
#include <iostream>

int num { 10 };

void printNum();

int main() {
    printNum();
    return 0;
}
```

**print.cpp**
```
#include <iostream>

extern int num;

void printNum() {
    std::cout << num << std::endl;
}
```

Compile the program using the following command:

```
g++ -o program main.cpp print.cpp
```

What do you expect the output to be when you run the program? Why?

---

You should expect the output to be 10. This is because we've declared `num` with external linkage in `main.cpp` and used the `extern` keyword in `print.cpp` to tell the compiler that `num` is defined in another file. So, when we call `printNum()` in `main.cpp`, it prints the value of `num` from `main.cpp`.

Understanding how linkage works in C++ can help you avoid multiple definition errors and organize your code more effectively. As you become more familiar with these concepts, you'll be better equipped to handle larger, more complex projects.

# Exercises

## Exercise 1
What is the linkage of a variable declared with the `static` keyword at file scope?

```
A) External linkage
B) Internal linkage
C) No linkage
```

---

```
B) Internal linkage
```

A variable declared with the `static` keyword at file scope (global scope) has internal linkage. This means it can only be accessed within the same translation unit (source file) where it is defined.

## Exercise 2
Consider the following code:

**file1.cpp**
```cpp
int num { 10 };
```

**file2.cpp**
```cpp
extern int num;
```

Is it possible to access the `num` variable from `file2.cpp`?

```
A) Yes
B) No
```

---

```
A) Yes
```

The `num` variable in `file1.cpp` is declared at file scope, which gives it external linkage by default. This means it can be accessed from other translation units. In `file2.cpp`, we declare `num` with the `extern` keyword, which tells the compiler that `num` is defined in another file. So, we can access `num` from `file2.cpp`.

## Exercise 3
What is the purpose of the `extern` keyword in C++?

```
A) It gives a variable internal linkage.
B) It gives a variable external linkage.
C) It tells the compiler that a variable is defined in another file.
```

---

```
C) It tells the compiler that a variable is defined in another file.
```

The `extern` keyword is used in C++ to declare a variable or function without defining it. It tells the compiler that the variable or function is defined in another file. This is useful when you want to use a variable or function across multiple files.

## Exercise 4
Consider the following program:

**file1.cpp**
```cpp
static int num { 10 };
```

**file2.cpp**

```cpp
extern int num;

void 
```

What will happen if you try to compile and link these two files together?

```
A) The program will compile and link without errors.
B) The program will compile but fail to link due to a multiple definition error.
C) The program will compile but fail to link due to an undefined reference error.
D) The program will not compile due to a syntax error.
```

vbnet
Copy code
C) The program will compile but fail to link due to an undefined reference error.
In this case, num in file1.cpp is declared with the static keyword, which gives it internal linkage. This means it can only be accessed within the same translation unit where it is defined. In file2.cpp, we attempt to declare num with the extern keyword, which tells the compiler that num is defined in another file. However, because num has internal linkage, it is not visible to file2.cpp, resulting in an undefined reference error at link time.

Linkage is a fundamental concept in C++ that determines the visibility and lifetime of variables and functions. By understanding how linkage works, you can write more efficient and organized code. Keep practicing and exploring these concepts to build your knowledge and skills. Happy coding!