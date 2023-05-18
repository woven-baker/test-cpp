# Basic Data Types

---

## What are Data Types?

In C++, everything has a type. You can think of a type like a label. The compiler uses these labels to organize memory and use the appropriate CPU instructions when the machine code is produced.

Here is an example of a variable that holds an integer:

```cpp
//This variable represents the number of wards in Tokyo.
int tokyoWards = 32
```

> A variable is memory that can hold a value, such as an integer in the previous example. You can give a variable any name, as long as it doesn't have the same name as a C++ keyword. We will study variables more deeply, soon.

Let's look at some of the basic data types that C++ uses.

---
## Integers

Integers are fundamental data types used to store numeric values. The four primary integer types in C++ are `int`, `short`, `long`, and `long long`. Each of these types can be either signed or unsigned, providing a range of options to balance between memory usage and the size of numbers that can be represented.

>Integers are whole numbers. `1,2,3,4` are examples of integers. Numbers such as `24.7` or `357.76` are **not** integers. (These are **floating-point** numbers, and C++ can use them, too. We will learn this later.)

Signed integers can store both positive and negative numbers, including zero. This can be useful for measuring things such as temperature, or a bank account balance, which both can have negative values.

Unsigned integers can only store positive numbers and zero. This can be useful when dealing with quantities that can never be negative, such as sizes, lengths, or counts.

In most situations, the default `int` type is sufficient. However, if you need to handle very large numbers or ensure that a number is always positive, you might choose to use a `long`, `long long`, or an unsigned integer type. When defining an integer, you should consider the range of values you need to represent and choose the smallest type that can handle those values, as this can help optimize memory usage. For instance, if you're writing a program that counts the number of times a loop executes, and you know the loop will never execute more than a few thousand times, a `short` would be a more memory-efficient choice than an `int` or `long`.


Here are the integer types we can use in C++, and some examples:

1. `short`: This type is usually used when the range of values that a variable may hold is small. For instance, if you're counting the number of days in a week.

    ```cpp
    short daysInWeek = 7;
    ```

2. `int`: This is the most commonly used integer type. It's typically used for numeric operations and loop counters. For example, you might use an `int` to store a person's age.

    ```cpp
    int age = 30;
    ```

3. `long`: This type is often used when the range of values that an `int` can hold is insufficient. For example, it can be used to store the population of a city.

    ```cpp
    long cityPopulation = 20000000; // 20 million
    ```

4. `long long`: This type is used when the range of values that a `long` can hold is insufficient. It can handle very large numbers. For instance, you could use a `long long` to store the number of stars in the universe.

    ```cpp
    long long starsInUniverse = 1000000000000000000LL; // 1 quintillion
    ```


By default, Integer types are signed. This means they can be positive or negative. If you are certain that a number must always be postive, you can use the `unsigned` keyword, like this:

```cpp
unsigned long lightSpeed = 299792458; // Speed of light in meters per second
```

In most cases, it is recommended to use the default versions of integer types (signed). When performing mathematical operations on unsigned integers in C++, many types of special errors can occur that are dangerous. This course won't introduce all of these special errors (because there are many cases where they occur), but it is recommended to research them on your own time.

> Remember that the exact range of values that can be represented by these types can vary depending on the system and compiler. For example, an `int` on a certain Linux system might be smaller or larger than `int` on a Windows or Mac computer.


---
## Float

C++ has two types that handle floating-point numbers. `float` is one of those types.

We use the `float` keyword to declare floating-point variables.

```cpp
float pi = 3.14159
float metersInMile = 1609.34
```

> Floats are **not** recommended when you need high accuracy with the numbers. For example, in scientific or mathematics situations.

---
## Double

`float` numbers have limits to their accuracy and precision. `float` numbers are only accurate until around 5-7 decimals places. If you need more precise and accurate numbers, you should use a `double`.

We use the `double` keyword to declare high-precision floating-point numbers.
```cpp
double veryBigNumber = 3.14159265358979
```

`double` is the preferred choice when higher precision is required, while `float` can be used when memory consumption needs to be minimized, or when a lower precision is acceptable for the given application.

---
## Characters

The char data type is used to store a single character in C++. A 'character' here can be any printable symbol from the ASCII table, which includes letters of the alphabet (both uppercase and lowercase), digits from 0 to 9, punctuation symbols, etc.

