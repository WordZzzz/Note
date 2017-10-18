# 《剑指offer》刷题笔记（画图让抽象形象化）：顺时针打印矩阵

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

## 解题思路

这个题完全没有涉及复杂的数据结构或者高级的算法，看起来是一个很简单的问题。但实际上解决这个问题，会在代码中包含多个循环，并且还需要判断多个边界条件。

顺时针打印就是按圈数循环打印，一圈包含两行两列（完整圆），在打印的时候会出现某一圈中只包含一行，要判断从左向右打印和从右向左打印的时候是否会出现重复打印，同样只包含一列时，要判断从上向下打印和从下向上打印的时候是否会出现重复打印的情况。

首先是while循环的终止条件：left <= right && top <= bottom。相信这个条件大家很容易就能理解。

接下来是后两个for循环的终止条件，因为到了最后一个圆的时候，这个圆可能不是一个完整的圆了，所以需要加入判断，以防止打印重复的行或者列。

## C++版代码实现

```c
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        int row = matrix.size();//行数
        int col = matrix[0].size();//列数
        vector<int> result;
        
        if(row == 0 && col == 0)
            return result;
        int left = 0, right = col - 1, top = 0, bottom = row - 1;
        while(left <= right && top <= bottom){
            //left>>right
            for(int i = left; i <= right; ++i)
                result.push_back(matrix[top][i]);
            //top>>bottom
            for(int i = top+1; i <= bottom; ++i)
                result.push_back(matrix[i][right]);
            //right>>left
            if(top != bottom)
                for(int i = right-1; i >= left; --i)
                    result.push_back(matrix[bottom][i]);
            //bottom>>top
            if(left != right)
                for(int i = bottom-1; i > top; --i)
                    result.push_back(matrix[i][left]);
            left++,top++,right--,bottom--;
        }
        return result;
    }
};
```

## Python版代码实现
### 递归
```python
# -*- coding:utf-8 -*-
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        row = len(matrix)
        col = len(matrix[0])
        result = []
        
        if row == 0 and col == 0:
            return result
        
        left = 0; right = col-1; top = 0; bottom = row-1;
        while left <= right and top <= bottom:
            for i in xrange(left, right+1):
                result.append(matrix[top][i])
            for i in xrange(top+1, bottom+1):
                result.append(matrix[i][right])
            if top is not bottom:
                for i in xrange(right-1, left-1, -1):
                    result.append(matrix[bottom][i])
            if left is not right:
                for i in xrange(bottom-1, top, -1):
                    result.append(matrix[i][left])
            left += 1;top += 1;right -= 1;bottom -= 1;
        return result
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**