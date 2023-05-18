# Standard Input and Standard Output

## What are Standard Input and Standard Output?
In the context of computer programming and data streams, "Standard Input" and "Standard Output" refer to the default systems of the OS used by a program to receive input and send output.

>Standard Input and Standard Output are a type of mechanism called a 'data stream'. That is because they will read a sequence of incoming data (a "stream"), piece-by-piece until they find some kind of signal that makes them stop reading data. There are different kinds of stop signals for different situations and you don't need to learn about them yet, but you can research them on your free time, if you want.

1. **Standard Input (stdin)**: This is the default input data stream. When a program expects input from the user, it's typically read from the Standard Input. By default, the Standard Input reads incoming signals from the keyboard. For instance, if you're running a program in the console or terminal and it expects you to type something, Standard Input will open and receive incoming keyboard signals. In C++, you often use `std::cin` (part of the iostream library) to read from the Standard Input.

2. **Standard Output (stdout)**: This is the default output data stream. When a program needs to output some data, it's usually sent to the Standard Output. By default, the Standard Output is the console or terminal. For example, when you run a program and it prints something out for you to read, it goes to Standard Output, and a program such as your terminal window will read the data from Standard Output (printing it on screen, in the terminal window). In C++, you often use `std::cout` (also part of the iostream library) to write to the Standard Output.

There's also a third commonly mentioned data stream, **Standard Error (stderr)**, which is used for outputting error messages. This is typically also directed to the terminal by default, but can be separated from stdout. In C++, `std::cerr` is used for writing to the standard error stream.

These streams can often be redirected at the operating system level. For example, you might have a program that normally reads input from the keyboard and writes output to the console, but you could redirect it to read input from a file and write output to a different file. This is a common technique for automating tasks and for handling large amounts of data.