---
title: LeetCode刷题笔记（模拟）：pascals-triangle
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Simulation
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

给定numRows，生成帕斯卡三角形的前numRows行。

## 解题思路

题目比较古老，几行代码就能搞定，注意一下代码中的边界条件。

我感觉像下面这样写会更直观一些，每一行两头的1我们都直接添加即可，从第三行开始，假定用res[][]表示下面各个值的位置，那么第三行的2所在的位置就是res[2][1]。res[2][1] = res[1][1] + res[1][0]，推广之后为res[i][j] = res[i][j] + res[i][j-1]，这就是中间这些数的计算公式，还是要注意边界问题。

```
1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
```

## C++版代码实现

```c
class Solution {
public:
    vector<vector<int> > generate(int numRows) {
        vector<vector<int>> res(numRows);
        for(int i = 0; i < numRows; ++i){
        	res[i].push_back(1);                                //第一个位置为1
            for(int j = 1; j < i; ++j)
                res[i].push_back(res[i-1][j-1] + res[i-1][j]);  //j从1开始，那么只有i>=2的时候才会执行这部分
            if(i > 0)
                res[i].push_back(1);                            //最后一个位置也为1，需要注意是不能再第一行中添加
        }
        return res;
    }
};
```


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**