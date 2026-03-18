>  声明： 本文档由 Deepseek v 3.2 生成

#cpp 
在 C++ 中，拼接两个 `std::string` 有多种方法：

## 1. **使用 `+` 运算符**（最常用）
```cpp
#include <string>
#include <iostream>

int main() {
    std::string str1 = "Hello, ";
    std::string str2 = "World!";
  
    std::string result = str1 + str2;
    std::cout << result << std::endl;  // 输出: Hello, World!
  
    // 也可以直接拼接
    std::string combined = "Hello, " + std::string("World!");
    return 0;
}
```

## 2. **使用 `append()` 方法**
```cpp
#include <string>
#include <iostream>

int main() {
    std::string str1 = "Hello, ";
    std::string str2 = "World!";
  
    str1.append(str2);
    std::cout << str1 << std::endl;  // 输出: Hello, World!
    return 0;
}
```

## 3. **使用 `+=` 运算符**
```cpp
#include <string>
#include <iostream>

int main() {
    std::string str = "Hello, ";
    std::string str2 = "World!";
  
    str += str2;
    std::cout << str << std::endl;  // 输出: Hello, World!
  
    // 也可以直接拼接字符串字面量
    str += " Welcome!";
    std::cout << str << std::endl;  // 输出: Hello, World! Welcome!
    return 0;
}
```

## 4. **使用 C++11 的用户定义字面量**
```cpp
#include <string>
#include <iostream>

int main() {
    using namespace std::string_literals;
  
    auto str = "Hello, "s + "World!"s;
    std::cout << str << std::endl;  // 输出: Hello, World!
    return 0;
}
```

## 5. **使用 `std::stringstream` **（适合多个字符串拼接）
```cpp
#include <string>
#include <iostream>
#include <sstream>

int main() {
    std::stringstream ss;
    ss << "Hello, " << "World" << "!";
  
    std::string result = ss.str();
    std::cout << result << std::endl;  // 输出: Hello, World!
    return 0;
}
```

## 6. **性能考虑**
- 对于少量拼接，使用 `+` 或 `+=` 即可
- 对于大量拼接（如循环中），推荐：
  ```cpp
  std::string result;
  result.reserve(total_length);  // 预分配内存，避免多次重新分配
  result += str1;
  result += str2;
  // ... 更多拼接
  ```

## 注意：
- 避免拼接 C 风格字符串时的问题：
  ```cpp
  // 错误：不能直接拼接两个字符串字面量
  // std::string str = "Hello, " + "World!";

  // 正确：至少一个是 std::string 类型
  std::string str = std::string("Hello, ") + "World!";
  ```

最常用的方法是 `+` 运算符和 `+=` 运算符，它们直观且易于阅读。