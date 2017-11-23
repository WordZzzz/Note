# 《剑指offer》刷题笔记（时间效率）：连续子数组的最大和

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？(子向量的长度至少是1)

## 解题思路

和leetcode里面的股票题目很像。看到这个题目，如果我们枚举出数组的所有子数组并求出他们的和。一个长度为n的数组，总共有n(n+1)/2个子数组。计算出所有的子数组的和，最快也需要O(n^2)的时间。运用数组分析或者动态规划则可以实现时间复杂度为O(n)，但是二者在本题的实现上是一样的。

数组分析：下图是我们计算数组（1，-2，3，10，-4，7，2，-5）中子数组的最大和的过程。通过分析我们发现，累加的子数组和，如果大于零，那么我们继续累加就行；否则，则需要剔除原来的累加和重新开始。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171123112556089?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

动态规划：如果用函数f(i)表示第i个数字结尾的子数组的最大和，那么我们需要求出man[f(i)]，其中0<=i<=n。我们可以用如下递归公式求f(i)：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171123112614392?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

当以第i-个数字结尾的子数组中的所有数字的和小于0时，如果把这个负数与第i个数累加，得到的结果比第i个数字本身还要小，所以这种情况下第i个数字结尾的字数子就是第i个数字本身。否则，累加。

## C++版代码实现

```c
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        if(array.empty())
            return 0;
        //初始值最好不要设置为0，防止全是负数的情况出现
        int nCurSum = array[0];
        int nGreatestSum = array[0];
        //从1开始遍历
        for(int i=1; i < array.size(); ++i){
            if(nCurSum <= 0)
                nCurSum = array[i];
            else
                nCurSum += array[i];
            if(nCurSum > nGreatestSum)
                nGreatestSum = nCurSum;
        }
        return nGreatestSum;
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def FindGreatestSumOfSubArray(self, array):
        # write code here
        if len(array) == 0:
            return 0
        #初始值最好不要设置为0，防止全是负数的情况出现
        nCurSum = nGreatestSum = array[0]
        #从1开始遍历
        for i in range(1,len(array)):
            if nCurSum <= 0:
                nCurSum = array[i]
            else:
                nCurSum += array[i]
                
            if nCurSum > nGreatestSum:
                nGreatestSum = nCurSum
        return nGreatestSum
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**