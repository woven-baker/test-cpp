# Challenges

## Challenge 1
For each of the following language features, provide one or more reasons why they are recommended or not recommended for use in an embedded context. Research if you need to.

- [Enum classes](https://en.cppreference.com/w/cpp/language/enum) (C++11)
- Classes
- Inheritance
- Templates
- Function overloading and default parameters
- Operator overloading
- References
- Namespaces
- Virtual methods
- [`std::unique_ptr`](https://en.cppreference.com/w/cpp/memory/unique_ptr) (C++11)
- [`using`](https://en.cppreference.com/w/cpp/language/type_alias) (C++11)
- [`auto`](https://en.cppreference.com/w/cpp/language/auto) (C++11, C++17)
- [`std::array`](https://en.cppreference.com/w/cpp/container/array) (C++11)
- [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view) (C++17)
- [`constexpr`](https://en.cppreference.com/w/cpp/language/constexpr) (C++11)
- [`consteval`](https://en.cppreference.com/w/cpp/language/consteval) (C++20)
- [`if constexpr`](https://en.cppreference.com/w/cpp/language/if) (C++17)
- [`if consteval`](https://en.cppreference.com/w/cpp/language/if) (C++23)
- [`noexcept specifier`](https://en.cppreference.com/w/cpp/language/noexcept_spec) (C++11)
- [`noexcept operator`](https://en.cppreference.com/w/cpp/language/noexcept) (C++11)
- [`static_assert`](https://en.cppreference.com/w/cpp/language/static_assert) (C++11, C++17)
- [Exceptions](https://en.cppreference.com/w/cpp/language/exceptions)
- [Function Objects](https://en.cppreference.com/w/cpp/utility/functional)
- [Unit Types](https://en.cppreference.com/w/cpp/language/type_alias), e.g. [std::chrono::duration](https://en.cppreference.com/w/cpp/chrono/duration) (C++11)
- [`new`](https://en.cppreference.com/w/cpp/language/new)/[`delete`](https://en.cppreference.com/w/cpp/language/delete)
- [`std::shared_ptr`](https://en.cppreference.com/w/cpp/memory/shared_ptr) (C++11)
- [RTTI](https://en.cppreference.com/w/cpp/language/typeid)
- [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string)
- [`std::vector`](https://en.cppreference.com/w/cpp/container/vector)
- [`std::function`](https://en.cppreference.com/w/cpp/utility/functional/function)

---

- Enum classes: Provide better type safety and scoping compared to plain enums, which helps prevent bugs and improves code readability.

- Classes: Encapsulate data and behavior, promoting modularity, code reuse, and maintainability.

- Inheritance: Enables code reuse and abstraction, simplifying complex systems.

- Templates: Enable compile-time polymorphism and code reuse, reducing code duplication and runtime overhead.

- Function overloading and default parameters: Improves code readability and flexibility by allowing multiple implementations of a function based on argument types or providing default values for arguments.

- Operator overloading: Allows custom implementations of operators for user-defined types, improving code readability and consistency.

- References: Provide safer and more predictable behavior compared to pointers, reducing the risk of null pointer dereferences and other memory-related bugs.

- Namespaces: Prevent naming conflicts and organize code, improving modularity and maintainability.

- Virtual methods: Enable polymorphism but may introduce runtime overhead due to vtable lookups and can increase code size. Consider alternatives such as static polymorphism with templates.

- `std::unique_ptr`: Automatically manage memory allocation and deallocation, reducing the risk of memory leaks and making code more robust.

- `using`: Allows for type aliasing and simplifies code, improving readability.

- `auto`: Enables automatic type deduction, making code more concise and less error-prone.

- `std::array`: Provides a fixed-size, stack-allocated container, ensuring predictable memory usage and performance.

- `std::string_view`: Recommended. It provides a lightweight, non-owning view of a string, which can improve performance and reduce memory usage.
  
- `constexpr`: Enables compile-time constant expression evaluation, reducing runtime overhead and improving performance.

- `consteval`: Enforces compile-time evaluation of a function, ensuring no runtime overhead.

- `if constexpr`: Enables compile-time branching, which can improve performance and reduce code size.

- `if consteval`: Like if constexpr, it enables compile-time branching with consteval functions.

- `noexcept specifier`: Recommended. It helps communicate the absence of exceptions and can improve performance.

- `noexcept operator`: Recommended. It allows checking at compile time whether an expression can throw an exception, which can improve performance.

- `static_assert`: Recommended. It enables compile-time assertions that can catch errors early and improve code safety.

- Exceptions: Required by the language. Useful in conjunction with noexcept in order to terminate programs using `std::terminate`.

- Function Objects: Use with caution. They can provide a great deal of flexibility, but also may introduce overhead and increase code size.

- Unit Types (e.g. std::chrono::duration): Recommended. They provide strong typing and safety, improving code correctness and readability.

- `new`/`delete`: Can lead to memory leaks and fragmentation, which is problematic in memory-constrained environments. Prefer using smart pointers or stack-based allocation.

- `std::shared_ptr`: Using `std::shared_ptr` adds ad-hoc Garbage Collection to the program. Objects are not bound to a scope and are effectively global objects. Destructors are not run in the inverse order from their initial construction. Destruction order is no longer deterministic, a change in the data being processed (e.g. different sensor readings at different times) can reorder object destruction. The state of the application when a destructor is run is unknown, the last ref might be destructed under a lock, on an interrupt routine, in an arbitrary thread.

- RTTI (Run-Time Type Information): Increases code size and introduces runtime overhead, which can be problematic in resource-constrained systems.

- `std::string`: Uses dynamic memory allocation and can cause memory fragmentation. Consider using fixed-size character arrays or custom string types optimized for embedded systems.

- `std::vector`: Uses dynamic memory allocation and can cause memory fragmentation. Prefer fixed-size containers like std::array or custom containers optimized for embedded systems.

- `std::function`: Not recommended. It can introduce overhead and increase code size, making it less suitable for embedded systems. Prefer function pointers or other lightweight alternatives when possible.