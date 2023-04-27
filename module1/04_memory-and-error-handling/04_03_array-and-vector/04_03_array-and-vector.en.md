Static Arrays
Before diving into std::array, let's briefly discuss static arrays, which are a basic form of arrays in C++.

cpp
Copy code
#include <iostream>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    for (int i = 0; i < 5; i++) {
        std::cout << numbers[i] << " ";
    }

    return 0;
}
Static arrays have a fixed size, which must be defined during compilation. This can be limiting, especially when you need a dynamic, resizable container for your data.

Introduction to std::array
std::array is a C++ Standard Library container that provides a fixed-size, zero-overhead alternative to static arrays. Its size is determined at compile time, and it provides useful functions to work with the elements.

Here is an example of how to use std::array:

cpp
Copy code
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> numbers = {1, 2, 3, 4, 5};

    for (const auto& num : numbers) {
        std::cout << num << " ";
    }

    return 0;
}
Notice that the syntax is very similar to static arrays, with the added benefit of helpful functions like .size() and .at().

Introduction to std::vector
std::vector is a dynamic container that can automatically resize itself to accommodate new elements. It offers random access, similar to an array, while providing the flexibility to grow or shrink during runtime.

Here's an example of how to use std::vector:

cpp
Copy code
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    numbers.push_back(6);

    for (const auto& num : numbers) {
        std::cout << num << " ";
    }

    return 0;
}
In this example, we start with a std::vector of size 5 and then add a new element using the push_back() function. This demonstrates the dynamic nature of std::vector.

`std::array` vs `std::vector`

feature	| std::array | std::vector
---|---|---
**Size** |	Fixed at compile time | Dynamic at runtime
**Memory allocation** | Stack | Heap
**Performance** | Fastest possible (no dynamic allocation) | Fast (dynamic allocation)
**Flexibility**	| Limited | High
**Convenience** | .size() and .at() methods | Many useful functions

While both std::array and std::vector provide storage for a collection of elements, you should choose the appropriate container based on your specific use case:

Use std::array when the size of the container is fixed and known at compile time, and you need better performance due to its stack allocation.
Use std::vector when you need a dynamic container that can grow or shrink during runtime, and you can afford the slightly slower performance due to heap allocation.

# Exercises

## Exercise 1
Select all the features that apply to std::array:

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