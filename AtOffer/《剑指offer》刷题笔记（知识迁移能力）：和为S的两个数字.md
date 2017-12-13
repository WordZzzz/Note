# 《剑指offer》刷题笔记（知识迁移能力）：和为S的两个数字

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

## 输出描述

对应每个测试案例，输出两个数，小的先输出。

## 解题思路

暴力的时间复杂度为O(n^2)。

下面我们想一下时间复杂度为O(n)的算法。我们可以定义两个指针，一个从前往后遍历（ahead），另一个从后往前遍历（behind）。首先，我们比较第一个数字和最后一个数字的和curSum与给定数字sum，如果curSum < sum，那么我们就要加大输入值，所以，ahead向后移动一位，重复之前的计算；如果curSum > sum，那么我们就要减小输入值，所以，behind向前移动一位，重复之前的计算；如果相等，那么这两个数字就是我们要找的数字，直接输出即可。

## C++版代码实现

```
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> result;
        int length = array.size();
        if(length < 1)
            return result;
        int ahead = length-1;
        int behind = 0;
        int curSum;
        while(ahead > behind){
            curSum = array[ahead] + array[behind];
            if(curSum == sum){
                result.push_back(array[behind]);
                result.push_back(array[ahead]);
                break;
            }
            else if(curSum < sum)
                ++behind;
            else
                --ahead;
        }
        return result;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**