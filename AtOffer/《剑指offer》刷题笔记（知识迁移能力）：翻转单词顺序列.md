# 《剑指offer》刷题笔记（知识迁移能力）：翻转单词顺序列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

## 解题思路

思路很简单，先将每个单词反转，再将所有字符串一起反转。我们这里写个函数封装一下反转的过程，同时用追加空格字符的方式来减少判断条件。

## C++版代码实现

```
class Solution {
public:
    string ReverseSentence(string str) {
        auto length = str.size();
        if(length == 0)
            return "";
        str += ' ';
        int mark = 0;
        for(int i = 0; i < length + 1; ++i){
            if(str[i] == ' '){
                reverseWord(str, mark, i-1);
                mark = i + 1;
            }
        }
        str = str.substr(0, length);
        reverseWord(str, 0, length-1);
        
        return str;
    }
    void reverseWord(string &str, int begin, int end){
        while(end > begin)
            swap(str[begin++],str[end--]);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**