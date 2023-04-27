# Introduction to `std::array` and `std::vector`

## C-style Static Arrays
Until this point, when we wanted to store an array of values, we would use a C-style static array such as in the following code:

```cpp
int numbers[5] = {1, 2, 3, 4, 5};
```

These arrays have some downsides.

First, we have to keep track of its size and manage it independently from the array itself:

```cpp
#include <iostream>

long sum(int numbers[5]) {
    long sum{};
    for (int i = 0; i < 6; ++i) {
        sum += numbers[i];
    }
    return sum;
}

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    auto result = sum(numbers);
    std::cout << "Sum: " << result << std::endl;

    return 0;
}
```

Did you notice the bug in the previous code?

---

We access past the end of the array within the `sum()` function. The compiler doesn't complain, and we may not even get a runtime error, but our sum will most certainly be incorrect.

Second, we cannot simply copy the array:

```cpp
int numbers[5] = {1, 2, 3, 4, 5};
auto numbers_copy = numbers;
```

C-style arrays automatically decay into pointers. `numbers` is a `int*`, and so is `numbers_copy`, which just gets assigned this same pointer. This can also be a source of confusion and bugs:

```cpp
int numbers[5] = {1, 2, 3, 4, 5};
auto numbers_copy = numbers;
numbers_copy[3] = 20;
std::cout << numbers[3] << std::endl;
```

What is the output of this code?

---

This code outputs 20, not 4.

