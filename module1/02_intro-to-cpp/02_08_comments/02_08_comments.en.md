# Comments

## What are comments?
Comments are a way to leave messages in your code for yourself or other people who read your code. Comments can help people understand your code, help you remember why you designed code in a certain way, and more.

Comments are ignored by the compiler and not included in the exectuable file, so they won't impact the final binary size or cause a change in program behavior.

## How to make comments
In C++, there are two main ways to write comments:

1. **Single-line comments**: These begin with two forward slashes `//` and continue until the end of the line. Everything on the line after the `//` is considered a comment and ignored by the compiler. 

   ```cpp
   // This is a single-line comment.
   int myVar = 10; // You can also have a comment at the end of a line.
   ```

2. **Multi-line comments**: These begin with a forward slash followed by an asterisk `/*` and end with an asterisk followed by a forward slash `*/`. Everything between these symbols is considered a comment and ignored by the compiler, and these comments can span multiple lines.

   ```cpp
   /* This is a multi-line
      comment. It continues until 
      the closing symbols are reached. */
   ```

## Conventions and advice for writing comments
Do not write comments as a way of justifying bad variable names or unclean or messy code. Do your best to write code that is self documenting and write comments only when the code cannot speak for itself.

When you do need to write comments, here are some common conventions used in writing good comments:

- **Descriptive**: Comments should explain what code does, not just restate the code. Good comments answer the question "why" rather than "what".

- **Brief**: While comments should be descriptive, they should also be concise. Aim for clarity and brevity in your comments.

- **Up to date**: Comments should be kept up-to-date with the code. An outdated comment can be more confusing than no comment at all. This is also why you should prefer to write clean, self-documenting code over writing comments.

- **Avoid Obvious Comments**: Comments should provide additional insights, not state the obvious. For instance, `int count; // declares an integer variable` is an example of an unnecessary comment. Often times you can remove the need for a comment by deciding on a more descriptive name for your variable or function, and by avoiding writing long lines of difficult-to-read logic.

- **Use TODO comments**: If you know that a certain part of your code needs to be revisited or is not complete, leave a `// TODO:` comment so you can easily find and address it later.