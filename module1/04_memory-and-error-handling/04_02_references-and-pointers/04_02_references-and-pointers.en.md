# 4. The Concept of Variables and Addresses (20 minutes)

## Variables as Named Memory Locations (5 minutes)

In C++, variables are named memory locations used to store data. When you declare a variable, the compiler allocates memory for the variable based on its data type. The memory location assigned to the variable has a unique address, which can be used to access the stored data.

### Example: Variable Declaration and Memory Allocation

```cpp
#include <iostream>

int main() {
    int myVar = 42; // Declare an integer variable named 'myVar' and initialize it with the value 42
    return 0;
}
```

In the example above, the int variable myVar is declared and initialized with the value 42. The compiler allocates memory for the variable, and the memory location has a unique address.

## Obtaining the Address of a Variable (5 minutes)
In C++, you can obtain the address of a variable using the address-of operator (&). The address-of operator returns the memory address of the specified variable.

### Example: Using the Address-of Operator to Obtain a Variable's Address

```cpp
#include <iostream>

int main() {
    int myVar = 42;
    std::cout << "Value of myVar: " << myVar << std::endl;
    std::cout << "Address of myVar: " << &myVar << std::endl;
    
    return 0;
}
```

### Practical Examples (5 minutes)
Provide hands-on examples and exercises to help learners understand the relationship between variables and addresses. For instance, you could ask learners to declare various data types and print their memory addresses. This will help learners comprehend how memory is allocated and managed for different variables.

In the next section, we will explore how arrays and memory are related in C++.

# godbolt.org
```cpp
int main() {
    int i = 5;
    return i;
}
```

```cpp
int main() {
    int i = 5;
    int& i_ref = i;
    return i;
}
```

```cpp
int main() {
    int i = 5;
    int* i_ptr = &i;
    return i;
}
```

```cpp
long add(int& a, int& b) {
    return a + b;
}

int main() {
    int a{ 5 };
    int b{ 6 };
    long result = add(a, b);
}
```

```cpp
long add(int a, int b) {
    return a + b;
}

int main() {
    int a{ 5 };
    int b{ 6 };
    long result = add(a, b);
}
```


# Exercises

## Exercise 
For the most optimal code, what should the function signature for add look like?

```cpp
a) long add(int a, int b);
b) long add(int& a, int& b);
c) long& add(int& a, int& b);
d) long& add(int a, int b);
```

---

```
a
```

## Exercise 
For the most optimal code, what should the function signature for print look like?
```cpp
int 
```

```cpp
a) void print(int a, int b) const;
b) void print(int& a, int& b);
c) void print(int& a, int& b) const;
d) void print(int a, int b);
```

---

```
a
```