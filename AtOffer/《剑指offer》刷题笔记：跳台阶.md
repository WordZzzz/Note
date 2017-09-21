# 《剑指offer》刷题笔记：跳台阶

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## 解题思路：

对于n级台阶，第一步有2种跳法：跳1级、跳2级；
跳1级，剩下n-1级，则剩下跳法是f(n-1)
跳2级，剩下n-2级，则剩下跳法是f(n-2)
所以f(n)=f(n-1)+f(n-2)。

实现f(n)=f(n-1)+f(n-2)的方法有很多种，递归、循环都可以。

## C++版代码实现：

### 递归：

```c
class Solution {
public:
    int jumpFloor(int number) {
        if(number <= 0)
            return 0;
        else if(number < 3)
            return number;
        else
            return jumpFloor(number-1)+jumpFloor(number-2);
    }
};
```

### 循环：

```c
class Solution {
public:
    int jumpFloor(int number) {
        if(number <= 0)
            return 0;
        else if(number < 3)
            return number;
        int first = 1, second = 2, third = 0;
        for (int i = 3; i <= number; i++) {
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
    def jumpFloor(self, number):
        # write code here
        if number <= 0:
            return 0
        elif number < 3:
            return number
        else:
            return self.jumpFloor(number-1) + self.jumpFloor(number-2)
```

### 循环：

```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloor(self, number):
        # write code here
        if number <= 0:
            return 0
        elif number < 3:
            return number
        first = 1
        second = 2
        third = 0
        for i in range(3,number+1):
            third = first + second
            first = second
            second = third
        return third
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**