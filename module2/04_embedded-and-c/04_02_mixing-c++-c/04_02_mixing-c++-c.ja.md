---
コンセプト: C++とCを組み合わせて使用する
必要な知識:
  - C++関数
  - C++クラス
  - C++参照
  - C++ポインタ
  - C++構造体
  - C関数
  - C構造体
  - C++ヘッダーファイル
  - C++コンパイル
  - C++リンク
---

# C++とCを組み合わせて使用する

組み込みソフトウェアの開発では、C++コードと既存のCコードを接続しなければならないことがよくあります。

## 名前マングリング

名前マングリング(名前修飾とも呼ばれます)は、プログラムの各関数と変数に一意の名前を生成するためにコンパイラが使用する手法です。これは、同じ名前を持つ複数の関数が存在するが、引数の型が異なる場合に、C++などの言語で関数のオーバーロードをサポートするためのものです。

ただし、Cは関数のオーバーロードをサポートしていないため、名前マングリングは必要ありません。そのため、CコンパイラとC++コンパイラで異なる命名スキームが使用されます。

次のC++関数があるとします。

```cpp
// math.cpp
int add(int a, int b) {
    return a + b;
}
```

C++コンパイラは、このコードをコンパイルするときに、関数名をマングルして、引数の型に関する情報を含めます。マングルされた名前は`_Z3addii`のようになります。この名前を使って、関数`addが`2つの`int`引数を取るという内容をコード化します。

では、これに相当するC関数について考えてみましょう。

```c
// math.c
int add(int a, int b) {
    return a + b;
}
```

Cは関数のオーバーロードをサポートしていないため、Cコンパイラは関数名をマングルする必要はありません。コンパイルされた関数は同じ名前`add`になります。

## マングルされた名前を調べる
`nm` (Unix系のシステムで使用可能)などのツールを使用して生成したオブジェクトファイルを調査して、プログラムのマングルされたC++名とマングルされていないC名を調べられます。`nm`は、関数と変数の名前を含むオブジェクトファイルのシンボルテーブルを表示する、コマンドラインユーティリティです。

`nm`を使用して名前を調べてみましょう。

まず、CファイルおよびC++ファイルを、オブジェクトファイルにコンパイルします。

Cファイルの場合は、次を使用します。

```bash
gcc -c math.c -o math_c.o
```

C++ファイルの場合は、次を使用します。

```bash
g++ -c math.cpp -o math_cpp.o
```

次に、`nm`を使用してオブジェクトファイルを調べます。

Cオブジェクトファイルの場合は、次を使用します。

```bash
nm math_c.o
```

これにより、次のような出力が得られます(正確な出力は異なる場合があります)。

```bash
0000000000000000 T add
```

Tは、`add`がグローバル(外部)リンケージを持つ関数であることを示します。マングルされていない名前の追加が表示されます。

C++オブジェクトファイルの場合は、次を使用します。

```bash
nm math_cpp.o
```

これにより、次のような出力が得られます(正確な出力は異なる場合があります)。

```bash
0000000000000000 T _Z3addii
```

Tは、`_Z3addii`がグローバル(外部)リンケージを持つ関数であることを示します。マングルされたC++ 名_Z3addiiが表示されます。

マングルされた名前はコンパイラ固有のものであり、コンパイラやコンパイラのバージョンによって異なる可能性があることに注意してください。ただし、マングリングの目的は同じで、リンクプロセス中に使用できる関数と変数の一意の名前を生成します。

## extern "C"の紹介

この命名スキームの違いにより、C++コードでC関数を使用しようとした場合、またはその逆の場合にリンケージの問題が発生します。C++コードはマングルされた名前を想定していますが、C関数にはマングルされた名前がありません。この問題を解決するには、`extern "C"`を使用して、指定された関数がC命名スキームを使用し、名前をマングルしないようにC++コンパイラに指示します。こうすることで、C++コードをC関数と正しくリンクできます。

`extern "C"`を使用するには、次のいずれかを実行します。

1. **cppファイルから**C関数宣言の先頭に追加します

```cpp
// cpp file
extern "C" void function_a(int);
```

2. **cppファイルから**1つ以上のC関数宣言をラップします

```cpp
// cpp file
extern "C" {
    int function_b(double);
    double function_c();
};
```

3. **cppファイルから**インクルードしたヘッダーをラップします

```cpp
// cpp file
extern "C" {
    #include "my_c_header.h"
}
```

4. 関数宣言を**cヘッダーから**ラップします

```c
// c header
#ifndef MY_C_HEADER_H
#define MY_C_HEADER_H

// other included header files

#ifdef __cplusplus
extern "C" {
#endif

// function declarations

#ifdef __cplusplus
}
#endif

#endif // MY_C_HEADER_H
```

最後の方法をお勧めします。

なぜでしょうか。

---

