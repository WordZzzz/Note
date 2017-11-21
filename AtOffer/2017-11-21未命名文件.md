# 《剑指offer》刷题笔记（分解让复杂问题简单）：字符串的排列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

在计算机领域有一类算法叫分治法，即“分而治之”。采用的就是各个击破的思想，我们把分解后的小问题各个解决，然后把小问题的解决方案结合起来解决大问题。

## 题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

## 输入描述

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

## 解题思路

全排列，我们很快就想到递归。

首先，我们把一个字符串看成两部分组成：第一部分为它的第一个字符，第二部分为后面的所有字符。首先求所有可能出现在第一个位置的字符，即把第一个字符和后面所有的字符交换。第二步固定第一个字符，求后面的所有字符的排列。这个时候我们仍然把后面所有的字符分成两部分：后面字符的第一个字符，以及这个字符之后的所有字符，然后把第一个字符逐一和它后面的字符交换。

典型的递归思想，下面分别用C/C++和python实现，python还是那么的简单暴力。


## C++版代码实现

```c
class Solution {
public:
    vector<string> result;
    vector<string> Permutation(string str) {
        if(str.length() == 0)
            return result;
        Permutation1(str, 0);
        sort(result.begin(), result.end());
        return result;
    }
    
    void Permutation1(string str, int begin){
        if(begin == str.length()){
            result.push_back(str);
            return;
        }
        for(int i=begin; str[i] != '\0'; ++i){
            //如果有重复的，交换没什么卵用，所以直接跳过，其实这段话不加也行
            if(i != begin && str[begin] == str[i])
                continue;
            //交换
            swap(str[begin], str[i]);
            //递归
            Permutation1(str, begin+1);
            //复位
            swap(str[begin], str[i]);
        }
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        # write code here
        if not ss:
            return []
        return sorted(list(set(map(''.join, itertools.permutations(ss)))))
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**