>ASCII is a standardized collection of alphabet characters and special characters and instructions for how computers should represent them in binary numbers. For many years, most computer systems used ASCII in America, but other countries sometimes use other character sets. For example, in the past, Japan has used Shift-JIS. Nowadays, many applications use UTF-8, which combines characters from almost all other character sets into a huge, single collection.

The keyword char is used to declare character type variables. For example:

```cpp
char myChar = 'A';
```

> `char` is used for **single characters**. To make words, you need multiple characters. In those cases, we use a `string`

Lastly, remember that characters are written within single quotes ('A'), while strings are written within double quotes ("Hello"). This is a common source of confusion for those new to the language.

---
## Strings

A string in C++ is essentially a sequence of characters. It is a derived data type, composed of an array of the `char` data type. The array of characters is terminated by a special character known as the null character (`\0`).

In the earliest versions of C++, to declare a string, you would typically use an array of characters, like this:

```cpp
char myString[6] = "Hello";
```

In this example, `myString` is an array that can hold up to 6 characters. We've stored the word `"Hello"` in it, which only needs 5 characters. However, we need to reserve an additional space for the null character (`\0`) which is automatically added by the compiler to the end of the string to indicate its termination.

While you can still use this approach, it's a bit low-level and doesn't offer the flexibility and functionality of the `std::string` class that's now available in C++. This class, which is part of the C++ Standard Library, provides a higher-level, easier-to-use way to work with strings.

> The C++ Standard Library is a collection of tools that any programmer can use. If you have access to a C++ compiler, you can use anything in the Standard Library.

Here's an example of declaring a string using the `std::string` class:

```cpp
std::string myString = "Hello";
```

With `std::string`, you don't need to worry about the size of the string in advance or the null termination character.

The `std::string` class also provides various functions that you can use to manipulate strings. For example, you can append to a string using the `+=` operator or the `append()` member function, find the length of a string using the `length()` or `size()` member function, and so on. 

> Functions provided by a class are called **member functions**

Here's an example of using some of these member functions:

```cpp
std::string myString = "Hello";
myString += " World";
std::cout << myString << std::endl;  // Outputs: Hello World
std::cout << myString.length() << std::endl;  // Outputs: 11
```

You can work with strings in C++ at a low level by dealing with them as arrays of characters. However, in most cases, it's much easier and more efficient to use the `std::string` class provided by the C++ Standard Library.

---
## Bool

The `bool` type in C++ is a built-in data type that represents boolean values: `true` and `false`. These are often used to control program flow in conditional statements and loops.

For example, you might use a `bool` in an `if` statement like this:

```cpp
bool isRaining = true;

if (isRaining) {
    std::cout << "Don't forget your umbrella!" << std::endl;
} else {
    std::cout << "It's a nice day outside." << std::endl;
}
```

In this example, if `isRaining` is `true`, the program will print "Don't forget your umbrella!", and if `isRaining` is `false`, it will print "It's a nice day outside."

Boolean expressions are also frequently used with relational operators (`==`, `!=`, `<`, `>`, `<=`, `>=`) and logical operators (`&&`, `||`, `!`). For example:

```cpp
int x = 5;
int y = 10;
bool isXLessThanY = x < y; // Evaluates to true because 5 is less than 10
```

In this example, `isXLessThanY` is a `bool` that holds the result of the expression `x < y`. Do you think it is `true` or `false`?

---
`isXLessThanY` will evaluate to `true` because `x < y` is `5 < 10`, and 5 is less than 10. 

>The variables or statements that are on the left side and right side of the operators, are called **operands**. In the previous example, the two operands are `x` and `y`.

Here is a quick reference list for boolean operators in C++:

1. Logical Operators:
    - `&&` : Logical AND. Returns `true` if both operands are `true`.
    - `||` : Logical OR. Returns `true` if either or both operands are `true`.
    - `!`  : Logical NOT. Returns `true` if the operand is `false`, and `false` if it is `true`.


2. Relational Operators:
    - `==` : Equal to. Returns `true` if both operands are equal.
    - `!=` : Not equal to. Returns `true` if operands are not equal.
    - `>`  : Greater than. Returns `true` if the left operand is greater than the right operand.
    - `<`  : Less than. Returns `true` if the left operand is less than the right operand.
    - `>=` : Greater than or equal to. Returns `true` if the left operand is greater than or equal to the right operand.
    - `<=` : Less than or equal to. Returns `true` if the left operand is less than or equal to the right operand.


