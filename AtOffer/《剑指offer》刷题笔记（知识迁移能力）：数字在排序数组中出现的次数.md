# 《剑指offer》刷题笔记（知识迁移能力）：数字在排序数组中出现的次数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

统计一个数字在排序数组中出现的次数。

## 解题思路

数组是排序的，那就没什么好说的了，直接二分查找。

假设我们给定的数字是2，二分查找可以帮我们找到一个2。然后我们左右两边顺序扫描，找到第一个2和最后一个2。因为要查找的数字在长度为n的数组中有可能出现O(n)次，所以顺序扫描的时间复杂度为O(n)。因此这种算法的效率和从头到尾直接顺序扫描整个数组统计出2的个数是一样的。

我们在这里采用更高效的方法。还是二分法找到一个2，然后我们开始判断，这个2是不是第一个2（或者最后一个2）：如果是，那么就返回序列号，如果不是，就继续二分查找。由于二分法查找第一个2和最后一个2在此处的判断条件不一样，所以得分开写。

我们不要拘泥于迭代，也可以试着写写循环嘛。

## C++版代码实现

```c
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int length = data.size();
        
        if(length == 0)
            return 0;
        
        int firstK = getFirstK(data, 0, length-1, k);
        int lastK = getLastK(data, 0, length-1, k);
        
        if(firstK != -1 && lastK != -1)
            return lastK-firstK+1;
        
        return 0;
    }
    //迭代实现
    int getFirstK(vector<int> data, int begin, int end, int k){
        if(begin > end)
            return -1;
        int middleIndex = (end+begin)>>1;
        int middleData = data[middleIndex];
        
        if(middleData == k){
            if((middleIndex > 0 && data[middleIndex-1] != k) || middleIndex == 0)
                return middleIndex;
            else
                end = middleIndex - 1;
        }
        else if(middleData > k)
            end = middleIndex - 1;
        else
            begin = middleIndex + 1;
        
        return getFirstK(data, begin, end, k);
    }
    //循环实现
    int getLastK(vector<int> data, int begin, int end, int k){
        int length = data.size();
        int middleIndex = (end+begin)>>1;
        int middleData = data[middleIndex];
        
        while(begin <= end){
            if(middleData == k){
                if((middleIndex < length-1 && data[middleIndex+1] != k) || middleIndex == length-1)
                    return middleIndex;
                else
                    begin = middleIndex + 1;
            }
            else if(middleData > k)
                end = middleIndex - 1;
            else
                begin = middleIndex + 1;
            middleIndex = (end+begin)>>1;
            middleData = data[middleIndex];
        }
        return -1;
    }
};
```

## Python版代码实现


```python
# -*- coding:utf-8 -*-
class Solution:
    def GetNumberOfK(self, data, k):
        # write code here
        return data.count(k)
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**