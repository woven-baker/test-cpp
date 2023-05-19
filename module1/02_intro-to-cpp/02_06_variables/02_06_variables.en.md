# Variables

## Introduction to Variables
We've already seen some variables before, and the data types they can hold. Let's review quickly.

A variable is a named area of memory where data can be stored and manipulated. Every variable has a name (the identifier) and a type, which defines what kind of data the variable can hold and what operations can be performed on it.

Here are some of the basic types of variables in C++:

- `int`: Used to store integer values.
- `float`: Used to store floating-point (decimal) numbers.
- `double`: Similar to `float` but with double the precision.
- `char`: Used to store a single character.
- `bool`: Used to store a Boolean value (`true` or `false`).

---
## Declaring and Initializing variables

In C++, we use the word 'declare' when we talk about creating a variable. When a variable is declared, some space is reserved in computer memory for the variable's value. However, it is important to know that when a variable is declared, it's value is not yet set. In fact, the reserved memory might already have a value left over from some previous usage.

After a variable is declared, we 'initialize' the variable by giving it a value. This is important, because using variables that have not been initialized can create bugs and errors.

> Don't worry, we can change the value of a variable even after we initialize it. However, if the variable uses the keyword `const`, we cannot change it after initializing. We learn more about `const`, later.

Here are some examples of how to declare and initialize variables of these types:

---
### Declaration without initialization

```cpp
int main() {
   int myInteger;
   float myFloat;
   double myDouble;
   char myChar;
   bool myBool;
}
```

In this case, the variables are declared, but they aren't initialized with any values. The values they hold are indeterminate and using them before assigning a value leads to **undefined behavior**.

> We will discuss undefined behavior later in this lesson.

---
### Declaration with initialization

```cpp
int main() {
   int myInteger { 10 };
   float myFloat { 20.5f };
   double myDouble { 30.15 };
   char myChar { 'A' };
   bool myBool { true };
}
```

Here, the variables are declared and initialized with a value at the same time.

If we want to initialize these built-in data types with 0, we can use empty braces:

```cpp
int main() {
   int myInteger {};
   float myFloat {};
   double myDouble {};
   char myChar {};
   bool myBool {};
}
```

---
### Uniform initialization (C++11 and later)

This whole time we have been initializing variables with curly braces `{}`. This is called **uniform initialization**.

Before C++11, programmers would just use the assignment operatorr `=`:

```cpp
int myInteger = 10;
float myFloat = 20.5f;
double myDouble = 30.15;
char myChar = 'A';
bool myBool = true;
```

Uniform initialization has several advantages over using the assignment operator, such as preventing narrowing conversions (i.e., conversions that can lead to data loss).

To illustrate this, consider the following code:

```cpp
#include <iostream>

int main() {
   int myInteger = 3.14;
   std::cout << myInteger << std::endl;
}
```

Go ahead and compile and run this program. The compiler will likely give a warning about the implicit conversion from `double` to `int`, because we are losing the decimal part of 3.14, but it will compile the program and let you run it.

This could very well have been a bug in our program. Either we shouldn't have been initializing `myInteger` to a double value, or we should have declared `myInteger` to be `double` instead of `int`.

Go ahead and compile and run the same program but using uniform initialization:

```cpp
#include <iostream>

int main() {
   int myInteger { 3.14 };
   std::cout << myInteger << std::endl;
}
```

Now the compiler gives us an error and prevents the program from compiling. This is usually what we want to have happen. This is type safety!

---

### Declaration first, then initialization

```cpp
int myInteger;
myInteger = 10;
```

In this case, the variable is declared first and then a value is assigned to it in a separate statement. While it may be fine in this small snippet, what if there's more code in between these two statements, and someone tries to use `myInteger` before it gets initialized? Undefined behavior. Always initialize variables when they are declared, wherever possible.

---
### Auto keyword (C++11 and later)

```cpp
auto myInteger { 10 };  // The compiler deduces that myInteger is an int
auto myFloat { 20.5f }; // The compiler deduces that myFloat is a float
```

The `auto` keyword tells the compiler to automatically deduce the type of the variable from its initializer.

---
## Undefined Behavior

Undefined behavior refers to parts of the C++ standard where the behavior of the program is not prescribed, meaning the compiler has no instructions on what to do in such cases. This can lead to unpredictable results, including crashes and random or incorrect program output. The worst case is when your program seems to produce normal results in most cases, but suddenly in certain situations, it might stop working correctly. These types of problems are extremely hard to find and fix.

Here are some examples of undefined behavior related to variables in C++:

1. **Using uninitialized variables**: In C++, local variables are not automatically initialized when you declare them (unless you explicitly initialize them). If you use the value of a variable before giving it a value, you get undefined behavior. (it might have a random value, or a value previously stored in that memory)

   ```cpp
   int myVariable;
   int anotherVariable = myVariable + 5;  // Undefined behavior
   ```

2. **Reading past the end of an array**: Arrays in C++ do not perform bounds checking. If you try to access an element past the end of an array, you get undefined behavior.

   ```cpp
   int myArray[5] = {1, 2, 3, 4, 5};
   int outOfBounds = myArray[5];  // Undefined behavior
   ```

3. **Dereferencing a null pointer**: If you have a pointer variable and you try to dereference it when it's null (when it doesn't hold a memory address), you get undefined behavior.

   ```cpp
   int *myPointer = nullptr;
   int badValue = *myPointer;  // Undefined behavior
   ```

4. **Modifying a const variable**: If you somehow manage to modify a variable that was declared as const, you get undefined behavior.

   ```cpp
   const int myConst = 5;
   int *badPointer = const_cast<int*>(&myConst);
   *badPointer = 10;  // Undefined behavior
   ```

5. **Overflow of signed integers**: In C++, the behavior is undefined if a signed integer (like `int`, `short`, `long`, etc.) overflows. The new value of the integer might come from a random place in computer memory, or it might reset the value.

   ```cpp
   int maxValue = INT_MAX;
   maxValue = maxValue + 1;  // Undefined behavior
   ```

Remember, undefined behavior can lead to bugs that are extremely difficult to diagnose and fix. Do your best to understand when undefined behavior can arise and avoid using the language in ways that cause undefined behavior.