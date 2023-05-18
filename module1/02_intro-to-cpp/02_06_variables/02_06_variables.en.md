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

Here are some examples of how to declare and initialize variables of these types:

---
### Declaration without initialization

```cpp
int myInteger;
float myFloat;
double myDouble;
char myChar;
bool myBool;
```

In this case, the variables are declared, but they aren't given a value. The values they hold are indeterminate and using them before assigning a value leads to **undefined behavior**.

>We will discuss undefined behavior later in this lesson.

---
### Declaration with initialization

```cpp
int myInteger = 10;
float myFloat = 20.5f;
double myDouble = 30.15;
char myChar = 'A';
bool myBool = true;
```

Here, the variables are declared and given a value at the same time.

---
### Uniform initialization (C++11 and later)

```cpp
int myInteger{10};
float myFloat{20.5f};
double myDouble{30.15};
char myChar{'A'};
bool myBool{true};
```

This form of initialization, introduced in C++11, has several advantages, such as preventing narrowing conversions (i.e., conversions that can lead to data loss).

---
### Declaration first, then initialization

```cpp
int myInteger;
myInteger = 10;
```

In this case, the variable is declared first and then a value is assigned to it in a separate statement.

---
### Auto keyword (C++11 and later)

```cpp
auto myInteger = 10;  // The compiler deduces that myInteger is an int
auto myFloat = 20.5f; // The compiler deduces that myFloat is a float
```

The `auto` keyword tells the compiler to automatically deduce the type of the variable from its initializer.

---
## Undefined Behavior

Undefined behavior refers to parts of the C++ standard where the behavior of the program is not prescribed, meaning the compiler has no instructions on what to do in such cases. This can lead to unpredictable results, including crashes and random or incorrect program output. The worst case is when your program seems to produce normal results in most cases, but suddenly in certain situations, it might stop working correctly. These types of problems are extremely hard to find and fix.

Here is an analogy using a real world example: Imagine you send your friend to buy some ice cream for you. The only instruction you give to your friend is: buy ice cream. Your instruction is specific, but doesn't have any details. What flavor should your friend look for? If your friend can't find that flavor, what should he do? How much money should he spend? When your friend arrives at the shop, he will buy the first ice cream flavor that he sees. This is dangerous because it might be a flavor that you hate, or it might be a seasonal flavor that costs a lot of money. In this case, your friend's actions analagous to **undefined behavior** in C++, because you didn't define the subsequent actions he should take. You only defined the first action, "buy ice cream".

Because you can't predict undefined behavior, it is particularly dangerous.

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

>Note: These examples show undefined behavior for variables, but undefined behvaior can affect other things in C++, as well, such as loop statements and arrays.

Remember, undefined behavior can lead to bugs that are extremely difficult to diagnose and fix, because the bugs might not be apparent and they can vary between compilers or even different executions of the program. Sometimes, a program's source code won't have any undefined behavior problems, but using the compiler's optimization options can **create undefined behavior conditions** in the executable file. This is one reason why testing the executable is important, to check the compiler's optimizations.