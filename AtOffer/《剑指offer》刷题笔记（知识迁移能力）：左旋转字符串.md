# 《剑指offer》刷题笔记（知识迁移能力）：左旋转字符串

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

## 解题思路

和上一道题一样，我们根据n将字符串分为两部分，这两部分先自己反转，然后再一同反转。

## C++版代码实现

```
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int length = str.size();
        if(length < 0)
            return NULL;
        if(length >= 0 && n >= 0 && n <= length){
            int pFirstStart = 0;
            int pFirstEnd = n - 1;
            int pSecondStart = n;
            int pSecondEnd = length - 1;

            // 翻转字符串的前面n个字符
            reverseWord(str, pFirstStart, pFirstEnd);
            // 翻转字符串的后面部分
            reverseWord(str, pSecondStart, pSecondEnd);
            // 翻转整个字符串
            reverseWord(str, pFirstStart, pSecondEnd);
        }
        return str;
    }
    void reverseWord(string &str, int begin, int end){
        while(begin < end)
            swap(str[begin++], str[end--]);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**