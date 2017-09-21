# 《剑指offer》刷题笔记（递归和循环）：矩形覆盖

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：

我们可以用2 * 1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2 * 1的小矩形无重叠地覆盖一个2 * n的大矩形，总共有多少种方法？

## 解题思路：

和跳台阶的解题思路是一样的。

对于2 * n的矩形，第一步有2种覆盖方法：横着放一个2 * 1的矩形、竖着放两个2 * 1的矩形；
横着放一个2 * 1的矩形，剩下2 * n-1的矩形，则剩下覆盖方法是f(n-1)
竖着放两个2 * 1的矩形，剩下2 * n-2的矩形，则剩下覆盖方法是f(n-2)
所以f(n)=f(n-1)+f(n-2)。
其中：f(1) = 1, f(2) = 2。

实现f(n)=f(n-1)+f(n-2)的方法有很多种，递归、循环都可以。

注意：由于递归比较耗费时间，加上python的运行效率本来就低，所以python的递归调用一般在牛客网的实例测试中下总是超时。

## C++版代码实现：

### 递归：

```c
class Solution {
public:
    int rectCover(int number) {
        if(number <= 0)
            return 0;
        else if(number < 3)
            return number;
        else
            return rectCover(number-1)+rectCover(number-2);
    }
};
```

### 循环：

```c
class Solution {
public:
    int rectCover(int number) {
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
    def rectCover(self, number):
        # write code here
        if number <= 0:
            return 0
        elif number < 3:
            return number
        else:
            return self.rectCover(number-1) + self.rectCover(number-2)
```

### 循环：

```python
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
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