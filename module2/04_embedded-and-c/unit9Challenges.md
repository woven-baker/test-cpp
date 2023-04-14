# Challenges

## Challenge 1
What are the mangled names for each of the following functions, from top to bottom?

```cpp
#include <string>
#include <vector>

int func(int a, int b) {
  return 4;
}

int func(const int a, int &b, const int &c) {
  return 4;
}

int func(float a, double b) {
  return 4;
}

int func(std::string a, char c) {
  return 4;
}

int func(int a[3]) {
  return 4;
}

int func(std::vector<int> a) {
  return 4;
}
```

```
```

***

```
__Z4funcii
__Z4funciRiRKi
__Z4funcfd
__Z4funcNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEEc
__Z4funcPi
__Z4funcNSt3__16vectorIiNS_9allocatorIiEEEE
```

## Challenge 2

Write a signature for a function in C++ that would have the following mangled name: `__Z10myFunctionhx`.

```
```

***

```cpp
bool myFunction(unsigned char a, long long b);
```