Cヘッダーに`extern "C"`ロジックを追加すると、C++ユーザーはそれをC++ コードに簡単に`#include`できるようになり、インクルードされた場所に関わらず、ヘッダーは同じように動作します。Cコンパイラは`extern "C"`構造を理解しないため、`extern "C" {`および`}`行は `#ifdef`でラップする必要があります。これにより、通常のCコンパイラではなくC++コンパイラでのみ表示されるようになります。

Cヘッダーファイルにアクセスできるが変更できない場合は、次を使用します。

```
A. 1
B. 2
C. 3
D. 4
```

---

```
C
```

Cヘッダーファイルにアクセスできない場合は、次を使用します。

```
A. 1
B. 2
C. 3
D. 4
```

---

```
A or B
```

Cヘッダーファイルにアクセスして変更できる場合は、次を使用します。

```
A. 1
B. 2
C. 3
D. 4
```

---

```
D
```

これ以降、特に明記されていない限り、Cヘッダーファイルにアクセスして変更できることを前提としています。以下がよくあるケースです。

## 関数を接続する
C++プログラムからC関数を接続する例を見てみましょう。

次のC関数が`math_c.c`というファイルに定義されているとします。

```c
// math_c.c
int math_add(int a, int b) {
    return a + b;
}
```

次に、`math_c.h`というヘッダーファイルを作成して、このC関数を宣言します。

```c
// math_c.h
#ifndef MATH_C_H
#define MATH_C_H

int math_add(int a, int b);

#endif // MATH_C_H
```

このC関数をC++ファイルで使用するには、関数宣言をextern "C"でラップします。

```cpp
// math_c.h
#ifndef MATH_C_H
#define MATH_C_H

#ifdef __cplusplus
extern "C" {
#endif

int math_add(int a, int b);

#ifdef __cplusplus
}
#endif

#endif // MATH_C_H
```

これで、C++ファイルでadd関数を使用できます。

```cpp
// main.cpp
#include <iostream>
#include "math_c.h"

int main() {
    int sum = math_add(3, 4);
    std::cout << "Sum: " << sum << std::endl;
    return 0;
}
```

Cで`log_c.h`と`log_c.c`という名前の別の翻訳単位(ヘッダーとソースファイル)を作成して、練習してみましょう。それぞれに、voidを返し、単一の整数引数を取り、`printf`を使用して出力する`log_print`関数の宣言と定義が含まれています。C++プログラムから呼び出せるように宣言をラップし、上記のmain関数から`log_print`を呼び出し、`sum`に引数として渡します。

---

```c
// log_c.c
#include <stdio.h>

void log_print(int a) {
    printf("%i\n", a);
}
```

```c
// log_c.h
#ifndef LOG_C_H
#define LOG_C_H

#ifdef __cplusplus
extern "C" {
#endif

void log_print(int a);

#ifdef __cplusplus
}
#endif

#endif // LOG_C_H
```

```cpp
// main.cpp
#include <iostream>
#include "math_c.h"
#include "log_c.h"

int main() {
    int sum = math_add(3, 4);
    std::cout << "Sum: " << sum << std::endl;
    log_print(sum);
    return 0;
}
```

## C++コンテナからCコードにデータを渡す
`std::array`、`std::vector`などのC++コンテナは、C関数と直接互換性がありません。ただし、データへのポインタを取得することで、これらのコンテナからCコードに基になるデータを渡せます。いくつかの例を見ながら、このプロセスを説明していきましょう。

### std::array
`std::array`は、要素を連続メモリブロックに格納するサイズが固定されたコンテナです。`data()`メンバ関数を使用して、基になるデータへのポインタを取得します。

整数の配列を出力する次のC関数があるとします。

```c
// print_array.c
#include <stddef.h>
#include <stdio.h>

void print_array(const int *arr, size_t size) {
    for (size_t i = 0; i < size; ++i) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}
```

このC関数を使用して、C++コードから`std::array`のデータを出力します。

```cpp
// main.cpp
#include <array>
#include "print_array.h"

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    print_array(arr.data(), arr.size());

    return 0;
}
```

このコードは機能しますが、Cの`print_array`関数は避けるべきアンチパターンを使用します。このアンチパターンは何でしょうか。

```
```

---

