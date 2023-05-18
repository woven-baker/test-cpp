# Compilation and using the compiler

## What is the Compiler?

In the previous section, we learned that C++ is a programming language that uses a compiler to make programs. Let's review that, quickly.

> The C++ compiler is a type of software that transforms the code written by a programmer into a format that a computer can understand and execute. This transformation process is known as "compilation".

To give you a simple analogy, imagine you speak only English, and you have a friend who speaks only Spanish. You want to communicate a message to your friend, but you don't know Spanish. Here, a compiler would act like a translator, taking your English message (equivalent to the high-level programming language), understanding it, and translating it into Spanish (equivalent to the low-level machine code) for your friend to understand.

In the process of compilation, the C++ compiler also performs various tasks like checking for syntax errors, optimizing the code for better performance, and more. If your C++ code has any errors, the compiler will let you know by generating error messages.

To use a C++ compiler, you typically write your C++ code in a text editor or integrated development environment (IDE), save it with a .cpp file extension, and then use the compiler to compile the .cpp file into an executable program. 

>The combination of 1) your compiler, 2) text editor or IDE, and 3) your Operating System and other tools is sometimes collectively called your **environment** or **local environment**.

## Basic Compiler usage

Let's make something!

First, you need to write your C++ code. This is a very basic program that prints "Hello, World!" to the console.

```cpp
// This is a simple C++ program

#include<iostream> // Include the iostream library

int main() { // This is the main function. Execution of the program starts here.
  std::cout << "Hello, World!"; // Print "Hello, World!" to the console
  return 0; // Indicate that the program ended successfully
}
```

You can copy and paste this code in a text editor and save it as `hello_world.cpp`.

Now, we want to compile this code using the C++ compiler. We'll use `g++`, a popular C++ compiler that's part of the GNU Compiler Collection. You'll need to open a terminal (or command prompt) and navigate to the directory where you saved `hello_world.cpp`.

In the terminal, you can compile the program using the following command:

```bash
g++ hello_world.cpp -o hello_world
```

In this command:

- `g++` is the compiler.
- `hello_world.cpp` is the source file (the C++ code you wrote).
- `-o` is an option that lets you specify the output file's name.
- `hello_world` is the name of the output file. This is the executable file that will be created by the compiler.

If there are no errors in your code, the compiler will create an executable file named `hello_world` (or `hello_world.exe` on Windows) in the same directory.

To run the program, you can use the following command in the terminal:

On Linux or MacOS:

```bash
./hello_world
```

On Windows:

```bash
hello_world.exe
```

When you run this program, it will print "Hello, World!" to the console. This is the output of your program.

>The machine code file that the compiler produces is called a **binary**, **executable file** or just **executable**. Be careful, different developers might use different names!.

**Let's review:**
What is the purpose of the `-o` option when using the compiler?
```
A. It makes the compilation faster.
B. It allows you to name the output file.
C. It specifies the file that contains your original C++ code.
D. It deletes the original C++ file after compilation finishes.
```

---

The answer is:
```
B.
```

---

## What else does the compiler do?

Compilers not only translate code from C++ to machine code, but they also perform optimization to make the code run faster or use fewer resources like memory and power.

Optimization is a complex process and involves several different techniques. Here are just a few examples:

1. **Constant folding**: If the compiler sees an expression involving constants, it calculates the result at compile time instead of at run time. For example, the compiler will replace `int x = 2 * 3` with `int x = 6`.

2. **Dead code elimination**: The compiler removes code that doesn't affect the program's output. For example, if there's a variable that's never used, the compiler might remove that code.

3. **Loop unrolling**: This is a technique that attempts to minimize the memory overhead of loop control structures by replicating the loop's body. This reduces the number of iterations and can improve performance, but at the cost of increasing the size of the binary.

4. **Function inlining**: This replaces a function call with the function's code to save the overhead of a function call. But, like loop unrolling, this can increase the binary size.

5. **Instruction scheduling**: The compiler rearranges machine instructions to keep the processor's pipeline filled, minimizing the time the processor spends idly waiting for instructions or data.

The amount of optimization done by the compiler is generally controlled by the programmer through the use of compiler flags. For the g++ compiler, you can control optimization with the `-O` flag followed by a number or letter:

- `-O0` (that's 'O' followed by a zero) means "no optimization". This is the default.
- `-O1` means "optimize somewhat". This might make the program run faster.
- `-O2` means "optimize more". This will make the compiler take more time to optimize the code, but might make the program run even faster.
- `-O3` means "optimize even more". This will make the compiler take even more time, and might make the program run fastest of all.
- `-Os` means "optimize for size". This will try to make the program smaller rather than faster.

Note that these optimizations aren't guaranteed to make every program run faster or be smaller. Sometimes an optimization can even make a program slower or larger. Finding the best optimization for a particular program might require lots of testing and experimenting.