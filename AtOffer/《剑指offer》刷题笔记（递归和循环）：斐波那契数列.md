# 《剑指offer》刷题笔记（递归和循环）：斐波那契数列

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

如果我们需要重复地多次计算相同的问题，通常可以选择用递归或者循环两种不同的方法。递归式在一个函数的内部调用这个函数自身。而循环则是通过设置计算的初始值及终止条件，在一个范围内重复运算。

通常递归的代码会比较简洁。在面试的时候，如果面试官没有特别的要求，应聘者可以尽量多采用递归。

递归虽然有简介的优点，但它同时也有显著的缺点，那就是时间和空间的消耗：每一次函数调用，都需要在内存栈中分配空间以保存参数、返回地址及临时变量，而且往栈里面压入数据和弹出数据都需要时间，这就不难理解上述的例子中递归实现的效率不如循环了。

除了效率，递归还有可能引起更严重的问题：调用栈溢出。当递归调用的层数太多时，就会超出栈的容量，从而导致调用栈溢出。

## 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。
n<=39

## 解题思路

斐波那契数列的定义如下：

`!$f(n) = \begin{cases} 0, & \text{n = 0} \\ 1, & \text{n = 1} \\ {f(n-1)+f(n-2)}, & \text{n > 1} \end{cases} $`

实现f(n)=f(n-1)+f(n-2)的方法有很多种，递归、循环都可以。

注意：由于递归比较耗费时间，加上python的运行效率本来就低，所以python的递归调用一般在牛客网的实例测试中下总是超时。这次C++的递归调用也超时了，所以啊，面试手写优先使用递归，在线编程优先使用循环呐。

## C++版代码实现：

### 递归：

```c
class Solution {
public:
    int Fibonacci(int n) {
        if(n <= 0)
            return 0;
        if(n == 1)
            return 1;
        return Fibonacci(n-1)+Fibonacci(n-2);
    }
};
```

### 循环：

```c
class Solution {
public:
    int Fibonacci(int n) {
        if(n <= 0)
            return 0;
        if(n == 1)
            return 1;
        int first = 0, second = 1, third = 0;
        for (int i = 2; i <= n; i++) {
            third = first + second;
            first = second;
            second = third;
        }
        return third;
    }
};
```

## Python 代码实现：

### 递归：

```python
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n <= 0:
            return 0
        if n == 1:
            return 1
        return self.Fibonacci(n-1) + self.Fibonacci(n-2)
```

### 循环：

```python
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n <= 0:
            return 0
        if n == 1:
            return 1
        first = 0
        second = 1
        third = 0
        for i in range(2,n+1):
            third = first + second
            first = second
            second = third
        return third
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**