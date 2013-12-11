---
layout : post
title : "外部模板"
category : "Language"
---

在标准C++中，只要在编译单元内遇到被完整定义的模板，编译器都必须将其实例化(instantiate)。这会大大增加编译时间，特别是模板在许多编译单元内使用相同的参数实例化。

原 C++ 强制编译器在特定位置开始实例化的语法：

    template class std::vector<MyClass>;

C++11 引入外部模板这一概念，将强制编译器在特定位置开始实例化，使用 extern 来显式组织编译器在某个编译单元内实例化。使用如下代码：

    extern template class std::vector<MyClass>;

这样告诉编译器不要在该编译单元内将该模板实例化(因为已经在别的单元实例化了)。


以上来自[外部模板](http://zh.wikipedia.org/wiki/C%2B%2B11#.E5.A4.96.E9.83.A8.E6.A8.A1.E6.9D.BF)，我对语言稍作组织。另，这个概念比较好理解，就不再举例说明了。

