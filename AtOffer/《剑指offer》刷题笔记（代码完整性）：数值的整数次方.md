# 《剑指offer》刷题笔记（代码完整性）：数值的整数次方

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

代码的规范性：书写清晰，布局清晰，命名合理。
代码的完整性：功能测试，边界测试，负面测试。

## 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

## 解题思路

方法一：公式求解

我们知道当指数为负数的时候，可以先对指针求绝对值，然后算出次方的结果之后再取倒数。如果要自己实现，那么就需要考虑各种错误处理和边界问题。比如，既然有求倒数，对0求倒数怎么办，当底数是0且指数是负数的时候，如果不做特殊处理，就会出现对0求倒数而导致程序运行出错。

一个细节值得我们注意：在判断底数是不是等于0时，不能直接写base == 0，这是因为在计算机内表示小数时都有误差。判断两个小数是否相等，只能判断他们之差的绝对值是不是在一个很小的范围内。如果两个数差值很小，就可以认为它们相等。这就是我们定义函数equal的原因。

方法二：迭代

`!$a^n = \begin{cases} a^{n/2}*a^{n/2}, & \text{n为偶数} \\ a^{(n-1)/2}*a^{(n-1)/2}*a, & \text{n为奇数} \end{cases} $`

这个公式很容易就用递归来实现。

## C++版代码实现：

### 调用pow：

```c
class Solution {
public:
    double Power(double base, int exponent) {
        double result;
        if(exponent >= 0){
            result = pow(base, exponent);
        }
        else{
            result = pow(base, -exponent);
            result = 1 / result;
        }
        return result;
    }
};
```

### 不调用pow：

```
class Solution {
public:
    bool g_InvalidInput = false;
    
    double Power(double base, int exponent) {
        g_InvalidInput = false;
        
        if(equal(base, 0.0) && exponent < 0) {
            g_InvalidInput = true;
            return 0.0;
        }
        
        unsigned int absExponent = (unsigned int)(exponent);
        if(exponent < 0)
            absExponent = (unsigned int)(-exponent);
        
        double result = PowerWithUnsignedExponent(base, absExponent);
        if(exponent < 0)
            result = 1.0 / result;
        return result;
    }
    
    double PowerWithUnsignedExponent(double base, unsigned int exponent){
        double result = 1.0;
        for (int i = 1; i <= exponent; ++i)
            result *= base;
        return result;
    }
    
    bool equal(double num1, double num2){
        if((num1 - num2 > -0.0000001) && (num1 - num2 < 0.0000001))
            return true;
        else
            return false;
    }
};
```

### 递归：

```c
class Solution {
public:
    bool g_InvalidInput = false;
    
    double Power(double base, int exponent) {
        g_InvalidInput = false;
        
        if(equal(base, 0.0) && exponent < 0) {
            g_InvalidInput = true;
            return 0.0;
        }
        
        unsigned int absExponent = (unsigned int)(exponent);
        if(exponent < 0)
            absExponent = (unsigned int)(-exponent);
        
        double result = PowerWithUnsignedExponent(base, absExponent);
        if(exponent < 0)
            result = 1.0 / result;
        return result;
    }
    
    double PowerWithUnsignedExponent(double base, unsigned int exponent){
        if(exponent == 0)
            return 1;
        if(exponent == 1)
            return base;
        
        double result = PowerWithUnsignedExponent(base, exponent >> 1);
        result *= result;
        if(exponent & 0x1 == 1)
            result *= base;
        return result;
    }
    
    bool equal(double num1, double num2){
        if((num1 - num2 > -0.0000001) && (num1 - num2 < 0.0000001))
            return true;
        else
            return false;
    }
};
```

## Python 代码实现：

### 调用pow：

```python
# -*- coding:utf-8 -*-
class Solution:
    def Power(self, base, exponent):
        # write code here
        if exponent >= 0:
            result = pow(base, exponent)
        
        else:
            result = pow(base, -exponent)
            result = 1 / result
        
        return result
```

### 不调用pow

```
# -*- coding:utf-8 -*-
class Solution:
    def Power(self, base, exponent):
        # write code here
        flag = 0
        if base == 0:
            return False
        if exponent == 0:
            return 1
        if exponent < 0:
            flag = 1
        result = 1
        for i in range(abs(exponent)):
            result *= base
        if flag == 1:
            result = 1 / result
        return result
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**