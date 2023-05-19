# Programming Environments

A typical C++ programming environment consists of several tools that facilitate the process of writing, debugging, and running your code.

In previous lessons, we learned about the compiler and linker programs. Let's look at some other common tools in C++ environments.

## Text Editor/Integrated Development Environment (IDE)

This is where you write your code. A text editor can be as simple as Notepad! 
More complex editors like Sublime Text, Atom, or Visual Studio Code provide features like syntax highlighting and auto-completion, which are incredibly useful for coding. 

An IDE, on the other hand, is a more comprehensive tool that typically includes a text editor, compiler, debugger, and more. Examples of C++ IDEs include Microsoft Visual Studio, and CLion.

>Note: 'IDE' is not a standardized term. Some developers may refer to an editor as an IDE, but some developers might not consider that same editor as a "complete" IDE.


## Debugger

A debugger is a tool that helps you find and fix bugs in your code. It allows you to pause your program, inspect the values of variables, step through your code one line at a time, and more. This makes it easier to identify the exact place and cause of a bug. GDB (GNU Debugger) and LLDB are popular debuggers for C++. Many IDEs also include graphical front-ends for these debuggers.

## Build System/Build Automation Tools
As your projects get larger, compiling and linking your code can become more complex. Build systems automate this process. You provide a script or configuration file that describes how to build your project, and the build system takes care of the rest whenever you want to build your project. Examples of build systems include Make, CMake, and Meson.

## Version Control
When working on a project (especially in a team), it's important to have a way of managing different versions of your code, keeping track of changes, and enabling collaboration. This is what a Version Control system does. Git is a widely used Version Control tool, and it integrates well with platforms like GitHub and GitLab for online hosting of code.

## Libraries and Frameworks
These are collections of pre-written code that you can use to perform common tasks, instead of writing everything by yourself. The C++ Standard Library comes with the language and provides a wide range of features, like containers (e.g., vectors, lists, maps) and algorithms (e.g., sort, find). There are also many third-party libraries and frameworks for things like graphics (e.g., SFML, OpenGL), testing (e.g., Google Test), and more.

## Documentation Tools
These tools are used to generate documentation from your source code. Doxygen is a popular documentation generator for C++. Good documentation makes your code easier to understand and maintain, both for yourself and other developers.
