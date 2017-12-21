# 《剑指offer》刷题笔记（字符串）：字符流中第一个不重复的字符

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

## 输出描述

如果当前字符流没有存在出现一次的字符，返回#字符。

## 解题思路

好吧，我是第一次见这种需要写部分测试函数的题目。

题目很简单，用hash表来实现，其实hash表的键值为输入字符的ASCII码，hash的值初始化为-1，出现一次则设置为该字符在字符串中的位置，出现两次以上则设置为-2。

最后搜索的时候，根据hash表的值就可以找到第一个不重复的字符。

- occurrence[i] = -1: 这个字符不存在；
- occurrence[i] = -2: 这个字符出现了多次；
- occurrence[i] >= 0: 这个字符只出现一次。

需要注意的是，因为我们要求如果当前字符流没有存在出现一次的字符，返回#字符，所以我们初始化字符时应该设置为'#'而不是'\0'。

## C++版代码实现

```c
class Solution
{
public:
    Solution()
    {
        memset(occurrence, -1, sizeof(occurrence));
    }
    //Insert one char from stringstream
    void Insert(char ch)
    {
        if(occurrence[ch] == -1)
            occurrence[ch] = index;
        else if(occurrence[ch] >= 0)
            occurrence[ch] = -2;
        index++;
    }
    //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        char ch = '#';
        int minIndex = numeric_limits<int>::max();
        for(int i = 0; i < 256; ++i){
            if(occurrence[i] >= 0 && occurrence[i] <= minIndex){
                ch = (char)i;
                minIndex = occurrence[i];
            }
        }
        return ch;
    }
private:
    int occurrence[256];
    int index;
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**