---
concept: "C++ Standard Library Containers"
author: Alexander Bolinsky
description: "Learn about various C++ standard library containers and their usage."
prerequisites:
  - "C++ Basics"
  - "C++ Functions"
learning_outcomes:
  - "Explain the purpose and functionality of C++ standard library containers"
  - "Demonstrate the usage of std::vector"
  - "Demonstrate the usage of std::list"
  - "Demonstrate the usage of std::deque"
  - "Demonstrate the usage of std::set"
  - "Demonstrate the usage of std::map"
  - "Compare the differences between the containers and select the appropriate container for a given situation"
---

# C++ Standard Library Containers
C++ Standard Library provides a variety of containers that allow you to store and manipulate data efficiently. Containers are classes that can store and manage a collection of objects. Each container has its unique properties and is designed to handle specific use cases.

There are three main categories of containers in the C++ Standard Library:

- Sequence containers
- Associative containers
- Unordered associative containers

## Sequence Containers
Sequence containers store elements in a linear sequence. They are designed to provide fast access to individual elements and offer efficient insertion and deletion of elements. The main sequence containers are:

- `std::vector`: Dynamic array that can grow or shrink in size. Provides fast random access and is efficient in terms of memory usage.
- `std::list`: Doubly-linked list that allows fast insertion and deletion of elements at any position within the list.
- `std::deque`: Double-ended queue that provides fast insertion and deletion at both ends of the container.

## Associative Containers
Associative containers store elements in a sorted order, which enables fast searches, insertions, and deletions. The elements are sorted by a key, and each element has a unique key. The main associative containers are:

- `std::set`: A collection of unique keys, sorted by the key.
- `std::map`: A collection of key-value pairs, with unique keys, sorted by the key.
- `std::multiset`: A collection of keys, sorted by the key, allowing multiple occurrences of the same key.
- `std::multimap`: A collection of key-value pairs, sorted by the key, allowing multiple key-value pairs with the same key.

## Unordered Associative Containers
Unordered associative containers store elements in an unordered manner and provide fast access to individual elements using a hash function. The main unordered associative containers are:

- `std::unordered_set`: A collection of unique keys, hashed by the key.
- `std::unordered_map`: A collection of key-value pairs, with unique keys, hashed by the key.
- `std::unordered_multiset`: A collection of keys, hashed by the key, allowing multiple occurrences of the same key.
- `std::unordered_multimap`: A collection of key-value pairs, hashed by the key, allowing multiple key-value pairs with the same key.

# Using Containers
Containers can be used with any data type, including user-defined types. You can use the provided member functions to manipulate the data within the container. The containers also work with iterators, which enable you to traverse the elements in a container.

Here's an example of using a std::vector to store and manipulate integer values:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers;

    for (int i = 1; i <= 5; ++i) {
        numbers.push_back(i);
    }

    for (size_t i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

Output:

```
1 2 3 4 5
```

This simple example demonstrates how to create a vector of integers, add elements to the vector using `push_back()`, and access the elements using the array indexing operator `[]`.

Here's an example of using a `std::map` to store and manipulate a dictionary of words and their frequencies:

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map<std::string, int> word_frequency;

    word_frequency["apple"] = 3;
    word_frequency["banana"] = 2;
    word_frequency["orange"] = 1;

    std::cout << "Frequency of 'apple': " << word_frequency["apple"] << '\n';

    for (const auto &entry : word_frequency) {
        std::cout << entry.first << ": " << entry.second << '\n';
    }

    return 0;
}
```

Output:

```
Frequency of 'apple': 3
apple: 3
banana: 2
orange: 1
```

This example demonstrates how to create a map of strings (words) and integers (frequencies), add key-value pairs to the map, access the value using the key, and iterate through the map using a range-based for loop.

# Choosing the Right Container

In order to choose the most appropriate container for a specific use case, it's important to understand the strengths and weaknesses of each container type. Here, we will explore various situations and identify which container would be the best fit.

## Fast insertion and deletion at both ends
**Scenario**: Imagine you are building a simple web server that maintains a list of incoming requests. The server processes requests in a first-in, first-out (FIFO) manner, and new requests are added to the end of the queue.

**Solution**: `std::deque` is the most appropriate container for this use case, as it allows for fast insertion at the back and fast removal at the front.

```cpp
#include <iostream>
#include <deque>

int main() {
    std::deque<std::string> request_queue;

    request_queue.push_back("Request 1");
    request_queue.push_back("Request 2");
    request_queue.push_back("Request 3");

    while (!request_queue.empty()) {
        std::cout << "Processing: " << request_queue.front() << '\n';
        request_queue.pop_front();
    }

    return 0;
}
```

## Efficient sorting and removal of duplicates
**Scenario**: You are building a program to analyze a large text file and generate a sorted list of unique words found in the file.

**Solution**: `std::set` is the most appropriate container for this use case, as it efficiently sorts elements and removes duplicates.

```cpp
#include <iostream>
#include <set>
#include <sstream>

