# Operations and Operators

## What are operators?

In C++, operators are special symbols used to perform various operations on operands. We've been using many of these symbols in previous lessons: `=`, `<`, `>`, and even the `[]` used to access array values are all operators.

Here's an overview of some of the common types of operators in C++.

---
## Arithmetic Operators 
These operators are used for mathematical operations.
- Addition: `+`
- Subtraction: `-`
- Multiplication: `*`
- Division: `/`
- Modulus (remainder): `%`

Examples:

```cpp
int main() {
    int a { 10 };
    int b { 3 };

    int sum = a + b;  // sum is 13
    int diff = a - b; // diff is 7
    int prod = a * b; // prod is 30
    int quot = a / b; // quot is 3
    int mod = a % b;  // mod is 1
}
```

---
## Relational Operators
These operators are used for comparison.
- Equal to: `==`
- Not equal to: `!=`
- Greater than: `>`
- Less than: `<`
- Greater than or equal to: `>=`
- Less than or equal to: `<=`

Examples:

```cpp
int main() {
    int a { 10 };
    int b { 3 };

    bool isEqual { a == b }; // isEqual is false
    bool isNotEqual { a != b }; // isNotEqual is true
    bool isGreaterThan { a > b }; // isGreaterThan is true
}
```

---
## Logical Operators

These operators are used for boolean logic operations.
- Logical AND: `&&`
- Logical OR: `||`
- Logical NOT: `!`

Example:

```cpp
int main() {
    bool a { true };
    bool b { false };

    bool andResult { a && b }; // andResult is false
    bool orResult { a || b }; // orResult is true
    bool notResult { !a }; // notResult is false
}
```

---
## Assignment Operators 
These operators are used to assign values to variables.
- Simple assignment: `=`
- Add and assign: `+=`
- Subtract and assign: `-=`
- Multiply and assign: `*=`
- Divide and assign: `/=`
- Modulus and assign: `%=`

Example:

```cpp
int main() {
    int a { 10 };
    a += 5; // a is now 15
    a -= 2; // a is now 13
}
```

---
## Bitwise Operators

These operators are used for manipulation of individual bits. These are normally used when directly manipulating memory for performance reasons, due to memory constraints, or for certain mathematical operations. They are a little more difficult to learn, as they require more knowledge about how values are stored in memory as binary data.
- Bitwise AND: `&`
- Bitwise OR: `|`
- Bitwise XOR: `^`
- Bitwise NOT: `~`
- Left shift: `<<`
- Right shift: `>>`

---
## Increment and Decrement Operators
These operators are used to increase or decrease a variable's value by one.
    - Increment: `++`
    - Decrement: `--`

Example:

```cpp
int a { 10 };
a++; // a is now 11
a--; // a is now 10
```

---
## Conditional Operator (Ternary operator)
This operator is used to perform operations based on a condition. It takes three operands: a condition, a result for when the condition is true, and a result for when the condition is false.

Example:

```cpp
int a { 10 };
int b { 20 };
int max { (a > b) ? a : b }; // max is 20
```

There are also other operators like the `new` and `delete` operators for dynamic memory allocation, the `[]` operator for accessing array elements, and the `.` and `->` operators for accessing members of a class or struct, but we will learn about these later.