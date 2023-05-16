# 演習

## 演習1

```
B
```

## 演習2

```
A
```

## 演習3

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

出力:

```
1 3 5 7 9
```

## 演習4

```
D
```

## 演習5

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

出力:

```
4 2
```

## 演習6

```
E
```

A std::map container would be most appropriate because it allows for efficient lookup, insertion, and deletion of key-value pairs. In this case, the key would be the student's name, and the value would be the corresponding test score.
