# What is C++?

C++ is a programming language developed in the 1980s. It was built as an extension of the C programming language. C++ is now widely used in many industries and, together with C, introduced many innovations in programming that almost all modern programming languages still use.

## How programming was done before C and C++
Before C and C++, much programming was done using Assembly and machine code. This meant that if you wanted to make a program for a computer, you had to write the program using the specific Assembly language that the computer's processor used. At the time, several companies were producing their own processors, and in many cases they each used their own version of Assembly. This means that a program written for one computer, would have to be completely re-written for another computer. 

This was a slow, and time-consuming process. Now, companies that manufacture processors have standardized much of the Assembly language that different processors use. However, similar to the 1980s, different types of processors still exist, and they still use different language instructions. Two examples of different processor types that are widely used today are ARM processors (often used in small devices, such as smartphones) and x86 processors (usually used in laptop and desktop computers).

---

## How did C and C++ change everything?

C and C++ are called **high-level languages**. Assembly and machine code are very long, and uses special vocabulary and abbreviations that are difficult to memorize and use. C and C++ code greatly simplified this, and provided a standard set of vocabulary for writing source code. Instead of using a large amount of difficult Assembly vocabulary, you could use a few C or C++ commands. Much easier!

Let's look at an example. Here is the code for `1 + 1` in x86 Assembly.
```asl
section .data
    result db 0 ; Define a byte to store the result

section .text
    global _start
    
_start:
    ; Move the value 1 into the AL register
    mov al, 1
    
    ; Add 1 to AL
    add al, 1
    
    ; Store the result in the 'result' variable
    mov [result], al
    
    ; Prepare for writing to standard output
    mov eax, 4      ; System call number for 'write'
    mov ebx, 1      ; File descriptor 1: standard output
    mov ecx, result ; Address of the data to write
    mov edx, 1      ; Number of bytes to write
    
    ; Perform the system call to write to standard output
    int 0x80        ; Call the kernel
    
    ; Exit the program
    mov eax, 1      ; System call number for 'exit'
    xor ebx, ebx    ; Exit status 0
    int 0x80        ; Call the kernel
```

Here is the C++ code for `1 + 1`:

```cpp
#include <iostream>

int main() {
    int result = 1 + 1;
    std::cout << result;
}
```
Do you think the C++ code is shorter and easier to read?

---

C and C++ also changed everything because a program you wrote for one processor could run on a different processor, without changing the original source code. How did this become possible?

C and C++ use a special program called a **compiler**. After you write some C++ code, you run the compiler which translates the code into machine code. This process is called **compiling**. You can choose what processor you want to compile your code for.

The compiler allowed C and C++ code to become very portable. The only requirement was that the compiler supported the target processor. Today, the C++ compiler can produce machine code for a very large number of processors.

> C and C++ were not the first **high-level languages**, but they were the first general purpose languages. Other high-level languages were designed for very specific uses. For example, you might have a programming language just for mathematics, or another programming language just for making business applications. C and C++ combined many features so you can build any type of program!

---
## Where is C++ used today?

C++ is used in almost every industry. 

- **Robotics**: C++ is used to control sensors, motors, and more.
- **Computer Hardware**: C++ is used to write Drivers, that allow different devices (such as keyboards or headphones) to connect to computers.
- **Systems Programming**: C++ is used to make Operating Systems and the tools they need.
- **GUI programming**: the graphical elements used in many OS, such as icons, menus, and windows.
- **Application Programming**: word processors, music applications, art and graphics applications.
- **Video Games and CGI**: computer graphics software and video game design software are usually made in C++
- **Machine Learning**: Although the Python programming language is famous for Machine Learning, those ML libraries are usually written in C or C++. The user only accesses those features by using Python!
- ...and more!

## Are C and C++ different?

C++ and C do share many features, and C++ can be thought of as a superset of C. C++ adds support for object-oriented programming, better type checking, and a host of other extremely useful features and abstractions.

Sometimes C++ and C code can be used together. You could write code that can be compiled with a C++ compiler but that is written in the style of C code, but this is not recommended. It is important to remember that **C++ is its own language**. It has its own guidelines and idioms and best-practices. C++ has many features that aim to make software development more safe, more maintainable, and more productive than development with C.

Because of these extra features, often times C++ code **cannot** directly be interchanged with C code. This won't be a problem in this module, but situations like this might occur in your job and in embedded development where you are working with existing code or existing libraries. Don't worry, we will learn more about this in the next module.