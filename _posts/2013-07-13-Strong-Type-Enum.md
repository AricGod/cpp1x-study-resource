---
layout : post
title : "强类型枚举"
category : "Document"
---

译自： [Better types in C++11 - nullptr, enum classes (strongly typed enumerations) and cstdint](http://www.cprogramming.com/c++11/c++11-nullptr-strongly-typed-enum-class.html);

C++03 的枚举本质上就是整型，它们可以和整型，和其它的枚举类型做比较。实际上，你并不希望不同枚举之间进行比较，就好像你不希望把钉子的种类和牙刷的种类做比较一样。

旧式枚举的另外一个限制是枚举的值是没有作用域的，换句话说，你不能两个枚举共享同一个名字，比如：

    enum Color {RED, GREEN, BLUE};
    enum Feeling {EXCITED, MOODY, BLUE}; // error: redeclaration of ‘BLUE’

C++11 提供了强类型枚举——枚举类(enum classes)，如下方式声明：

`class`意味着每一个枚举类型实际上都是不一样的，不能和其它类型做比较。强类型枚举，枚举类有很好的作用域。每个枚举值作用域被限制在枚举类中。这意味着，想要存取枚举值，你必须要这么用：

    Color color = Color::GREEN;
    if (Color::RED == color) {
        // the color is red;
    }

如果你想用旧式的枚举，仍旧是可用的。很大程度上，是可以和既有的代码相兼容的。教你一招，现在你可以直接在值前面加上枚举名：Color::RED.但是这种情况下，不能解决命名冲突的问题，它只是更清晰了一些。

枚举类比旧式枚举类型另外一个优势是，你可以使用前向声明一个强枚举类型，意味着你可以这样写代码：

    enum class Mood;
    void assessMode (Mood m);
    enum class {EXCITED, MOODY, BLUE};

Why would this be useful? Forward declarations are often about the physical layout of code on disk into different files or to provide opaque objects as part of an API. In the first case, where you care about the physical disk layout, using a forward declaration allows you to declare an enum type in the header file while putting specific values into the cpp file. This lets you change the list of possible enum values quite frequently without forcing all dependent files to recompile. In the second case, an enum class can be exposed as a type-safe but otherwise opaque value returned from one API function to be passed into another API function. The code using the API need not know the possible values the type can take on. Since the compiler still knows about the type, it can enforce that variables declared to work with that type are not confused with variables working with another type. (译者注：这段话不太理解，所有没有翻译，欢迎补充翻译。)

枚举类的最后一个好处是你可以你的枚举的大小，你可以使用任何签名的或者未签名的整型类。默认是 int ，但是你也可以使用 char, unsigned long 等等。这样可以确保跨平台编译器的兼容性。

    enum class Colors : char { RED = 1, GREEN = 2, BLUE = 3 };

但是在在 C++11 中，在指定类型大小上，我们可以用更好的方式，使用 `cstdint`。

C++ 在类型方面存在一个问题，比如说，你想要一个 32位 的整型，但是 int 可能在不同体系架构下有不同的大小。在 C++11 中，C99 的头文件 `stdint.h` 包含在 `cstdint` 中。`cstdint` 头文件包含了诸如 `std::int8_t`, `std::int32_t` 和 `int64_t`(对应的 unsigned 版本是从 `std::uint8_t` 开始的)的类型。

下面是使用新类型和枚举类完美配合，告诉跨平台编译器和体系架构你的枚举大小：

enum class Colors: std::int8_t { RED = 1, GREEN = 2, BLUE = 3};

## 扩展资料 ##

+ [Better types in C++11 - nullptr, enum classes (strongly typed enumerations) and cstdint](http://www.cprogramming.com/c++11/c++11-nullptr-strongly-typed-enum-class.html)
+ [Strongly typed enumerations](https://en.wikipedia.org/wiki/C%2B%2B11#Strongly_typed_enumerations)
