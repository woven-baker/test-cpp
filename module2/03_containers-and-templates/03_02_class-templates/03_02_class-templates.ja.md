---
コンセプト: C++のクラステンプレート
必要な知識:
  - C++の関数
  - C++のテンプレート
  - C++のクラス
  - C++のオブジェクト指向プログラミング
---

# C++のクラステンプレート
関数テンプレートを使用すると、さまざまなデータ型で利用可能な関数を作成できます。これと同様に、クラステンプレートを使用すると、さまざまなデータ型で利用可能なクラスを作成できます。クラステンプレートは、さまざまなデータ型を格納できるコンテナやデータ構造を定義する必要がある場合に特に便利です。

次のシンプルなスタッククラスの例について考えてみましょう。

```cpp
#include <iostream>
#include <vector>

class IntStack {
private:
    std::vector<int> data;

public:
    void push(int value) {
        data.push_back(value);
    }

    int pop() {
        if (!data.empty()) {
            int value = data.back();
            data.pop_back();
            return value;
        } else {
            std::cerr << "Error: Stack is empty.\n";
            exit(EXIT_FAILURE);
        }
    }

    bool empty() const {
        return data.empty();
    }
};

int main() {
    IntStack stack;
    stack.push(1);
    stack.push(2);
    stack.push(3);

    while (!stack.empty()) {
        std::cout << stack.pop() << '\n';
    }

    return 0;
}
```

このIntStackクラスは整数のみを処理できます。他のデータ型に対応する同様のスタックを作成する場合は、各型に対応する別々のクラスを作成する必要があるため、コードの重複が多くなります。

これを避けるために、任意のデータ型に対応するスタックのクラステンプレートを作成します。

```cpp
#include <iostream>
#include <vector>

template<typename T>
class Stack {
private:
    std::vector<T> data;

public:
    void push(T value) {
        data.push_back(value);
    }

    T pop() {
        if (!data.empty()) {
            T value = data.back();
            data.pop_back();
            return value;
        } else {
            std::cerr << "Error: Stack is empty.\n";
            exit(EXIT_FAILURE);
        }
    }

    bool empty() const {
        return data.empty();
    }
};

int main() {
    Stack<int> intStack;
    intStack.push(1);
    intStack.push(2);
    intStack.push(3);

    while (!intStack.empty()) {
        std::cout << intStack.pop() << '\n';
    }

    Stack<double> doubleStack;
    doubleStack.push(1.1);
    doubleStack.push(2.2);
    doubleStack.push(3.3);

    while (!doubleStack.empty()) {
        std::cout << doubleStack.pop() << '\n';
    }

    Stack<std::string> stringStack;
    stringStack.push("Hello");
    stringStack.push("World");
    stringStack.push("!");

    while (!stringStack.empty()) {
        std::cout << stringStack.pop() << '\n';
    }

    return 0;
}
```

この例では`IntStack`クラスを、テンプレートパラメータ`T`を持つクラステンプレート`Stack`に置き換えました。これにより、`int`、`double`、`std::string`、独自のユーザー定義型などのさまざまなデータ型に対応するスタックオブジェクトを作成できます。データ型ごとに別々のクラスを作成する必要はありません。

## クラステンプレートの特殊化

クラステンプレートを使用しているときに、特定のデータ型専用の機能を実装したい場合があります。このような場合は、クラステンプレートを特殊化できます。

たとえば、`ブール`型用に`Stack`クラステンプレートの特殊なバージョンを作成し、メモリを節約するためにブール値をビットで格納するとします。次のようにクラステンプレートを特殊化します。

```cpp
template <>
class Stack<bool> {
private:
    std::vector<unsigned char> data;
    size_t bitIndex;

public:
    Stack() : bitIndex(0) {}

    void push(bool value) {
        if (bitIndex == 0) {
            data.push_back(0);
        }

        if (value) {
            data.back() |= (1 << bitIndex);
        }

        bitIndex = (bitIndex + 1) % 8;
    }

    bool pop() {
        if (!data.empty()) {
            bool value = data.back() & (1 << (bitIndex - 1));
            bitIndex = (bitIndex + 7) % 8;

            if (bitIndex == 7) {
                data.pop_back();
            }

            return value;
        } else {
            std::cerr << "Error: Stack is empty. Cannot pop a value." << '\n';
            return false;
        }
    }
};
```

これで、ブール値をビットで格納するために最適化された特殊なバージョンの`Stack`クラステンプレートができました。このバージョンの`Stack`クラスは、`ブール`値のスタックを作成するときに使用されます。

