# 《剑指offer》刷题笔记（知识迁移能力）：数组中只出现一次的数字

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

## 解题思路

我刚看到这道题的时候，一点思路都没有，看了别人的解法之后才恍然大悟。

这道题我们可以利用异或的特性--两个相同数字异或，结果为0。把数组中的数据全部想成二进制会更容易理解。

首先我们来考虑数组中只有一个出现一次的数字的情况，这种情况下，我们直接异或数组中的所有值即可得到这个数字；接下来考虑有两个出现一次的数字的情况，这种情况下，我们可以把原来的数组分成两组，每组里面各有一个只出现一次的数字，然后运用前面的解法即可得到结果。

问题的关键是如何划分成两个数组。我们呢可以根据全部值异或的结果进行思考，我们可以先把结果中最右边的1作为划分条件，然后判断数组中每个数字该位置上是不是1，如果是则为第一组，否则为第二组。这样分完之后，一组里面正好一个只出现一次的数字。

## C++版代码实现

```
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int length = data.size();
        if(length < 2)
            return;
        //异或操作
        int resultExclusiveOR = 0;
        for(int i = 0; i < length; ++i)
            resultExclusiveOR ^= data[i];
        //获取倒数第一个1的位置
        unsigned int indexOf1 = findFirstBitIs1(resultExclusiveOR);
        //分组异或
        *num1 = *num2 = 0;
        for(int j=0; j < length; ++j){
            if(isBit1(data[j], indexOf1))
                *num1 ^= data[j];
            else
                *num2 ^= data[j];
        }
    }
    
    unsigned int findFirstBitIs1(int num){
        int indexBit = 0;
        while((num & 1) == 0 && indexBit < 8 * sizeof(int)){
            num = num>>1;
            ++ indexBit;
        }
        return indexBit;
    }
    
    bool isBit1(int num, unsigned int indexBit){
        num = num >> indexBit;
        return (num & 1);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**