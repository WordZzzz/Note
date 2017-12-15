# 《剑指offer》刷题笔记（抽象建模能力）：扑克牌顺子

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何。为了方便起见,你可以认为大小王是0。

## 解题思路

一次for循环就可以搞定，满足如下条件才可以认为是顺子：

- 输入数据个数为5；
- 输入数据都在0-13之间；
- 没有相同的数字；
- 最大值与最小值的差值不大于5；

不用对输入数据做排序，直接设置一个flag，该flag的每一位分别用来记录1-13是否出现，出现过一次置1，出现两次则中止程序。

## C++版代码实现

```
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        if(numbers.size() < 5)
            return false;
        int max = -1;
        int min = 14;
        int flag = 0;
        for(int i = 0; i < numbers.size(); ++i){
            int number = numbers[i];
            if(number < 0 || number > 13)
                return false;
            if(number == 0)
                continue;
            if(((flag >> number) & 1) == 1)
                return false;
            flag |= 1 << number;
            if(number < min)
                min = number;
            if(number > max)
                max = number;
            if(max - min >= 5)
                return false;
        }
        return true;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**