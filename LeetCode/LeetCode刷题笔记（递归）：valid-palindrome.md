---
title: LeetCode刷题笔记（递归）：valid-palindrome
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Recursive
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

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama"is a palindrome.
"race a car"is not a palindrome.

Note: 
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

## 解题思路

额，判断字符串是否为回文，这道题貌似没什么好说的，需要注意的是空字符串也算回文哦。

从两边开始遍历字符串，如果遇到非数字字符的就跳过，然后比较当前两个字符是否相等；相等就进入下一个循环，否则返回false。

## C++版代码实现

### 循环

```c
class Solution {
public:
    bool isPalindrome(string s) {
        if(s.size() == 0)
            return true;
        for(int i = 0, j = s.size() - 1; i < j; ++i, --j){
            while(i < j && !isalnum(s[i]))
                i++;
            while(i < j && !isalnum(s[j]))
                j--;
            if(i < j && tolower(s[i]) != tolower(s[j]))
                return false;
        }
        return true;
    }
};
```

### 递归

```c
class Solution {
public:
    
    bool isPalindromeCore(string &s, int start, int end){
        if(start >= end)
            return true;
        while(start < end && !isalnum(s[start]))
            start++;
        while(start < end && !isalnum(s[end]))
            end--;
        if(start < end && tolower(s[start]) != tolower(s[end]))
            return false;
        return isPalindromeCore(s, ++start, --end);
    }
    bool isPalindrome(string s) {
        if(s.size() == 0)
            return true;
        return isPalindromeCore(s, 0, s.size());
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**