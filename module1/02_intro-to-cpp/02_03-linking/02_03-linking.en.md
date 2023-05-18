# Linking

## Introduction to linking
Linking is an essential step in the process of turning your source code into an executable program. After the individual pieces of code have been compiled into object files, these files need to be linked together to form the final executable.

Let's explain this in a bit more detail:

1. **Object Files**: When you compile a C++ program, each source code file (.cpp) is turned into an "object file" (.o or .obj). This file contains the compiled machine code of your source file along with additional information about the symbols (such as function names and global variables) defined and used in the code.

2. **Linking**: After all your source files have been compiled into object files, these object files need to be combined into a single executable file. The process of combining these files is called "linking", and it's done by a program called a "linker".

The linker's job involves resolving references between files. Sometimes, your program source code might be separated into multiple files. Code you write in one file, can be used in any of the other files. The linker will try to find the best way to combine code, so you don't need copies of code in several places of your executable. We will learn more about this, later.

Linking isn't just for **your** code; it also involves linking to libraries. Libraries are collections of pre-compiled code that you can reuse in your programs. For example, the `std::cout` object that you use to print to the console in a C++ program is defined in the C++ Standard Library. When you use `std::cout` in your program, the linker adds the necessary code from the Standard Library into your executable.

Linking can be either static or dynamic:

- **Static Linking**: Here, the linker takes the library code your program uses and includes copies of it directly in your executable file. This makes the executable file larger, but the advantage is that you can run the program on any system without needing to have the library installed.

- **Dynamic Linking**: In this case, the linker doesn't include the library code in your executable. Instead, it adds information to the executable that says "when you run this program, load the code from this library". This makes the executable file smaller and allows multiple programs to share the same library code in memory, but it means that you need to have the correct version of the library installed on any system where you want to run the program.

Later, we will study how to control the linking process more directly.