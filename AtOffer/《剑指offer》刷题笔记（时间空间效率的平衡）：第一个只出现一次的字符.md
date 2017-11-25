# 《剑指offer》刷题笔记（时间空间效率的平衡）：第一个只出现一次的字符

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

## 解题思路

我们首先想到的可能就是遍历数组，对每一个字符串再遍历判断是否唯一，这样的时间复杂度为O(n^2)。这显然不是我们想要的。

然后我们可能会想到，为什么不用hash表呢？首先遍历数组建立hash表，key为字符串，value为该字符串出现的次数。然后再遍历hash，找到里面value为1的key就可以了。

C++中，我们可以用数组实现，此时，数组长度设定为256，对应字符串的ASCII码；也可以用map实现。
Python中，我们可以用字典实现，但是很耗空间。

## C++版代码实现

### 数组

```c
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        if(str.size() == 0)
            return -1;
        char ch[256] = {0};
        for(int i=0; i < str.size(); ++i)
            ch[str[i]]++;
        for(int i=0; i < str.size(); ++i){
            if(ch[str[i]] == 1)
                return i;
        }
        return 0;
    }
};
```

### map

```c
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        if(str.size() == 0)
            return -1;
        map<char, char> mp;
        for(int i=0; i < str.size(); ++i)
            mp[str[i]]++;
        for(int i=0; i < str.size(); ++i){
            if(mp[str[i]] == 1)
                return i;
        }
        return 0;
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def FirstNotRepeatingChar(self, s):
        # write code here
        if len(s) == 0:
            return -1
        cur = {}
        for i in range(len(s)):
            if s[i] not in cur.keys():
                cur[s[i]] = 1
            else:
                cur[s[i]] += 1
        for i in range(len(s)):
            if cur[s[i]] == 1:
                return i
        return 0
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**