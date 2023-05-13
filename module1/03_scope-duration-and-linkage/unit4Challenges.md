# Challenges

## Challenge 1
There is a fourth type of storage duration since C++11. What is it? How would we properly declare an `int` variable `thread_data` such that it has this storage duration, and initialize it with the value 10?

---

The fourth type of storage duration is thread storage duration. Thread storage duration applies to variables that are declared with the `thread_local` specifier. These variables are created when a thread begins and destroyed when it ends. Each thread has its own instance of these variables.

```cpp
#include <thread>

thread_local int thread_data { 10 };
```