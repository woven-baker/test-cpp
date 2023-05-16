# Pointers and References

## Pointers
A pointer is a data type that holds the memory address of some object in memory.

### Declaration and Initialization
To declare a pointer, you need to specify the data type of the variable it points to, followed by the `*` symbol and the pointer's name:

```cpp
int* int_ptr;
```

A pointer does not automatically point to a valid memory address. You need to explicitly assign the address of a variable to a pointer. To do this, you can use the address-of operator `&`:

```cpp
int number = 42;
int* int_ptr = &number;
```

### Dereferencing Pointers
Dereferencing a pointer means accessing the value stored at the memory address the pointer is pointing to. You can dereference a pointer using the contents-of operator `*` before the pointer's name:

```cpp
int number = 42;
int* int_ptr = &number;

std::cout << "Value of number: " << *int_ptr << std::endl;
```

We can assign values to the memory at the address of the pointer the same way:

```cpp
int number = 42;
int* int_ptr = &number;

*int_ptr = 101;

std::cout << "Value of number: " << *int_ptr << std::endl;
```

What is the output of the previous code?

---

```
101
```

We can take a look at the memory addresses themselves as well:

```cpp
int number = 42;
int* int_ptr = &number;

std::cout << "Value of number: " << *int_ptr << std::endl;
std::cout << "Value of int_ptr: " << int_ptr << std::endl;
std::cout << "Address of number: " << &number << std::endl;
```

Here the value of `int_ptr` and the address of `number` should be the same? What are they in your program?

### nullptr

Consider the following code:

```cpp
#include <iostream>

void assignFortyTwo(int* ptr) {
    *ptr = 42;
}

int main() {
    int* int_ptr;
    assignFortyTwo(int_ptr);
    std::cout << "Value of number: " << *int_ptr << std::endl;
}
```

Go ahead and run this. What is the output?

---

If we do not initialize `int_ptr` with a valid address, it will be initialized to whatever value is in memory at the location where `int_ptr` was declared. What happens then when we dereference the memory at this address and do something with it?

In the best case, the value that is there forms an address that we don't have permission to access, and our program crashes, likely with a **segmentation fault**. In the worst case, the value forms an address to memory that our program is using, such as another variable, and silently corrupts that data.

If you need to use a pointer, you should always take care to intialize it with a valid address. In the event that you can't, initialize the pointer with `nullptr` instead:

```cpp
#include <iostream>

void assignFortyTwo(int* ptr) {
    *ptr = 42;
}

int main() {
    int* int_ptr = nullptr;
    assignFortyTwo(int_ptr);
    std::cout << "Value of number: " << *int_ptr << std::endl;
}
```

Run this fixed code. What is the output?

---

This doesn't solve the problem of our program crashing, but it prevents the possibility of data corruption. To completely fix code like this, we want to be diligent and always check for `nullptr` before we dereference a pointer, so we can prevent a crash:

```cpp
#include <iostream>

void assignFortyTwo(int* ptr) {
    if (ptr == nullptr) {
        std::cerr << "Unexpected nullptr!" << std::endl;
        return;
    }

    *ptr = 42;
}

int main() {
    int* int_ptr = nullptr;
    assignFortyTwo(int_ptr);
    std::cout << "Value of number: " << *int_ptr << std::endl;
}
```

Here we check for a `nullptr` inside `assignFortyTwo()`, and if we find one we log an error message and return early.

## References
A reference is an alias for another variable. It allows you to create a new name for an existing variable, and any changes made to the reference will also affect the original variable.

To declare a reference, you need to specify the data type of the variable it refers to, followed by the `&` symbol and the reference's name. Unlike pointers, references *must* be initialized when they are declared:

```cpp
int number = 42;
int& number_ref = number;
```

### Using References
When you use a reference, you don't need to dereference it like you would with a pointer. You can use the reference just like you would use the original variable:

```cpp
int number = 42;
int& number_ref = number;

number_ref = 100;

std::cout << "Value of number: " << number << std::endl;
```

What is the output of this previous code?

---

```
Value of number: 100
```

## Passing by Reference
There are several ways you could pass a value to a function, but we will typically want to do it in one of the following three ways:

```cpp
void passByValue(int a) { a = 5; }
void passByReference(int& a) { a = 5; }
void passByConstReference(const std::string& a) { std::cout << a << std::endl; }
```

When you want the function to take a copy of the parameter, pass by value:

```cpp
void passByValue(int a) { a = 5; }
```

Here the integer that gets passed to `passByValue` gets copied into the integer variable `a` that is local to this function.

When you want the function to modify the parameter, pass by reference:

```cpp
void passByReference(int& a) { a = 5; }
```

Let's use the previous code and replace the pointer with a reference to illustrate how to pass by reference:

```cpp
#include <iostream>

void assignFortyTwo(int& num) {
    num = 42;
}

int main() {
    int num = 10;
    assignFortyTwo(num);
    std::cout << "Value of num: " << num << std::endl;
}
```

Notice that when you call a function that takes a value by reference, you can pass the variable directly in the call. It will get turned into a reference automatically.

What is the output of the previous code:

---

```
Value of number: 43
```

If you have a parameter that is expensive to copy **and** you don't want the function to be able to modify it, pass by const reference:

```cpp
void passByConstReference(const std::string& a) { std::cout << a << std::endl; }
```

