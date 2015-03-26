---
layout : post
title : "新的函数声明语法"
category : "Language"
---

译自 [Alternative function syntax](https://en.wikipedia.org/wiki/C%2B%2B11#Alternative_function_syntax)


标准 C 函数声明语法对于 C 语言的特性来说是非常完美的。C++ 是从 C 语言扩展来而的，它保持了 C 的基本语言并且进行了一些必需的扩展。然后，随着 C++ 的语言复杂化，它有了很多的局限。特别是在模板函数的声明。下面的例子在 C++03 中是不允许的：

    template <class Lhs, class Rhs>
    Ret adding_func(const Lhs & lhs, const Rhs & rhs) { return lhs + rhs; } // Ret must be the type of lhs+rhs

因为 Ret 的类型是由 lhs 和 rhs 共同决定的(比如 lhs 是 char , rhs 是 int，返回的应该是个 int)，尽管 C++11 有了 `decltype` 可以进行类型推导，但是下面的使用也是不可以的：

    template <class Lhs, class Rhs>
    decltype(lsh+rhs) adding_func(const Lhs & lhs, const Rhs & rhs) { return lhs + rhs; } // Not legal C++11

不合法的原因是 lhs 和 rhs 在函数定义之前出现了，直到编译器解析到函数原型的后半部分， lhs 和 rhs 才是有意义的。

为了解决这个问题，C++11 引入了一种新的函数声明语法，即`尾部返回值(trailing-return-type)`:

    template <class Lhs, class Rhs>
    auto adding_func(const Lhs & lhs, const Rhs & rhs) -> decltype(lhs+rhs) { return lhs + rhs; }

这种语法可以应用到很多普通函数的声明和定义:

    struct SomeStruct  {
        auto func_name(int x, int y) -> int;
    };
     
    auto SomeStruct::func_name(int x, int y) -> int {
        return x + y;
    }

此种用法实际与lambda函数是很统一的，并不是很莫名其妙的语法。



## 扩展资料 ##

+ [Alternative function syntax](https://en.wikipedia.org/wiki/C%2B%2B11#Alternative_function_syntax)
+ [Improved Type Inference in C++11: auto, decltype, and the new function declaration syntax](http://www.cprogramming.com/c++11/c++11-auto-decltype-return-value-after-function.html)
