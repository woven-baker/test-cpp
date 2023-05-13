# 課題

## 課題1
以下の各プログラム言語の機能において、組み込みコンテキストでの使用が推奨される理由と推奨されない理由を1つ以上記入してください。必要であれば調べてみてください。

- [enumクラス](https://en.cppreference.com/w/cpp/language/enum)(C++11)
- クラス
- 継承
- テンプレート
- 関数のオーバーロードとデフォルトパラメータ
- 演算子のオーバーロード
- 参照
- 名前空間
- 仮想メソッド
- [`std::unique_ptr`](https://en.cppreference.com/w/cpp/memory/unique_ptr) (C++11)
- [`using`](https://en.cppreference.com/w/cpp/language/type_alias) (C++11)
- [`auto`](https://en.cppreference.com/w/cpp/language/auto) (C++11, C++17)
- [`std::array`](https://en.cppreference.com/w/cpp/container/array) (C++11)
- [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view) (C++17)
- [`constexpr`](https://en.cppreference.com/w/cpp/language/constexpr) (C++11)
- [`consteval`](https://en.cppreference.com/w/cpp/language/consteval) (C++20)
- [`if constexpr`](https://en.cppreference.com/w/cpp/language/if) (C++17)
- [`if consteval`](https://en.cppreference.com/w/cpp/language/if) (C++23)
- [`noexcept指定子`](https://en.cppreference.com/w/cpp/language/noexcept_spec)(C++11)
- [`noexcept演算子`](https://en.cppreference.com/w/cpp/language/noexcept)(C++11)
- [`static_assert`](https://en.cppreference.com/w/cpp/language/static_assert)(C++11、C++17)
- [例外](https://en.cppreference.com/w/cpp/language/exceptions)
- [関数オブジェクト](https://en.cppreference.com/w/cpp/utility/functional)
- [Unit型](https://en.cppreference.com/w/cpp/language/type_alias)、例[std::chrono::duration](https://en.cppreference.com/w/cpp/chrono/duration)(C++11)
- [`new`](https://en.cppreference.com/w/cpp/language/new)/[`delete`](https://en.cppreference.com/w/cpp/language/delete)
- [`std::shared_ptr`](https://en.cppreference.com/w/cpp/memory/shared_ptr) (C++11)
- [RTTI](https://en.cppreference.com/w/cpp/language/typeid)
- [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string)
- [`std::vector`](https://en.cppreference.com/w/cpp/container/vector)
- [`std::function`](https://en.cppreference.com/w/cpp/utility/functional/function)

---

- 列挙型クラス: 普通の列挙型に比べて型安全とスコープが優れているため、バグが発生しにくくなり、コードが読みやすくなります。

- クラス: データと動作をカプセル化し、モジュール化、コードの再利用、保守がしやすくなります。

- 継承: コードの再利用と抽象化が可能になり、複雑なシステムを単純化できます。

- テンプレート: コンパイル時のポリモーフィズムとコード再利用が可能になり、コードの重複と実行時のオーバーヘッドを削減します。

- 関数のオーバーロードとデフォルトパラメータ: 引数の型に基づいて関数を複数実装したり、引数のデフォルト値を提供したりするので、コードの読みやすさと柔軟性が向上します。

- 演算子のオーバーロード: ユーザー定義型の演算子をカスタム実装できるため、コードの読みやすさと一貫性が向上します。

- 参照: ポインタよりも安全で予測可能な動作を提供し、ヌルポインタの逆参照やその他のメモリ関連のバグのリスクを軽減します。

- 名前空間: 名前の重複を防ぎ、コードが整理されるので、モジュール化と保守性が向上します。

- 仮想メソッド: ポリモーフィズムが使用できますが、vtableのルックアップが原因で実行時のオーバーヘッドが発生し、コードサイズが大きくなる可能性があります。テンプレートを使用した静的ポリモーフィズムなどの代替案を検討してください。

- `std:: unique_ptr`: メモリの割り当てと割り当て解除を自動的に管理し、メモリリークのリスクを軽減し、コードをより堅牢にします。

- `using`: 型エイリアシングが使用でき、コードが簡略化され、読みやすさが向上します。

- `auto`: 自動型推論が使用できるので、コードがさらに簡潔になり、エラーが発生しにくくなります。

- `std:: array`: スタックに割り当てられた固定サイズのコンテナを提供し、メモリ使用量とパフォーマンスが予測できるようになります。

- `std::string_view`: 推奨されます。文字列の所有権を保持せず軽量に表示できるため、パフォーマンスが向上し、メモリ使用量が削減されます。
  
- `constexpr`: コンパイル時の定数式を評価できるので、実行時のオーバーヘッドを削減し、パフォーマンスを向上させます。

- `consteval`: コンパイル時の関数評価を強制実行するので、実行時のオーバーヘッドがなくなります。

- `if constexpr`: コンパイル時条件によって分岐できるので、パフォーマンスの向上とコードサイズの削減が可能になります。

- `if consteval`: ifconstexprと同様に、consteval関数を使用してコンパイル時条件によって分岐できます。

- `noexcept指定子`: 推奨されます。例外がないことを知らせるので、パフォーマンスが向上します。

- `noexcept演算子`: 推奨されます。式が例外をスローできるかどうかをコンパイル時にチェックできるため、パフォーマンスが向上します。

- `static_assert`: 推奨されます。エラーを早期に検出してコードの安全性を向上できる、コンパイル時アサーションが使用できます。

- 例外: プログラム言語によっては必須です。noexceptと組み合わせて使用すると、`std::terminate`を使ってプログラムを終了させるのに便利です。

- 関数オブジェクト: 注意して使用してください。柔軟性が高くなりますが、オーバーヘッドが発生してコードサイズが大きくなる可能性があります。

- Unit型(例:std:: chrono:: duration): 推奨されます。強い型付けと安全性を提供し、コードが正確になり、読みやすくなります。

- `new`/`delete`: メモリリークや断片化を引き起こす可能性があり、メモリに制約のある環境では問題になります。スマートポインタまたはスタックベースの割り当てを使用することをお勧めします。

- `std::shared_ptr`: `std::shared_ptr`を使用すると、アドホックのガベージコレクションがプログラムに追加されます。オブジェクトはスコープにバインドされず、実質的にはグローバルオブジェクトです。デストラクタは最初のコンストラクションと逆の順序では実行されません。破棄の順序は常に決まっている訳ではなく、処理中のデータが変化すると(たとえば、センサーの読み取り値が異なる場合など)、オブジェクトを破棄する順序が変わる場合があります。デストラクタを実行したときのアプリケーションの状態が不明な場合、最後の参照がロック中、割り込みルーチン、任意のスレッドで破棄される可能性があります。

- RTTI (Run-Time Type Information): コードサイズが大きくなり、実行時のオーバーヘッドが発生するため、リソースに制約のあるシステムでは問題になる可能性があります。

- `std::string`: 動的メモリ割り当てを使用するため、メモリの断片化が発生する可能性があります。固定サイズの文字配列、または組み込みシステム用に最適化されたカスタム文字列型の使用を検討してください。

- `std::vector`: は動的メモリ割り当てを使用するため、メモリの断片化が発生する可能性があります。std::arrayのような固定サイズのコンテナや、組み込みシステム用に最適化されたカスタムコンテナをお勧めします。

- `std::function`: 推奨されません。オーバーヘッドが発生してコードサイズが大きくなるため、組み込みシステムにはあまり適していません。できれば、関数ポインタやその他の軽量な代替手段の使用をお勧めします。