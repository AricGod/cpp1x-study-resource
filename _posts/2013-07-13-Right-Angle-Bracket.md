---
layout : post
title : "右尖括号"
category : "Document"
---

译自：[Right angle bracket](https://en.wikipedia.org/wiki/C%2B%2B11#Right_angle_bracket)

C++03 的析取符定义为">>"(注，在我的第一本C++书中，">>"被称为析取运算符，而"<<"被称为插入运算符，我感觉那样理解挺不错的)。 然而，在模板的嵌套声明中，程序员需要在两个尖括号之间加上一个空格，否则会引发一个编译错误。

C++11 提供了新的解析规则，多个右尖括号的时候，会优先为模板解析。但是有括号的时候，会先解析括号：

    template<bool Test> class SomeType;
    std::vector<SomeType<1>2>> x1; // Interpreted as a std::vector of SomeType<true> 2>,
    // which is not legal syntax. 1 is true.
    std::vector<SomeType<(1>2)>> x1; // Interpreted as a std::vector of SomeType<false>,
    // which is legal C++11 syntax, (1>2) is false.

## 扩展资料 ##

+ [Right angle bracket](https://en.wikipedia.org/wiki/C%2B%2B11#Right_angle_bracket)
