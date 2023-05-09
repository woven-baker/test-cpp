# Challenges

## Challenge 1
Given an integer variable `x`, write a code snippet to print its memory address. Then, create an integer pointer `p` that points to the address of `x`, and print the value stored at the address pointed to by `p`.

```cpp
#include <iostream>

int main() {
    int x = 42;

    // TODO
}
```

---

```cpp
#include <iostream>

int main() {
    int x = 42;

    std::cout << "Address of x: " << &x << std::endl;
    int *p = &x;
    std::cout << "Value pointed by p: " << *p << std::endl;
}
```

## Challenge 2
Create a function `swap_int` that takes two integer pointers as its arguments and swaps the values they point to. Call the function from the main function and verify that the values are swapped.

```cpp
#include <iostream>

// TODO

int main() {
    int a = 10;
    int b = 20;

    std::cout << "Before swap: a = " << a << ", b = " << b << std::endl;
    // TODO
    std::cout << "After swap: a = " << a << ", b = " << b << std::endl;
}
```

---

```cpp
#include <iostream>

void swap_int(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int a = 10;
    int b = 20;

    std::cout << "Before swap: a = " << a << ", b = " << b << std::endl;
    swap_int(&a, &b);
    std::cout << "After swap: a = " << a << ", b = " << b << std::endl;
}
```

## Challenge 3
Create a function `double_elements` that takes a `std::vector<int>` as its argument and returns a new `std::vector<int>` with each element doubled. Then, use a range-based for loop to print the elements of both the original and the new vector.

```cpp
#include <iostream>
#include <vector>

// TODO

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // TODO
}
```

---

```cpp
#include <iostream>
#include <vector>

std::vector<int> double_elements(const std::vector<int> &input) {
    std::vector<int> output;
    output.reserve(input.size());

    for (int element : input) {
        output.push_back(element * 2);
    }

    return output;
}

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    std::vector<int> doubled_numbers = double_elements(numbers);

    std::cout << "Original vector: ";
    for (int element : numbers) {
        std::cout << element << ' ';
    }
    std::cout << std::endl;

    std::cout << "Doubled vector: ";
    for (int element : doubled_numbers) {
        std::cout << element << ' ';
    }
    std::cout << std::endl;
}
```

## Challenge 4
Create a function `safe_sqrt` that takes a `double` as its argument and returns the square root of the number. If the argument is negative, the function should throw a `std::domain_error` with the message "Cannot compute the square root of a negative number". In the main function, call `safe_sqrt` with both a positive and a negative number, handling the exception appropriately.

```cpp
#include <cmath>
#include <iostream>
#include <stdexcept>

// TODO

int main() {
    double positive_number { 9.0 };
    double negative_number { 4.0 };

    // TODO
}
```

---

```cpp
#include <cmath>
#include <iostream>
#include <stdexcept>

double safe_sqrt(double x) {
    if (x < 0) {
        throw std::domain_error("Cannot compute the square root of a negative number");
    }
    return std::sqrt(x);
}

int main() {
    double positive_number { 9.0 };
    double negative_number { -4.0 };

    try {
        std::cout << "Sqrt of " << positive_number << " is: " << safe_sqrt(positive_number) << std::endl;

        std::cout << "Sqrt of " << negative_number << " is: " << safe_sqrt(negative_number) << std::endl;
    } catch (const std::domain_error &e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

## Challenge 5
The following code has a bug. Fix the bug.

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

const int* find_maximum(const std::vector<int>& numbers) {
    if (numbers.empty()) {
        throw std::runtime_error("Empty vector has no maximum value");
    }

    const int* max_ptr = &numbers[0];
    for (int number : numbers) {
        if (number > *max_ptr) {
            max_ptr = &number;
        }
    }

    return max_ptr;
}

int main() {
    std::vector<int> non_empty_vector = {1, 5, 3, 8, 2}; 
    std::vector<int> empty_vector;

    try {
        auto max_ptr = find_maximum(non_empty_vector);
        std::cout << "Maximum value in the non-empty vector is: " << *max_ptr << std::endl;

        max_ptr = find_maximum(empty_vector);
        std::cout << "Maximum value in the empty vector is: " << *max_ptr << std::endl;
    } catch (const std::runtime_error &e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

---

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

const int* find_maximum(const std::vector<int>& numbers) {
    if (numbers.empty()) {
        throw std::runtime_error("Empty vector has no maximum value");
    }

    const int* max_ptr = &numbers[0];
    for (const int& number : numbers) {
        if (number > *max_ptr) {
            max_ptr = &number;
        }
    }

    return max_ptr;
}

int main() {
    std::vector<int> non_empty_vector = {1, 5, 3, 8, 2}; 
    std::vector<int> empty_vector;

    try {
        auto max_ptr = find_maximum(non_empty_vector);
        std::cout << "Maximum value in the non-empty vector is: " << *max_ptr << std::endl;

        max_ptr = find_maximum(empty_vector);
        std::cout << "Maximum value in the empty vector is: " << *max_ptr << std::endl;
    } catch (const std::runtime_error &e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

## Challenge 6
Create a function `element_at` that takes a `std::vector<int>` and an integer index as its arguments. The function should return the element of the vector at the specified index. If the index is out of bounds, throw a `std::out_of_range` exception with a suitable error message. In the main function, call `element_at` with both valid and invalid indices, handling the exception appropriately.

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

// TODO

int main() {
    std::vector<int> numbers = {1, 3, 5, 7, 9};

    // TODO
}
```

---

```cpp
int element_at(const std::vector<int> &numbers, int index) {
    if (index < 0 || index >= static_cast<int>(numbers.size())) {
        throw std::out_of_range("Index is out of bounds");
    }
    return numbers[index];
}

int main() {
    std::vector<int> numbers = {1, 3, 5, 7, 9};

    try {
        int valid_index = 2;
        std::cout << "Element at index " << valid_index << " is: " << element_at(numbers, valid_index) << std::endl;

        int invalid_index = 10;
        std::cout << "Element at index " << invalid_index << " is: " << element_at(numbers, invalid_index) << std::endl;
    } catch (const std::out_of_range &e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

## Challenge 7

Look up the documentation for `std::vector` on cppreference.com to help answer this question, if needed. What is the output of the following code?

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers;

    std::cout << "Initial size: " << numbers.size() << ", capacity: " << numbers.capacity() << std::endl;

    numbers.push_back(1);
    numbers.push_back(2);
    numbers.push_back(3);
    std::cout << "After adding elements: size: " << numbers.size() << ", capacity: " << numbers.capacity() << std::endl;

    numbers.reserve(10);
    std::cout << "After reserving space: size: " << numbers.size() << ", capacity: " << numbers.capacity() << std::endl;
}
```

---

```
Initial size: 0, capacity: 0
After adding elements: size: 3, capacity: 4
After reserving space: size: 3, capacity: 10
```