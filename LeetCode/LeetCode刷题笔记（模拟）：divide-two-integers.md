---
title: LeetCode刷题笔记（模拟）：divide-two-integers
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Simulation
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

Divide two integers without using multiplication, division and mod operator.

不用乘法，除法和取模运算符来计算两个整数相除的结果。

## 解题思路

假设我们要划分15和3，所以15是dividend、3是divisor。那么划分只需要我们找出可以从dividend中减去多少次divisor，而不会造成dividend负面影响。

首先是15-3=12，然后让我们尝试去减去更多。3左移一位得6，15-6=9。6左移一位是12，15-12=3。当我们再次对12进行左移得到24时，已经比15大了，所以15最多可以减去12。12是3的四倍，那么我们怎么得到这个4呢？其实我们可以在3（cur）左移的时候，对1（multiple）进行同样的左移操作即可，这样，当cur成为12的时候，multiple正好是4。

接下来，把15-12剩下的3再次进行循环，得出multiple为1，所以最后得到4+1=5次。

根据问题陈述，我们需要处理一些例外，例如溢出。

那么有两种情况可能会导致溢出：

- divisor = 0;
- dividend = INT_MIN和divisor = -1（因为abs(INT_MIN) = INT_MAX + 1）。

当然，我们也需要考虑相对容易的标志。

## C++版代码实现

```c
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(!divisor || dividend == INT_MIN && divisor == -1)
            return INT_MAX;
        long long dvd = labs(dividend);
        long long dvs = labs(divisor);
        int res = 0;
        while(dvd >= dvs){
            long long cur = dvs, multiple = 1;
            while(dvd >= cur << 1){
                cur <<= 1;
                multiple <<= 1;
            }
            dvd -= cur;
            res += multiple;
        }
        return ((dividend < 0) ^ (divisor < 0)) ? -res : res;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**