ポインタとそのサイズでそれぞれ個別の引数を持つのはアンチパターンであり、回避しなければなりません。より適切な方法としては、C++側とC側の両方で [std::span](https://en.cppreference.com/w/cpp/container/span) (C++ 20)または同等のものを使用し、これら2つの間をマップしてポインタとサイズのgetterを提供する変換関数を使用するやり方があります。

# 演習

# 演習1
シンプルなLEDライトを制御する組み込みシステムを考えてみましょう。組み込みのCライブラリを使用すると、LEDのオン/オフを切り替えられます。このCライブラリを使用して、LEDライトを制御するC++クラスを作成します。

## 組み込みCライブラリ
最初に、LED制御をシミュレートする組み込みCライブラリを作成しましょう。

led_controller.c:

```c
#include "led_controller.h"

static bool led_state = false;

void led_init(void) {
    led_state = false;
}

void led_on(void) {
    led_state = true;
}

void led_off(void) {
    led_state = false;
}

bool led_get_state(void) {
    return led_state;
}
```

led_controller.h:

```c
#ifndef LED_CONTROLLER_H
#define LED_CONTROLLER_H

#include <stdbool.h>

void led_init(void);
void led_on(void);
void led_off(void);
bool led_get_state(void);

#endif // LED_CONTROLLER_H
```

上記のled_controllerコードから静的ライブラリを作成しましょう。

まず、-cフラグを使用して、led_controller.cファイルをコンパイルします。これでコンパイラにリンクを実行せずに`led_controller.c`をオブジェクトファイルにコンパイルするよう指示します。

```bash
gcc -c -Wall -Werror -Wextra led_controller.c
```

`led_controller.o`オブジェクトファイルが作成されました。

次に、`ar`コマンドを使用して静的ライブラリを作成し、ライブラリに含めたいオブジェクトファイルを渡します。

```bash
ar -rcs libledcontroller.a led_controller.o
```

後でC++プログラムにリンクできる`libledcontroller.a`静的ライブラリが作成されます。

C++プログラムを作成したら、最後のステップとして、C++プログラムをコンパイルし、静的ライブラリ`libledcontroller.a`をリンクします。

```bash
g++ -o main -L. -lledcontroller main.cpp led_controller.cpp
```

`-L`フラグは、ライブラリを含むパスを指定します。`-l`フラグは、先頭の「lib」と末尾の「.a」を省略して、ライブラリの名前を指定します。

## 演習2:  関数宣言のラップ
`extern "C"`を使用して、LEDコントローラーのCライブラリ関数宣言をラップします。

---

led_controller.h:

```c
#ifndef LED_CONTROLLER_H
#define LED_CONTROLLER_H

#include <stdbool.h>

#ifdef __cplusplus
extern "C" {
#endif

void led_init(void);
void led_on(void);
void led_off(void);
bool led_get_state(void);

#ifdef __cplusplus
}
#endif

#endif // LED_CONTROLLER_H
```

## 演習3: C++ LEDコントローラークラスの作成
Cライブラリを使用してLEDライトを制御するLedControllerというC++クラスを作成します。メンバ関数を実装して、初期化、オン/オフ、LEDの状態の取得を実行します。

---

led_controller.hpp:

```cpp
#pragma once

#include "led_controller.h"

class LedController {
public:
    LedController();
    void on();
    void off();
    bool getState();
};
```

led_controller.cpp:

```cpp
#include "led_controller.hpp"

LedController::LedController() {
    led_init();
}

void LedController::on() {
    led_on();
}

void LedController::off() {
    led_off();
}

bool LedController::getState() {
    return led_get_state();
}
```

## 演習4: プログラムでのC++ LEDコントローラークラスの使用
LedControllerクラスを使用して、LEDライトを制御するC++プログラムを作成します。LEDをオンにしてその状態を出力し、LEDをオフにしてその状態をもう一度出力します。

---

main.cpp:

```cpp
#include <iostream>
#include "led_controller.hpp"

int main() {
    LedController led;

    led.on();
    std::cout << "LED state: " << (led.getState() ? "ON" : "OFF") << std::endl;

    led.off();
    std::cout << "LED state: " << (led.getState() ? "ON" : "OFF") << std::endl;

    return 0;
}
```

## 演習5

次のCヘッダーファイルがあり、C++プログラムから`double_array`を呼び出したいとします。

```c
// double_array.h
#ifndef DOUBLE_ARRAY_H
#define DOUBLE_ARRAY_H

#include <stddef.h>

void double_array(int *arr, size_t size);

#endif // DOUBLE_ARRAY_H
```

`double_array`宣言をCヘッダーファイル内でラップします。

---

```c
// double_array.h
#ifndef DOUBLE_ARRAY_H
#define DOUBLE_ARRAY_H

#include <stddef.h>

#ifdef __cplusplus
extern "C" {
#endif

void double_array(int *arr, size_t size);

#ifdef __cplusplus
}
#endif

#endif // DOUBLE_ARRAY_H
```

## 演習6

`std::array`から基になるデータで`double_array`を呼び出して、C++プログラムから使用します。

---

```cpp
#include <array>
#include <iostream>
#include "double_array.h"

void print(const auto& array) {
    for (const auto& i : array) {
        std::cout << i << " ";
    }
    std::cout << '\n';
}

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    std::cout << "Original array: ";
    print(arr);

    double_array(arr.data(), arr.size());

    std::cout << "Modified array: ";
    print(arr);

    return 0;
}
```