int main() {
    std::string text = "apple banana apple orange orange banana grape";
    std::istringstream iss(text);
    std::set<std::string> unique_words;

    for (std::string word; iss >> word;) {
        unique_words.insert(word);
    }

    for (const std::string& word : unique_words) {
        std::cout << word << " ";
    }

    return 0;
}
```

## Fast random access to elements
**Scenario**: You are building an application to analyze sensor data. The sensor records data points at fixed intervals, and the application must store these data points, calculate some statistics like the average, and retrieve values at specific positions.

**Solution**: `std::vector` is the most appropriate container for this use case, as it provides efficient contiguous memory allocation, fast access to elements using indices, and dynamic resizing when needed.

```cpp
#include <iostream>
#include <vector>
#include <numeric>
#include <algorithm>

double calculate_average(const std::vector<double>& data) {
    if (data.empty()) return 0.0;
    double sum = std::accumulate(data.begin(), data.end(), 0.0);
    return sum / data.size();
}

int main() {
    std::vector<double> sensor_data;

    sensor_data.push_back(20.5);
    sensor_data.push_back(21.3);
    sensor_data.push_back(19.8);
    sensor_data.push_back(22.1);

    double average = calculate_average(sensor_data);
    std::cout << "Average sensor data value: " << average << std::endl;

    double third_data_point = sensor_data[2];
    std::cout << "Third data point: " << third_data_point << std::endl;

    std::sort(sensor_data.begin(), sensor_data.end());

    std::cout << "Sorted sensor data: ";
    for (double data_point : sensor_data) {
        std::cout << data_point << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

## Efficient insertion and deletion in the middle - std::list
**Scenario**: You are building a program to manage a playlist of songs, where users can add or remove songs at any position in the playlist.

**Solution**: `std::list` is the most appropriate container for this use case, as it allows for efficient insertion and deletion of elements in the middle of the sequence.

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<std::string> playlist = {"Song A", "Song B", "Song C"};

    playlist.insert(std::next(playlist.begin()), "Song D");

    playlist.erase(std::next(playlist.begin()));

    for (const std::string& song : playlist) {
        std::cout << song << '\n';
    }

    return 0;
}
```

## Mapping keys to values
**Scenario**: You are building an application to manage an inventory for a store. You need to associate product names with their respective stock counts.

**Solution**: `std::map` is the most appropriate container for this use case, as it efficiently maintains a mapping of keys to values, with unique keys.

```cpp
#include <iostream>
#include <map>

int main() {
    std::map<std::string, int> inventory{
        {"Apples", 10},
        {"Oranges", 5},
        {"Bananas", 7}
    };

    inventory["Grapes"] = 15;

    inventory["Apples"] = 12;

    for (const auto& [product, stock] : inventory) {
        std::cout << product << ": " << stock << '\n';
    }

    return 0;
}

```

# Exercises
## Exercise 1
What is the output of the following code:
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers{ 10, 20, 30, 40, 50 };

    for (int i = numbers.size() - 1; i >= 0; --i) {
        std::cout << numbers[i] << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

```
A: 10 20 30 40 50

B: 50 40 30 20 10

C: 50 40 30 20

D: Compiler Error
```

***

```
B
```

## Exercise 2
Given the following code, what is the output?

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map<std::string, int> ages;
    ages["Alice"] = 28;
    ages["Bob"] = 35;
    ages["Charlie"] = 22;

    std::cout << ages["Diana"] << '\n';

    return 0;
}
```

```
A: 0

B: 1

C: -1

D: Compiler Error
```

***

```
A
```

## Exercise 3
Write a program that creates a `std::set` of integers and adds the numbers 1 to 10 to the set. Then, remove all even numbers from the set and print the remaining elements.

```
```

***

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> numbers;

    for (int i = 1; i <= 10; ++i) {
        numbers.insert(i);
    }

    for (int i = 2; i <= 10; i += 2) {
        numbers.erase(i);
    }

    for (const auto &num : numbers) {
        std::cout << num << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

Output:

```
1 3 5 7 9
```

## Exercise 4
Consider the following code that uses a `std::list` of integers. What is the output?

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> numbers{ 1, 2, 3, 4, 5 };
    auto iter = numbers.begin();
    std::advance(iter, 2);

    numbers.insert(iter, 10);
    numbers.reverse();

    for (const auto &num : numbers) {
        std::cout << num << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

```
A: 1 2 3 4 5 10

B: 1 2 10 3 4 5

C: 5 4 3 10 2 1

D: 5 4 10 3 2 1
```

***

```
D
```

## Exercise 5
Write a program that creates a `std::deque` of integers and adds the numbers 1 to 5 to the front of the deque. Then, remove all odd numbers from the deque and print the remaining elements.

```
```

***

```cpp
#include <iostream>
#include <deque>

int main() {
    std::deque<int> numbers;

    for (int i = 1; i <= 5; ++i) {
        numbers.push_front(i);
    }

    for (auto iter = numbers.begin(); iter != numbers.end();) {
        if (*iter % 2 != 0) {
            iter = numbers.erase(iter);
        } else {
            ++iter;
        }
    }

    for (const auto &num : numbers) {
        std::cout << num << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

Output:

```
4 2
```

## Exercise 6
You are building a program to manage a list of student names and their corresponding test scores. You need to be able to quickly search for a student's score using their name, and you also need to be able to update a student's score. Which container would be the most appropriate for this use case?

```
A. std::vector

B. std::list

C. std::deque

D. std::set

E. std::map
```

```
```

***

```
E

A std::map container would be most appropriate because it allows for efficient lookup, insertion, and deletion of key-value pairs. In this case, the key would be the student's name, and the value would be the corresponding test score.
```