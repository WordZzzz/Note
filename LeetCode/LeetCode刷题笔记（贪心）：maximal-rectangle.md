---
title: LeetCode刷题笔记（贪心）：maximal-rectangle
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Greedy
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

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing all ones and return its area.

给定一个用0和1填充的2D二进制矩阵，找到包含所有1的最大矩形并返回其面积。

## 解题思路

刚看到这道题的时候，一脸无奈。我们还是先来看一下Largest Rectangle in Histogram这道题目吧。

### Largest Rectangle in Histogram

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126101534175?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

The largest rectangle is shown in the shaded area, which has area = 10 unit.

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126101634504?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

For example,
Given heights = [2,1,5,6,2,3],
return 10.

我们先把代码贴出来，然后边看代码边捋清思路。

```c
class Solution {
public:
    int largestRectangleArea(vector<int>& height) {
        stack<int> s;
        height.push_back(0);//追加一个零用来做终止条件。
        int maxSize = 0;
        int i = 0; 
        while(i < height.size()){
            if(s.empty() || height[i] >= height[s.top()])
                s.push(i++);
            else{
                int cur = height[s.top()];
                s.pop();
                maxSize = max(maxSize, cur * (s.empty() ? i : i - 1 - s.top()));
            }
        }
        return maxSize;
    }
};
```

- 首先，我们需要新建stack来保存遍历过程中的递增序列。如果栈是空的，那么索引i入栈。所以当i=0时，将0入栈。注意栈内保存的是索引，不是高度。然后i++。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126110920004?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 当i=1的时候，发现height[i]小于了栈内的元素，于是出栈。这时候stack为空，所以面积的计算是height[t] * i。t是刚刚弹出的stack顶元素。也就是蓝色部分的面积。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111432408?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 这时候stack为空了，那就继续入栈。注意到只要是连续递增的序列，我们都要keep pushing，直到我们遇到了i=4，height[i]=2小于了栈顶的元素。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111452015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 这时候开始计算矩形面积。首先弹出栈顶元素，t=3。即下图绿色部分。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111528463?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 接下来注意到栈顶的（索引指向的）元素还是大于当前i指向的元素，于是出栈，并继续计算面积，桃红色部分。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111658101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 最后，栈顶的（索引指向的）元素大于了当前i指向的元素，循环继续，入栈并推动i前进。直到我们再次遇到下降的元素，也就是我们最后人为添加的dummy元素0.

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111715452?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

- 同理，我们计算栈内的面积。由于当前i是最小元素，所以所有的栈内元素都要被弹出并参与面积计算。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20180126111731018?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

注意我们在计算面积的时候已经更新过了maxSize 。

我们可以看到，stack中总是保持递增的元素的索引，然后当遇到较小的元素后，依次出栈并计算栈中bar能围成的面积，直到栈中元素小于当前元素。

### maximal-rectangle

这道题的解法灵感来自于Largest Rectangle in Histogram这道题，假设我们把矩阵沿着某一行切下来，然后把切的行作为底面，将自底面往上的矩阵看成一个直方图（histogram）。直方图的中每个项的高度就是从底面行开始往上1的数量。根据Largest Rectangle in Histogram中的largestRectangleArea函数我们就可以求出当前行作为矩阵下边缘的一个最大矩阵。接下来如果对每一行都做一次Largest Rectangle in Histogram，从其中选出最大的矩阵，那么它就是整个矩阵中面积最大的子矩阵。算法时间复杂度为O(m*n)，这解法真是太棒了，其实应该算是动态规划的题目了。

## C++版代码实现

```c
class Solution {
public:
    
class Solution {
public:
    int largestRectangleArea(vector<int>& height) {
        stack<int> s;
        height.push_back(0);//追加一个零用来做终止条件。
        int maxSize = 0;
        int i = 0; 
        while(i < height.size()){
            if(s.empty() || height[i] >= height[s.top()])
                s.push(i++);
            else{
                int cur = height[s.top()];
                s.pop();
                maxSize = max(maxSize, cur * (s.empty() ? i : i - 1 - s.top()));
            }
        }
        return maxSize;
    }
};
    
    int maximalRectangle(vector<vector<char> > &matrix) {
        if(matrix.empty())
            return 0;
        int maxRec = 0;
        vector<int> height(matrix[0].size(), 0);
        for(int i = 0; i < matrix.size(); ++i){
            for(int j = 0; j < matrix[0].size(); ++j){
                if(matrix[i][j] == '1')//这里的数字1一定要加单引号哦~
                    height[j]++;
                else
                    height[j] = 0;
            }
            maxRec = max(maxRec, largestRectangleArea(height));//对每一行都求一次最大值，而不是只有最后一行。
        }
        return maxRec;
    }
};
```

[参考链接](http://www.cnblogs.com/lichen782/p/leetcode_Largest_Rectangle_in_Histogram.html)

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**