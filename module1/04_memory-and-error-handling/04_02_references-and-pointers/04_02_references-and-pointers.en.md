# Pointers and References
Pointers and references allow you to manipulate memory addresses and access the data stored at those addresses directly. Understanding pointers and references is essential to writing efficient and flexible C++ code.

## Pointers
A pointer is a variable that holds the address of another variable in memory. Pointers are declared using the `*` symbol after the data type.

### Declaration and Initialization
To declare a pointer, you need to specify the data type of the variable it points to, followed by the `*` symbol and the pointer's name:

```cpp
int* int_ptr;
```

It's important to note that the pointer does not automatically point to a valid memory address. You need to explicitly assign the address of a variable to the pointer. You can use the address-of operator `&` to get the memory address of a variable.

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

If we do not initialize `int_ptr` with a valid address, it will be initialized to whatever 8 bytes are on the stack at the location where memory was allocated for `int_ptr`. In the best case, these 8 bytes form an address that we don't have permission to access, and our program crashes, likely with a **segmentation fault**. In the worst case, these 8 bytes somehow form an address to memory that our program is using, such as another variable, and silently corrupts that data.

You should always intialize a pointer with a valid address. But in the event that you can't, initialize the pointer with `nullptr` instead:

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

### Pointer Arithmetic
Pointer arithmetic allows you to navigate through memory addresses by adding or subtracting the number of elements from a pointer. For example, if you have a pointer to an integer, adding 1 to the pointer will make it point to the memory address of the next integer in memory:

```cpp
int numbers[] = {1, 2, 3};
int* int_ptr = numbers;

std::cout << *int_ptr << std::endl;

int_ptr++;

std::cout << *int_ptr << std::endl;
```

## References
A reference is an alias for another variable. It allows you to create a new name for an existing variable, and any changes made to the reference will also affect the original variable. References are declared using the `&` symbol after the data type.

### Declaration and Initialization
To declare a reference, you need to specify the data type of the variable it refers to, followed by the `&` symbol and the reference's name. Unlike pointers, references must be initialized when they are declared.

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

## References vs. Pointers
While both references and pointers allow you to access the memory address of another variable, there are some key differences between the two:

- References must be initialized when they are declared, whereas pointers can be uninitialized.
- References cannot be reassigned to another variable, while pointers can.
- You don't need to dereference a reference to access the value it refers to, but you need to dereference a pointer.
- References cannot be `nullptr`, while pointers can.

In general, use references when you want to create an alias for a variable and don't need to change the alias to another variable later on. Use pointers when you need more flexibility, such as the ability to reassign the pointer or to use pointer arithmetic.

## Passing by Reference vs. Passing by Pointer
When passing arguments to a function, you can choose between passing by reference or passing by pointer. Both methods allow the function to modify the original variable *by passing the address of the original variable*. However, there are some differences in syntax and usage.

### Passing by Reference
To pass an argument by reference, you need to declare the function parameter as a reference. When you call the function, you can pass the variable directly:

```cpp
void increment(int& num) {
    num++;
}

int main() {
    int number = 42;
    increment(number);
    std::cout << "Value of number: " << number << std::endl;
    return 0;
}
```

What is the output of the previous code:

---

```
Value of number: 43
```

### Passing by Pointer
To pass an argument by pointer, you need to declare the function parameter as a pointer. When you call the function, you need to pass the address of the variable using the address-of operator `&`:

```cpp
void increment(int* num) {
    (*num)++;
}

int main() {
    int number = 42;
    increment(&number);
    std::cout << "Value of number: " << number << std::endl;
    return 0;
}
```

What is the output of the previous code:

---

```
Value of number: 43
```

## Which Method to Choose
Pass by reference when you want a more straightforward syntax and when you want to ensure that the function always receives a valid reference. Pass by pointer when you want to allow the function to receive a `nullptr` or when you want to use pointer arithmetic.

In general, prefer passing by reference.

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
Given the following code, what will be the output?

```cpp
#include <iostream>

void update(int*& p) {
    p = new int{42};
}

int main() {
    int x = 5;
    int* p = &x;
    update(p);
    std::cout << "*p: " << *p << std::endl;
    delete p;
    return 0;
}
```

---

```
*p: 42
```

## Exercise 8
Consider the following code:

```cpp
#include <iostream>

int global_var = 3;

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

## Exercise 9
What will be the output after the second `std::cout` statement?

---

```
x: 1, y: 2, *p1: 2, *p2: 1, result: 3, ref: 3, global_var: 3
```