```cpp
int main() {
    Stack<int> intStack;
    intStack.push(42);
    std::cout << intStack.pop() << std::endl;

    Stack<double> doubleStack;
    doubleStack.push(3.14);
    std::cout << doubleStack.pop() << std::endl;

    Stack<std::string> stringStack;
    stringStack.push("Hello");
    stringStack.push("World");
    std::cout << stringStack.pop() << std::endl;
    std::cout << stringStack.pop() << std::endl;

    Stack<bool> boolStack;
    boolStack.push(true);
    boolStack.push(false);
    std::cout << std::boolalpha << boolStack.pop() << std::endl;
    std::cout << std::boolalpha << boolStack.pop() << std::endl;

    return 0;
}
```

# 演習

## 演習1
C++のクラステンプレートを使用する目的は何ですか。

```
A. さまざまなデータ型で利用可能なクラスを作成するため
B. さまざまなデータ型で利用可能な関数を作成するため
C. 特定のデータ型専用のクラスを作成するため
D. データをカプセル化し、さまざまな実装に対応する単一のインターフェイスを提供するため
```

---

```
A
```

## 演習2
次の関数テンプレートについて考えてみましょう。

```cpp
template<typename T>
class Array {
private:
    T *data;
    size_t size;

public:
    Array(size_t s) : data(new T[s]), size(s) {}

    T& operator[](size_t index) {
        return data[index];
    }

    ~Array() {
        delete[] data;
    }
};
```

5個の`浮動小数点`の値を格納する`Array`オブジェクトを初期化できるのは、次のどのコードスニペットですか。

```cpp
A. Array<float> arr(5);
B. Array<float[5]> arr;
C. Array<float, 5> arr;
D. Array<float> arr[5];
```

---

```
A
```

## 演習3
任意の型の2つの同じ値を格納できるPairクラステンプレートを作成します。カンマで区切った2つの値を出力する`print()`メンバ関数を実装してください。

---

```cpp
#include <iostream>

template<typename T>
class Pair {
private:
    T first;
    T second;

public:
    Pair(T firstValue, T secondValue) : first(firstValue), second(secondValue) {}

    void print() const {
        std::cout << first << ", " << second << std::endl;
    }
};

int main() {
    Pair<int> intPair(1, 2);
    intPair.print();

    Pair<double> doublePair(3.14, 1.59);
    doublePair.print();

    Pair<std::string> stringPair("Hello", "World");
    stringPair.print();

    return 0;
}
```

## 演習4
前のPairクラステンプレートを変更して、任意の型の2つの値(整数型と文字列型など)を格納できるようにします。同じ型の2つの値を使用した既存の形(`Pair<int>`)でも機能するようにします。この演習問題を解くには、 *[デフォルトテンプレート引数](https://ja.cppreference.com/w/cpp/language/template_parameters)* について調べ、これを適用する必要があります。変更する必要があるのは1行だけです。

---

```cpp
#include <iostream>

template<typename T, typename U = T>
class Pair {
private:
    T first;
    U second;

public:
    Pair(T firstValue, U secondValue) : first(firstValue), second(secondValue) {}

    void print() const {
        std::cout << first << ", " << second << std::endl;
    }
};

int main() {
    Pair<int> intPair{1, 2};
    intPair.print();

    Pair<double> doublePair{3.14, 1.59};
    doublePair.print();

    Pair<std::string> stringPair{"Hello", "World"};
    stringPair.print();

    Pair intStringPair{1, "Hello, world!"};
    intStringPair.print();

    return 0;
}
```

## 演習5
次のMatrixクラステンプレートについて考えてみましょう。

```cpp
template<typename T, size_t R, size_t C>
class Matrix {
private:
    T data[R][C];

public:
    T& at(size_t row, size_t col) {
        return data[row][col];
    }

    size_t rows() const {
        return R;
    }

    size_t cols() const {
        return C;
    }
};
```

Matrixクラステンプレートでは、行数(R)と列数(C)に*非型テンプレートパラメータ*を使用しています。[*非型テンプレートパラメータ*](https://ja.cppreference.com/w/cpp/language/template_parameters)について調査し、次の質問に答えてください。

1. *非型テンプレートパラメータ*とはどのようなもので、*型テンプレートパラメータ*とどのように異なりますか。
2. *非型テンプレートパラメータ*は任意のデータ型に対応できますか。できない場合、どのような制限がありますか。

---

1. 非型テンプレートパラメータは、型ではなく値を表すテンプレートパラメータです。「typename T」などの型テンプレートパラメータを使用すると、さまざまなデータ型で機能するクラスや関数を作成できますが、非型テンプレートパラメータを使用すると、異なる定数値で機能するクラスや関数を作成できます。

2. 非型テンプレートパラメータは任意のデータ型に対応できません。次のいずれかである必要があります。

  - 整数型または列挙型(int、size_t、bool、ユーザー定義の列挙型など)
  - オブジェクトまたは関数へのポインタ型(メンバへのポインタを含む)
  - std::nullptr_t (例: nullptr)
  - オブジェクトまたは関数への参照
  - std::integral_constantまたは同様の型

  非型テンプレートパラメータは定数式にする必要があることに留意してください。つまり、コンパイル時に値がわかっている必要があります。