# 《剑指offer》刷题笔记（字符串）：正则表达式匹配

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配。

## 解题思路

只有当模式串和字符串同时等于'\0'，才可以认为两个串匹配。

在匹配中，对于每个位的匹配可以分为三种情况:

- 1、（相应位匹配||模式串为'.'&&字符串不是'\0'）&&模式串下一位是'*'
- 2、（相应位匹配||模式串为'.'&&字符串不是'\0'）&&模式串下一位不是'*'
- 3、相应位不匹配&&（模式位不为'.'||字符串是'\0'）

对应1，最复杂。分为\*取0，\*取1，\*>=2三种情况。
\*取0对应跳过当前匹配位，继续寻找patter的下一个匹配位，str不变，pattern+2
\*取1对应当前匹配位算一次成功匹配，str+1，pattern+2
\*取>=2对应一次成功匹配，继续匹配字符串的下一位是否匹配，str+1，pattern不变
三者取或。即只要有一种情况能匹配成功认为字符串就是匹配成功的。
对应2，相当于一次成功匹配，str+1，pattern+1
对应3，匹配失败，直接返回false

## C++版代码实现

```c
class Solution {
public:
    bool match(const char* str, const char* pattern)
    {
        if(str == nullptr || pattern == nullptr)
            return false;
    
        return matchCore(str, pattern);
    }
    
    bool matchCore(const char* str, const char* pattern)
    {
        if(*str == '\0' && *pattern == '\0')
            return true;
    
        if(*str != '\0' && *pattern == '\0')
            return false;
    
        if(*(pattern + 1) == '*')
        {
            if(*pattern == *str || (*pattern == '.' && *str != '\0'))
                // 进入有限状态机的下一个状态
                return matchCore(str + 1, pattern + 2)
                // 继续留在有限状态机的当前状态 
                || matchCore(str + 1, pattern)
                // 略过一个'*' 
                || matchCore(str, pattern + 2);
            else
                // 略过一个'*'
                return matchCore(str, pattern + 2);
        }
    
        if(*str == *pattern || (*pattern == '.' && *str != '\0'))
            return matchCore(str + 1, pattern + 1);
    
        return false;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**