---
layout : post
title : "控制和查询对象布局"
category : "Document"
---

+ 内容为个人理解，不保证正确。


C++11 可以显示的控制和查询内存布局方式。

通过使用 `alignof` 可以得到对象的内存布局字节基数，比如：

    struct char_align {
        char c1;
        char c2;
    };

`alignof(char_align)` 得到的值为 `1`.

    struct short_int_align {
        short int si1;
        short int si2;
    };

`alignof(short_int_align)` 得到的值 `2` .

    struct test_align {
        int i;
        char c;
        float f;
        double d;
        long int li;
    };

`alignof(short_int_align)` 得到的值是 `4`.

如果之前了解过 C/C++ 字节对齐的朋友应该一点就明白了，我这里不解释了，不明白的话，建议去看一下字节对齐这个技术。

`alignas` 与之相反，可以显式的指定字节对其方式，指定的参数必须是 2 的倍数。eg:

    struct alignas(8) test_alignas {
        int i;
        char c;
        float f;
        double d;
        long int li;
    };

这么做指定了 test_alignas 按照 8 字节对齐。再使用 `alignof(test_alignas)` 得到的值就是 8 .

## 扩展资料 ##

+ [What does static_assert do, and what would you use it for?](http://stackoverflow.com/questions/1647895/what-does-static-assert-do-and-what-would-you-use-it-for)
+ [Control and query object alignment](https://en.wikipedia.org/wiki/C%2B%2B11#Control_and_query_object_alignment)
