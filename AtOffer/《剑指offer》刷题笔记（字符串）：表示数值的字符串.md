# 《剑指offer》刷题笔记（字符串）：表示数值的字符串

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

## 解题思路

其实这道题也是正则表达式的匹配过程。判断一个字符串是否表示数值的正则表达式为：[\+\-]\?[0-9]\*(\.[0-9]\*)\?([eE][\+\-]?[0-9]\*)\?。更加详细的注释都在程序里了，这里就不再赘述。

## C++版代码实现

```c
class Solution {
public:
    // 数字的格式可以用A[.[B]][e|EC]或者.B[e|EC]表示，其中A和C都是
    // 整数（可以有正负号，也可以没有），而B是一个无符号整数
    bool isNumeric(const char* str)
    {
        if(str == nullptr)
            return false;

        bool numeric = scanInteger(&str);

        // 如果出现'.'，接下来是数字的小数部分
        if(*str == '.')
        {
            ++str;

            // 下面一行代码用||的原因：
            // 1. 小数可以没有整数部分，例如.123等于0.123；
            // 2. 小数点后面可以没有数字，例如233.等于233.0；
            // 3. 当然小数点前面和后面可以有数字，例如233.666
            numeric = scanUnsignedInteger(&str) || numeric;
        }

        // 如果出现'e'或者'E'，接下来跟着的是数字的指数部分
        if(*str == 'e' || *str == 'E')
        {
            ++str;

            // 下面一行代码用&&的原因：
            // 1. 当e或E前面没有数字时，整个字符串不能表示数字，例如.e1、e1；
            // 2. 当e或E后面没有整数时，整个字符串不能表示数字，例如12e、12e+5.4
            numeric = numeric && scanInteger(&str);
        }

        return numeric && *str == '\0';
    }

    bool scanUnsignedInteger(const char** str)
    {
        const char* before = *str;
        while(**str != '\0' && **str >= '0' && **str <= '9')
            ++(*str);

        // 当str中存在若干0-9的数字时，返回true
        return *str > before;
    }

    // 整数的格式可以用[+|-]B表示, 其中B为无符号整数
    bool scanInteger(const char** str)
    {
        if(**str == '+' || **str == '-')
            ++(*str);
        return scanUnsignedInteger(str);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**