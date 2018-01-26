---
title: LeetCode刷题笔记（贪心）：gas-station
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Greedy
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

There are N gas stations along a circular route, where the amount of gas at station i isgas[i].

You have a car with an unlimited gas tank and it costscost[i]of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

Note: 
The solution is guaranteed to be unique.

沿着一个圆形的路线有N个加油站，那里的加油站的天然气数量是gas [i]。 你有一辆带有无限制燃气罐的汽车，从车站i到下一站（i + 1）需要花费天然气。 你从一个加油站开始，开始时邮箱为空。 如果您可以在电路上巡回一次，则返回启动加油站的索引，否则返回-1。 注意：解决方案保证是唯一的。

## 解题思路

从start出发，如果油量足够，可以一直向后走end++；油量不够的时候，
start向后退，最终start == end的时候，如果有解一定是当前start所在位置。

因为是个环，所以我们将最后一个元素设置为起点，第一个元素设置为start下一点，这样可以用统一的判断条件start < end来作为循环的条件，当start和end两者相等时，循环结束。

## C++版代码实现

```c
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        if(gas.size() <= 0 || cost.size() <= 0)
            return -1;
        
        int start = gas.size() - 1;
        int end = 0;
        int sum = gas[start] - cost[start];
        while(start > end){
            if(sum >= 0){
                sum += gas[end] - cost[end];
                ++end;
            }
            else{
                --start;
                sum += gas[start] - cost[start];
            }
        }
        return sum >= 0 ? start : -1;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**