## Parameter Passing Guidelines

### "in" parameters
For "in" parameters, pass cheaply-copied types by value and others by reference to const:

```cpp
void cheap(const std::string& s);
void expensive(std::string s);
void unbeatable(int x);
void bad(const int& x);
```

Why? Assuming we're on a typical 64-bit machine, a reference is simply an address and takes up 8 bytes of memory. It also requires the machine to go through a level of indirection and dereference the underlying object, which has an overhead. A typical `int` takes up 4 bytes of memory and does not need to be dereferenced. If we passed an `int` by const reference, we have actually reduced the performance of our program for no reason. A `std::string` on the other hand, is likely to be quite a bit larger than 8 bytes, and so it makes sense to simply pass an 8 byte address and dereference instead of potentially waste a lot of resources copying the `std::string` around.

### "in-out" parameters
For "in-out" parameters, pass by reference to non-const:

```cpp
void update(Record& r);
```

When we write functions with "in-out" parameters like this, the expectation is that this function will modify the parameter.

### "out" parameters
Always prefer return values to "out" parameters. In other words, don't write this:

```cpp
void squareNumber(int number, long& result);
```

Write this instead:

```cpp
long squareNumber(int number);
```

## References vs. Pointers
While both references and pointers allow you to access the memory address of another variable, there are some key differences between the two:

- References must be initialized when they are declared, whereas pointers can be uninitialized.
- References cannot be reassigned to another variable, while pointers can.
- You don't need to dereference a reference to access the value it refers to, but you need to dereference a pointer.
- References cannot be `nullptr`, while pointers can.

In general, use references everywhere that you can. Only when you need to pass a memory address to something and you can't use a reference, use a pointer.

# Exercises

## Exercise 1
What is the output of the following code?
```cpp
int x = 10;
int y = 20;
int *ptr = &x;
*ptr = y;

std::cout << x << std::endl;
```

```
A. 10
B. 20
C. A memory address
D. A compilation error
```

---

```
B. 20
```

## Exercise 2
Fill in the blanks to create a function that swaps the values of two integers using references.

```cpp
void swap(______ a, ______ b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int num1 = 5;
    int num2 = 10;
    swap(______, ______);
    std::cout << "num1: " << num1 << ", num2: " << num2 << std::endl;
    return 0;
}
```

---

```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int num1 = 5;
    int num2 = 10;
    swap(num1, num2);
    std::cout << "num1: " << num1 << ", num2: " << num2 << std::endl;
    return 0;
}
```

## Exercise 3
What are the two main differences between using the `&` symbol and the `*` symbol?

```
A. The & symbol is used to declare pointers, while the * symbol is used to declare references.
B. The & symbol is used to get the address of a variable, while the * symbol is used to dereference a pointer.
C. The & symbol is used to dereference a pointer, while the * symbol is used to get the address of a variable.
D. The & symbol is used to declare references, while the * symbol is used to declare pointers.
```

---

```
B and D.
```

## Exercise 4
Which of the following statements is true about references in C++?

```
A. References can be uninitialized.
B. References can be reassigned to another variable.
C. References must be dereferenced to access the value they refer to.
D. References must be initialized when they are declared.
```

---

```
D. References must be initialized when they are declared.
```

## Exercise 5
Which of the following statements about nullptr are true? (Select all that apply.)

```
A. nullptr is a keyword in C++.
B. nullptr can be assigned to any pointer type.
C. nullptr is a special type of reference.
D. It is good practice to initialize a pointer to nullptr if it is not immediately assigned to a valid memory address.
```

---

```
A. nullptr is a keyword in C++.
B. nullptr can be assigned to any pointer type.
D. It is good practice to initialize a pointer to nullptr if it is not immediately assigned to a valid memory address.
```

## Exercise 6
Complete the following code snippet to check if a pointer is equal to nullptr:

```cpp
int* ptr = nullptr;

if (______) {
    std::cout << "The pointer is a nullptr." << std::endl;
} else {
    std::cout << "The pointer is not a nullptr." << std::endl;
}
```

---

```cpp
if (ptr == nullptr)
```

## Exercise 7
Consider the following code:

```cpp
#include <iostream>

constexpr int global_var = 3;

int& complex_function(int*& a, int*& b, int& c) {
    int* temp = a;
    a = b;
    b = temp;
    c = *a + *b;
    return global_var;
}

int main() {
    int x = 1;
    int y = 2;
    int* p1 = &x;
    int* p2 = &y;
    int result = 0;
    int& ref = complex_function(p1, p2, result);

    std::cout << "x: " << x << ", y: " << y << ", *p1: " << *p1 << ", *p2: " << *p2
              << ", result: " << result << ", ref: " << ref << ", global_var: " << global_var << std::endl;

    ref = 10;

    std::cout << "x: " << x << ", y: " << y << ", *p1: " << *p1 << ", *p2: " << *p2
              << ", result: " << result << ", ref: " << ref << ", global_var: " << global_var << std::endl;

    return 0;
}
```

What will be the output after the first `std::cout` statement?

---

```
x: 1, y: 2, *p1: 2, *p2: 1, result: 3, ref: 3, global_var: 3
```

## Exercise 8
What will be the output after the second `std::cout` statement?

---

```
x: 1, y: 2, *p1: 2, *p2: 1, result: 3, ref: 3, global_var: 3
```