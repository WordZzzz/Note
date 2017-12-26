# 《剑指offer》刷题笔记（树）：数据流中的中位数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

## 解题思路

解法很多，直接列出表格吧。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171226114037598?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

我们这里采用最优的最大最小堆来实现。下面来讨论一些细节问题。

首先要保证数据平均分配到两个堆中，因此两个堆中数据的数目之差不能超过1.为了实现平均分配，可以在数据的总数目是偶数时把新数据插入到最小堆中，否则插入到最大堆中。

此外，还要保证最大堆中所有数据小于最小堆中数据。所以，新传入的数据需要先和最大堆的最大值或者最小堆中的最小值进行比较。以总数目为偶数为例，按照我们制定的规则，新的数据会被插入到最小堆中，但是在这之前，我们需要判断这个数据和最大堆中的最大值谁更大，如果最大堆中的数据比较大，那么我们就需要把当前数据插入最大堆，然后弹出新的最大值，再插入到最小堆中。由于最终插入到最小堆的数字是原最大堆中最大的数字，这样就保证了最小堆中所有数字都大于最大堆的数字。

下面代码中，我们基于stl中的函数push_heap、pop_heap以及vector实现堆。比较仿函数less和greater分别用来实现最大堆和最小堆。

## C++版代码实现

```c
class Solution {
public:
    void Insert(int num)
    {
        if(((min.size() + max.size()) & 1) == 0){
            if(max.size() > 0 && num < max[0]){
                max.push_back(num);
                push_heap(max.begin(), max.end(), less<int>());
                num = max[0];
                pop_heap(max.begin(), max.end(), less<int>());
                max.pop_back();
            }
            min.push_back(num);
            push_heap(min.begin(), min.end(), greater<int>());
        }
        else{
            if(min.size() > 0 && num > min[0]){
                min.push_back(num);
                push_heap(min.begin(), min.end(), greater<int>());
                num = min[0];
                pop_heap(min.begin(), min.end(), greater<int>());
                min.pop_back();
            }
            max.push_back(num);
            push_heap(max.begin(), max.end(), less<int>());
        }
    }

    double GetMedian()
    { 
        int size = max.size() + min.size();
        if(size == 0)
            return 0;
        if((size & 1) == 1)
            return min[0];
        else
            return (min[0] + max[0]) / 2.0;
    }
private:
    vector<int> min;
    vector<int> max;
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**