# 《剑指offer》刷题笔记（递归和循环）：变态跳台阶

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/nowcoder/blob/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## 解题思路：

因为n级台阶，第一步有n种跳法：跳1级、跳2级、到跳n级
跳1级，剩下n-1级，则剩下跳法是f(n-1)
跳2级，剩下n-2级，则剩下跳法是f(n-2)
所以f(n)=f(n-1)+f(n-2)+...+f(1)
因为f(n-1)=f(n-2)+f(n-3)+...+f(1)
所以f(n)=2*f(n-1)

实现f(n)=2*f(n-1)的方法有很多种，递归、循环都可以。

注意：由于递归比较耗费时间，加上python的运行效率本来就低，所以python的递归调用一般在牛客网的实例测试中下总是超时。

## C++版代码实现：

### 递归：

```c
class Solution {
public:
    int jumpFloorII(int number) {
		if(number <= 0){
            return -1;
        }else if(number == 1){
            return 1;
        }else{
            return 2 * jumpFloorII(number -1);
        }
    }
};
```

### 循环：

```c
class Solution {
public:
    int jumpFloorII(int number) {
        if(number == 0)
            return number;
        int total=1;
        for(int i=1; i<number; i++)
            total *= 2;
        return total;
    }
};
```

### 移位：

```c
class Solution {
public:
    int jumpFloorII(int number) {
        return 1<<--number;
    }
};
```

## Python 代码实现：

### 递归：

```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        if number <= -1:
            return -1
        elif number == 1:
            return 1
        else:
            return 2*self.jumpFloorII(number-1)
```

### 循环：

```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        if number == 0:
            return number
        total = 1
        for i in range(number-1):
            total *= 2
        return total
```

### 移位：

```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        return 1<<(number-1);
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**