The `bool` type is very useful in C++ programming for controlling the logic of your program. It's a fundamental part of conditional statements, loops, and more.


---
## Void

`void` is a special data type that represents the absence of type. It essentially means "no type". It's most commonly used in these contexts: function return types, function parameters. `void` is also used with pointers, which we will learn about, later.

1. Function Return Types: When `void` is used as the return type for a function, it indicates that the function does not return a value. For example:

    ```cpp
    void printMessage() {
        std::cout << "Hello, World!" << std::endl;
    }
    ```
    In this example, `printMessage` is a function that doesn't return anything. Its only purpose is to print a message to the console.

2. Function Parameters: If a function is declared with `void` as its parameter list, it means that the function takes no arguments. For example, the `printMessage` function above has empty parentheses `()`. In reality, the compiler views the function like this, at compile time: `printMessage(void)`


> It's important to note that you cannot create variables of type `void`.

---
## Arrays

An array is a collection of elements of the same type stored in contiguous memory locations. The elements in an array can be accessed using their index, which starts from 0 for the first element, 1 for the second element, and so on.

Here's how you declare an array:

```cpp
type arrayName[arraySize];
```

For instance, to declare an array of integers named `numbers` that can hold 5 elements, you would write:

```cpp
int numbers[5];
```
The above example only creates the array. No values are added to the array, yet.

You can also insert values in an array when you declare it:

```cpp
int numbers[] = {10, 20, 30, 40, 50};
```

In this case, you don't have to specify the size of the array. The compiler automatically determines the size based on the number of elements in the initialization list.

Let's practice. How would you declare an array of `long` integers?

---
You might declare an array of `long` integers like this:
```cpp
long bigNumbers[] = {500000, 700000, 800000}
```

Each value in an array has an **index**. The index of a value is a number that represents its position in the array. In C++, and many other programming languages, 0 is the first index of an array.

In this example, what is the index for the number 30?
```cpp
int numbers[] = {10, 20, 30, 40, 50};
```

---
The index for the number 30 is 2. (Remember, the first value in an array is index 0, so the second value is index 1)


To access an element in the array, you use the array name followed by the index of the element in square brackets. For example, `numbers[0]` would give you the first element in the array.

It's important to note that C++ does not perform **bounds checking** for arrays. This means that if you try to access or store a value at an index outside the valid range of indices for the array, you will not get an error, but it can lead to unpredictable behavior or crashes. When you try to access as you will be accessing or modifying memory that is not allocated for the array. This is a common source of bugs in C++ programs. 

There are other data structures in the Standard Library, such as `std::array` and `std::vector`, that provide more features and better safety compared to built-in arrays.

---
## Pointers

A Pointer is a variable that stores the memory address of another variable. This address is the location of the variable in the computer's memory. Pointers provide a way to access and manipulate data in memory directly, which can be very useful but also potentially risky if not handled properly. 

Pointers are often used when your system has little memory, so memory must be carefully and efficiently used. Pointers use very little memory, even if they hold the memory address of a very large number, for example.

A pointer is declared using the `*` operator before the pointer name, and the data type of the pointer should match the type of the variable it points to. For example, an `int` pointer can store the address of an `int` variable:

```cpp
int num = 10;
int* p = &num;  // & operator is used to get the memory address of a variable
```

In this example, `p` is a pointer to an `int`, and it stores the memory address of the `num` variable. 

To access the value at the memory address that a pointer is pointing to, you use the `*` operator again:

```cpp
std::cout << *p;  // This will print 10
```
> The process of accessing the value of the memory address that a pointer holds, is called **dereferencing**.

You can also modify the value at the memory address that the pointer is pointing to:

```cpp
*p = 20;  // This changes the value of num to 20
```

Pointers are a fundamental part of C and C++ programming. They are used in a variety of contexts, including dynamic memory allocation, function arguments, and data structures. However, incorrect use of pointers can lead to bugs and security issues, so it's important to use them carefully.

A `void` pointer, denoted as `void*`, is a special type of pointer that can point to objects of any data type. A `void` pointer cannot be dereferenced unless it is cast to another type. We will learn about type casting, later.

---
