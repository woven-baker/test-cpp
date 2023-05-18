# Comments

## What are comments?
Comments are a way to leave messages in your code for yourself or other people who read your code. Comments can help people understand your code, help you remember why you designed code in a certain way, and more.

Comments are ignored by the compiler and not included in the exectuable file, so don't worry about writing too many comments.

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
Here are some common conventions used in writing good comments:

- **Descriptive**: Comments should explain what code does, not just restate the code. Good comments answer the question "why" rather than "what". For example, instead of writing `// incrementing i`, a better comment would be `// increment i to move to the next element`.

- **Brief**: While comments should be descriptive, they should also be concise. Aim for clarity and brevity in your comments.

- **Up to date**: Comments should be kept up-to-date with the code. An outdated comment can be more confusing than no comment at all.

- **Avoid Obvious Comments**: Comments should provide additional insights, not state the obvious. For instance, `int count; // declares an integer variable` is an example of an unnecessary comment.

- **Function/Method Comments**: It's good practice to have comments above functions or methods, explaining what the function does, its input parameters, and its return value.

- **Use TODO comments**: If you know that a certain part of your code needs to be revisited or is not complete, leave a `// TODO:` comment so you can easily find and address it later.

- **Block Comments for Sections**: If you have a section of code doing one unit of work, you might want to start the section with a multi-line comment explaining the purpose of the following code block.

Remember, the purpose of comments is to help other people understand your code. It also helps you remember what your code does when you come back to it after a long time. (You might not remember what something does).

Good commenting can make a big difference in the maintainability and readability of your code.