Furthermore, to copy this array we would have to use a function like [memcopy](https://en.cppreference.com/w/cpp/string/byte/memcpy) or iterate over the array and move elements one at a time manually.

Fortunately C++ has better ways of using arrays.

## Introduction to `std::array`
`std::array` is a C++ Standard Library container that provides a fixed-size, zero-overhead alternative to C-style static arrays. Like a C-style array, its size is determined at compile time, but it encapsulates C-style arrays and provides useful functions to work with the elements:

```cpp
#include <iostream>
#include <array>

long sum(const std::array<int, 5>& numbers) {
    long sum{};
    for (auto i : numbers) {
        sum += i;
    }
    return sum;
}

int main() {
    std::array<int, 5> numbers {1, 2, 3, 4, 5};
    auto result = sum(numbers);
    std::cout << "Sum: " << result << std::endl;

    std::cout << "Is empty?: " << std::boolalpha << numbers.empty() << std::endl;
    std::cout << "Size of numbers: " << numbers.size() << std::endl;

    auto numbers_copy = numbers;
    numbers_copy[3] = 20;
    std::cout << numbers[3] << std::endl;

    return 0;
}
```

Take some time to look at all of the improvements `std::array` has over C-style arrays:

- Automatically manages the size of the array. We can use `.size()` and `.empty()` methods to query the size and whether it is empty.
- Provides `.begin()` and `.end()` iterators to the beginning and end of the array, so we can use a for-each loop.
- Can be simply copied.

For more information about `std::array`, see [its entry in this wonderful resource](https://en.cppreference.com/w/cpp/container/array).

Both static arrays and `std::array` have a fixed size, which must be known at compile time. This can be limiting, especially when you need a dynamic, resizable container for your data. For instance, how would you write a program that could take in any number of values from the user and calculate a simple average?

## Introduction to `std::vector`
`std::vector` is a dynamic container that can automatically resize itself to accommodate new elements. It offers random access, similar to an array, while providing the flexibility to grow or shrink during runtime:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    numbers.push_back(6);
    numbers.push_back(7);
    numbers.push_back(8);
    numbers.push_back(9);

    for (auto num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    numbers.erase(numbers.begin());
    std::cout << numbers[0] << std::endl;

    auto numbers_copy = numbers;
    numbers.clear();

    std::cout << "Is empty?: " << std::boolalpha << numbers.empty() << std::endl;
    std::cout << "Size of numbers: " << numbers.size() << std::endl;

    std::cout << "Is empty?: " << std::boolalpha << numbers_copy.empty() << std::endl;
    std::cout << "Size of numbers: " << numbers_copy.size() << std::endl;

    return 0;
}
```

In this example, we start with a `std::vector` of size 5 and then add several new elements using the `push_back()` function. We iterate through the vector with a for-each loop thanks to the vector's `.begin()` and `.end()` iterators. We erase a particular element from the vector. We copy it. We clear it's contents. And we check whether it is empty and how many elements it has inside.

For more information about `std::vector`, see [its entry in this wonderful resource](https://en.cppreference.com/w/cpp/container/vector).

## Algorithms
Both `std::array` and `std::vector` can be used with the Algorithms library. There are many, many useful functions in this library, but as a good starting point, look no further than `std::sort`:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<float> temperatures{ 36.7, 37.0, 37.1, 35.8, 34.9, 37.2, 37.5, 38.9, 20.4 };

    std::sort(temperatures.begin(), temperatures.end());

    for (auto t : temperatures) {
        std::cout << t << " ";
    }
    std::cout << std::endl;
}
```

## `std::array` vs `std::vector`

feature	| std::array | std::vector
---|---|---
**Size** |	Fixed at compile time | Dynamic at runtime
**Memory allocation** | Stack | Heap
**Performance** | Fastest possible (no dynamic allocation) | Fast (dynamic allocation)
**Flexibility**	| Limited | High
**Convenience** | `.size()` and `.at()` methods | Many useful methods

While both `std::array` and `std::vector` provide storage for a collection of elements, you should choose the appropriate container based on your specific use case:

Use `std::array` when the size of the container is fixed and known at compile time, and you need better performance due to its stack allocation.

Use `std::vector` when you need a dynamic container that can grow or shrink during runtime, and you can afford the slightly slower performance due to heap allocation.

Always prefer either over C-style arrays.

# Exercises

## Exercise 1
Select all the features that apply to `std::array`:

```
a) Fixed size at compile time
b) Dynamic size at runtime
c) Stack memory allocation
d) Heap memory allocation
e) Provides the .size() method
f) Provides the .push_back() method
```

---

```
Answer: a) Fixed size at compile time, c) Stack memory allocation, e) Provides the .size() method
```

## Exercise 2
Complete the following code snippet to declare a `std::vector` of integers named `numbers` and initialize it with the values 1, 2, and 3:

```cpp
#include <iostream>
#include <________>

int main() {
    ________________ numbers = {1, 2, 3};

    numbers.____________(4);
    for (auto num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

Output:
```cpp
1 2 3 4 
```

---

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3};

    numbers.push_back(4);
    for (auto num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

## Exercise 3
Complete the following code snippet to declare a `std::array` of integers named `numbers` and initialize it with the values 1, 2, and 3:

```cpp
#include <iostream>
#include <________>

int main() {
    ________________ numbers = {1, 2, 3};

    for (auto num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

Output:
```cpp
1 2 3 
```

---

```cpp
#include <iostream>
#include <array>

int main() {
    std::array<int, 3> numbers = {1, 2, 3};

    for (auto num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

## Exercise 4
Briefly explain the main differences between `std::array` and `std::vector`, and when you would use one over the other.

```
```

---

`std::array` has a fixed size determined at compile time and is allocated on the stack, while `std::vector` has a dynamic size that can change at runtime and is allocated on the heap. `std::array` typically has better performance due to its stack allocation, but `std::vector` is far more flexible with its dynamic sizing. Use `std::array` when you know the size of the container at compile time and need better performance. Use `std::vector` when you need a dynamic container that can grow or shrink during runtime, and you can afford the slightly slower performance due to heap allocation.

## Exercise 5
Complete the following program that uses a `std::vector` to store a series of integer scores from the user via `std::cin` and calculates and prints the average of all of the scores after each input. If the user inputs -1, the program will return.

```cpp
#include <iostream>
#include <vector>

bool getScoreFromUser(std::vector<int>& scores) {
    // TODO
}

float calculateAverage(const std::vector<int>& scores) {
    // TODO
}

void print(float average) {
    // TODO
}

int main() {
    std::vector<int> scores;

    for (;;) {
        auto run = getScoreFromUser(scores);
        if (!run) {
            break;
        }
        
        auto average = calculateAverage(scores);
        print(average);
    }

    return 0;
}
```

---

```cpp
#include <iostream>
#include <vector>

bool getScoreFromUser(std::vector<int>& scores) {
    int score{};

    std::cout << "Enter score (0 - 100): ";
    std::cin >> score;
    if (score == -1) {
        return false;
    }

    scores.push_back(score);

    return true;
}

float calculateAverage(const std::vector<int>& scores) {
    long sum{};
    for (auto score : scores) {
        sum += score;
    }

    float average{ static_cast<float>(sum) / scores.size() };
    return average;
}

void print(float average) {
    std::cout << "Average score: " << average << std::endl;
}

int main() {
    std::vector<int> scores;

    for (;;) {
        auto run = getScoreFromUser(scores);
        if (!run) {
            break;
        }
        
        auto average = calculateAverage(scores);
        print(average);
    }

    return 0;
}
```