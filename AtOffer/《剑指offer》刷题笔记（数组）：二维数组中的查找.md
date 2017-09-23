# 《剑指offer》刷题笔记（数组）：二维数组中的查找

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

## 题目描述

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 解题思路

首先选取数组中右上角的数字。如果该数字等于要查找的数字，查找过程结束；如果该数字大于要查找的数字，剔除这个数字所在的列；如果该数字小于要查找的数字，剔除这个数字所在的行。也就是说如果要查找的数字不在数组的右上角，则每一次都在数组的查找范围中剔除一行或者一列，这样每一步都可以缩小查找的范围，直到找到要查找的数字，或者查找范围为空。

当然，大家也可以制定自己的规则，建议从左下角或者右上角开始，因为如果直接随机选取数组中的一个数开始进行比较，剩下的区域会出现重合的现象，即所谓的岔路，不好操作。比如我用的右上角，那就一直用右上角的进行判断，进行比较后剔除一行或者一列。算法复杂度为n。

还有人直接对每一行用二分法进行查找，算法复杂度为nlogn。

## C++代码实现

### 右上角

```c
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row = array.size();
        int col = array[0].size();
        
        if(!array.empty() && row > 0 && col > 0) {
            int i = 0;
            int j = col - 1;
            while(i < row && j >= 0) {
                if(array[i][j] == target)
                    return true;
                else if(array[i][j] > target)
                    --j;
                else
                    ++i;
            }
        }
        return false;
    }
};
```

### 二分遍历

```c
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        for(int i=0; i < array.size(); i++){
            int low=0;
            int high=array[i].size()-1;
            while(low <= high){
                int mid = (low + high) / 2;
                if(target > array[i][mid])
                    low = mid + 1;
                else if(target < array[i][mid])
                    high = mid - 1;
                else
                    return true;
            }
        }
        return false;
    }
};
```

## Python代码实现

### 直接遍历

```python
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        n=len(array)
        flag='false'
        for i in range(n):
            if target in array[i]:
                flag='true';
                break
        return flag
while True:
    try:
        S=Solution()
        # 字符串转为list
        L=list(eval(raw_input()))
        array=L[1]
        target=L[0]
        print(S.Find(target, array))
    except:
        break
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**