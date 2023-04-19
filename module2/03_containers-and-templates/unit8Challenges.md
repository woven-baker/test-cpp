# Challenges

## Challenge 1
Create a function template called print_elements that accepts a standard library container (like `std::vector`, `std::list`, or `std::deque`) and prints its elements. Test your function with at least two different types of containers.

```
```

***

```cpp
#include <iostream>
#include <vector>
#include <list>

template<typename Container>
void print_elements(const Container& container) {
    for (const auto& element : container) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::list<char> lst = {'a', 'b', 'c', 'd', 'e'};

    print_elements(vec);
    print_elements(lst);

    return 0;
}
```

## Challenge 2
Create a class template called `Stack` that mimics the behavior of a stack. This class should have the following member functions:
- `void push(const T& value)`, which pushes a value onto the stack.
- `void pop()`, which pops the top value off the stack.
- `T top() const`, which returns the value on the top of the stack.
- `bool is_empty() const`, which returns if the stack is empty.

Use a standard library container (e.g., `std::vector`, `std::list`, or `std::deque`) as the underlying data structure for the stack. Test your class with at least two different types of containers.

```
```

***

```cpp
#include <iostream>
#include <vector>
#include <list>

template<typename T, typename Container = std::vector<T>>
class Stack {
public:
    void push(const T& value) {
        data.push_back(value);
    }

    void pop() {
        if (!is_empty()) {
            data.pop_back();
        }
    }

    T top() const {
        if (!is_empty()) {
            return data.back();
        } else {
            throw std::runtime_error("Stack is empty");
        }
    }

    bool is_empty() const {
        return data.empty();
    }

private:
    Container data;
};

int main() {
    Stack<int> int_vector_stack;
    int_vector_stack.push(1);
    int_vector_stack.push(2);
    std::cout << int_vector_stack.top() << std::endl; // Output: 2
    int_vector_stack.pop();
    std::cout << int_vector_stack.top() << std::endl; // Output: 1
    
    Stack<char, std::list<char>> char_list_stack;
    char_list_stack.push('a');
    char_list_stack.push('b');
    std::cout << char_list_stack.top() << std::endl; // Output: b
    char_list_stack.pop();
    std::cout << char_list_stack.top() << std::endl; // Output: a

    return 0;
}
```

## Challenge 3
Compare the performance of `std::vector`, `std::list`, and `std::deque` when used as the underlying data structure for a stack. Write a small program that performs a series of push and pop operations on a stack, then time the execution for each container type. Determine which container is the best choice for this application and explain your choice. Make sure to compile with -O0 so the compiler doesn't optimize away your code and result in useless benchmarks.

```
```

***

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <deque>
#include <chrono>

constexpr int NUM_OPERATIONS = 100000;

template <typename T, typename Container>
class Stack {
public:
    void push(const T& value) {
        data.push_back(value);
    }

    void pop() {
        if (!is_empty()) {
            data.pop_back();
        }
    }

    T& top() {
        return data.back();
    }

    bool is_empty() const {
        return data.empty();
    }

private:
    Container data;
};

template<typename StackType>
void measure_performance() {
    StackType stack;
    auto start_time = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        stack.push(i);
    }

    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        stack.pop();
    }

    auto end_time = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(end_time - start_time).count();
    
    std::cout << "Time taken: " << duration << " microseconds" << std::endl;
}

int main() {
    std::cout << "Using std::vector:" << std::endl;
    measure_performance<Stack<int, std::vector<int>>>();

    std::cout << "Using std::list:" << std::endl;
    measure_performance<Stack<int, std::list<int>>>();

    std::cout << "Using std::deque:" << std::endl;
    measure_performance<Stack<int, std::deque<int>>>();

    return 0;
}
```

## Challenge 4
Design a program that reads a text file containing a list of names and phone numbers. The program should store the names and phone numbers in a `std::map` container. Implement the following features:

- Add a name and phone number to the map.
- Remove a name and its associated phone number from the map.
- Search for a phone number based on a given name.
- Display all names and phone numbers in alphabetical order.

In this challenge, you will need to research and understand how to use `std::map` and its associated functions. Also, you'll need to implement input validation and error handling to ensure the program works as expected.

```
```

***

```cpp
#include <iostream>
#include <string>
#include <map>
#include <fstream>

class PhoneBook {
public:
    void add_entry(const std::string& name, const std::string& number) {
        phone_book[name] = number;
    }

    void remove_entry(const std::string& name) {
        phone_book.erase(name);
    }

    std::string find_number(const std::string& name) const {
        auto it = phone_book.find(name);
        if (it != phone_book.end()) {
            return it->second;
        } else {
            return "Name not found";
        }
    }

    void display() const {
        for (const auto& entry : phone_book) {
            std::cout << entry.first << ": " << entry.second << std::endl;
        }
    }

    void load_from_file(const std::string& file_path) {
        std::ifstream input_file(file_path);

        if (!input_file) {
            std::cerr << "Error opening input file" << std::endl;
            return;
        }

        std::string name, number;
        while (input_file >> name >> number) {
            add_entry(name, number);
        }

        input_file.close();
    }

private:
    std::map<std::string, std::string> phone_book;
};

int main() {
    PhoneBook phone_book;
    phone_book.load_from_file("phone_numbers.txt");

    phone_book.display();

    std::cout << std::endl << "Adding a new entry (John, 555-1234):" << std::endl;
    phone_book.add_entry("John", "555-1234");
    phone_book.display();

    std::cout << std::endl << "Removing an entry (John):" << std::endl;
    phone_book.remove_entry("John");
    phone_book.display();

    std::cout << std::endl << "Searching for a number by name (Alice): ";
    std::string number = phone_book.find_number("Alice");
    std::cout << number << std::endl;

    return 0;
}
```