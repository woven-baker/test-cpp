---
コンセプト: C++の可変引数テンプレート
必要な知識:
  - C++の関数
  - C++のテンプレート
  - C++のクラス
  - C++の関数テンプレート
  - C++のクラステンプレート
  - C++の再帰
---

# C++の可変引数テンプレート
可変引数テンプレートはC++11以降の機能で、任意の数の引数を受け取るテンプレートを定義できます。これを使用して、任意の数の型や値を処理する関数とクラスを作成できます。可変引数テンプレートを使用すると、コードが大幅にシンプルになり、汎用的で再利用可能なものになります。

## 可変引数関数テンプレート
次のような可変引数の関数テンプレートの例について考えてみます。

```cpp
#include <iostream>

void print() {
    std::cout << '\n';
}

template<typename T, typename... Args>
void print(const T& value, const Args&... args) {
    std::cout << value;
    if constexpr(sizeof...Args > 0) {
        std::cout << ", ";
    }
    print(args...);
}

int main() {
    print(1, 2.5, "Hello", 3, "World");
    return 0;
}
```

このコードの出力はどのようになりますか。

---

```
1, 2.5, Hello, 3, World
```

このprint関数は、カンマで区切られた*任意の数*の値を出力できます。

`typename...Args`構文のArgs(任意の名前に変更可能)は、任意の数の型引数を取ることができる`テンプレートパラメータパック`です。`args...`構文は、print関数を再帰呼び出しする際にテンプレートパラメータパックを展開する目的で使用されます。

再帰呼び出しがどのように機能するかを理解するために、print関数テンプレートを一連のオーバーロード関数と考えることができます。

```cpp
void print();
void print(const int&);
void print(const int&, const double&);
void print(const int&, const double&, const char*);
// ... and so on.
```

print関数を呼び出すときに指定した引数に応じて、コンパイラは随時これらのオーバーロード関数を生成します。この例では、引数(1, 2.5, "Hello", 3, "World")でprint関数を呼び出しています。 

これにより、次の一連の呼び出しが生成されます。

```cpp
print(1, 2.5, "Hello", 3, "World")
print(2.5, "Hello", 3, "World")
print("Hello", 3, "World")
print(3, "World")
print("World")
print()
```

再帰の基本関数はvoid print()です。引数を持たず、改行文字を出力するだけです。

では、このprint関数を変更して次のように出力されるようにしましょう。

```
(1) (2.5) (Hello) (3) (World)
```

---

```cpp
#include <iostream>

void print() {
    std::cout << '\n';
}

template<typename T, typename... Args>
void print(const T& value, const Args&... args) {
    std::cout << '(' << value << ')';
    if (sizeof...(Args) > 0) {
        std::cout << " ";
    }
    print(args...);
}

int main() {
    print(1, 2.5, "Hello", 3, "World");
    return 0;
}
```

## 可変引数クラステンプレート

可変引数テンプレートは、クラステンプレートでも使用できます。

たとえば、シンプルなイベントシステムを作成し、各イベントには引数の数が異なる複数のリスナーを持てるようにします。リスナーを格納するために可変引数クラステンプレートを使用し、任意の数の引数で呼び出すことができます。

```cpp
#include <iostream>
#include <vector>
#include <functional>

template<typename... Args>
class Event {
public:
    using Listener = std::function<void(Args...)>;

private:
    std::vector<Listener> listeners;

public:
    void addListener(const Listener& listener) {
        listeners.push_back(listener);
    }

    void notify(Args... args) {
        for (auto& listener : listeners) {
            listener(args...);
        }
    }
};

void onPlayerMoved(int x, int y) {
    std::cout << "Player moved to (" << x << ", " << y << ")" << std::endl;
}

void onPlayerJumped() {
    std::cout << "Player jumped" << std::endl;
}

int main() {
    Event<int, int> playerMovedEvent;
    playerMovedEvent.addListener(onPlayerMoved);

    Event<> playerJumpedEvent;
    playerJumpedEvent.addListener(onPlayerJumped);

    playerMovedEvent.notify(5, 3);
    playerJumpedEvent.notify();

    return 0;
}
```

# 演習

## 演習1
C++の可変引数テンプレートの目的は何ですか。

```
A. 固定数の引数を持つテンプレートを作成するため
B. 可変数の引数を持つテンプレートを作成するため
C. 特定のデータ型用にテンプレートの特殊なバージョンを作成するため
D. クラス階層を作成し、クラス間の関係を確立するため
```

---

```
B
```

## 演習2
次の可変引数関数テンプレートについて考えてみます。

```cpp
template<typename... Args>
void printArguments(Args... args) {
    // ...
}
```

次の`printArguments`呼び出しのうち、有効なものはどれですか。

```
A. printArguments(1, 2, 3);
B. printArguments("hello", "world");
C. printArguments();
D. All of the above
```

---

```
D
```

## 演習3
任意の数の整数型引数を受け取り、それらの合計を返す可変引数関数テンプレート「sum」を作成してください。引数は整数で、引数の数は0より大きいものとします。

---

```cpp
#include <iostream>

template<typename... Args>
int sum(int head, Args... args){
    if constexpr (sizeof...(args) == 0) {
        return head;
    } else {
        return head + sum(args...);
    }
}

int main() {
    std::cout << "Sum of 1, 2, 3: " << sum(1, 2, 3) << std::endl;
    std::cout << "Sum of 4, 5, 6, 7: " << sum(4, 5, 6, 7) << std::endl;
    std::cout << "Sum of 8: " << sum(8) << std::endl;

    return 0;
}
```

## 演習4
任意の数のブール型引数を受け取り、すべての引数がtrueの場合はtrue、それ以外の場合はfalseを返す可変引数関数テンプレート「allTrue」を作成してください。引数が指定されていない場合は、trueを返すようにします。

使用例:

```cpp
// Should output: 1 (true)
std::cout << allTrue(true, true, true) << std::endl;
// Should output: 0 (false)
std::cout << allTrue(true, false, true) << std::endl;
// Should output: 1 (true)
std::cout << allTrue() << std::endl;
```

---

```cpp
#include <iostream>

// Base case for the recursion
bool allTrue() {
    return true;
}

// Recursive variadic function template
template<typename... Args>
bool allTrue(bool head, Args... args) {
    return head && allTrue(args...);
}

int main() {
    std::cout << "All true: " << allTrue(true, true, true) << std::endl;
    std::cout << "Not all true: " << allTrue(true, false, true) << std::endl;
    std::cout << "No arguments: " << allTrue() << std::endl;

    return 0;
}
```