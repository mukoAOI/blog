在 C++ 中，**lambda 表达式**是一种允许你定义匿名函数的功能，它从 C++11 标准开始引入，并在 C++14、C++17 和 C++20 中得到了增强和扩展。Lambda 表达式非常适合用于需要传递短小函数对象的场景，例如在 STL 算法中进行自定义排序、过滤、转换等操作。

### 基本语法

C++ 中的 lambda 表达式具有以下基本语法：

```cpp
[capture](parameters) -> return_type {
    // function body
}
```

- **capture**：捕获列表，指定如何捕获外部变量。
- **parameters**：参数列表，指定 lambda 表达式的参数。
- **return_type**：返回类型，可选，指定 lambda 的返回类型。
- **function body**：函数体，包含 lambda 表达式的具体实现。

### 捕获列表（Capture List）

捕获列表用于指定如何在 lambda 表达式中访问外部变量。捕获可以按值、按引用或不捕获。

- **按值捕获**（`[=]`）：捕获外部变量的副本。
- **按引用捕获**（`[&]`）：捕获外部变量的引用。
- **混合捕获**（`[=, &x]`）：捕获外部变量的副本和特定变量的引用。

### 示例

#### 简单的 Lambda 表达式

```cpp
#include <iostream>

int main() {
    // 无参数、无返回值的 lambda 表达式
    auto hello = []() {
        std::cout << "Hello, World!" << std::endl;
    };

    hello(); // 调用 lambda 表达式
    return 0;
}
```

#### 带参数的 Lambda 表达式

```cpp
#include <iostream>

int main() {
    // 带参数和返回值的 lambda 表达式
    auto add = [](int a, int b) -> int {
        return a + b;
    };

    std::cout << "Sum: " << add(5, 3) << std::endl;
    return 0;
}
```

#### 捕获外部变量

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;

    // 按值捕获
    auto sum = [x, y]() {
        return x + y;
    };

    std::cout << "Sum: " << sum() << std::endl;
    return 0;
}
```

#### 捕获引用

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;

    // 按引用捕获
    auto incrementX = [&x]() {
        x++;
    };

    incrementX();
    std::cout << "x after increment: " << x << std::endl;
    return 0;
}
```

#### 混合捕获

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;

    // 捕获 x 的引用和 y 的副本
    auto modify = [x, &y]() mutable {
        std::cout << "Captured x: " << x << std::endl;
        x += 5; // 修改 x 的副本
        y += 10; // 修改 y
    };

    modify();
    std::cout << "y after modify: " << y << std::endl; // y 被修改
    return 0;
}
```

### Lambda 表达式的特点

1. **内联定义**：Lambda 表达式允许你在调用时直接定义函数体，而无需单独定义一个函数。
2. **闭包**：Lambda 表达式可以捕获并记住外部作用域中的变量，即使它们在 lambda 被调用时已经超出了作用域。
3. **可变性**：使用 `mutable` 关键字可以在 lambda 内部修改捕获的变量副本。

### 总结

C++ 的 lambda 表达式是一个强大且灵活的工具，特别适合用作回调函数、STL 算法的自定义操作、并行编程等场景。它们提供了在函数对象的定义和使用中更大的简洁性和可读性。