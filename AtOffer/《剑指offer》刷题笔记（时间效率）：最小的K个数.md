# 《剑指offer》刷题笔记（时间效率）：最小的K个数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

## 解题思路

思路一：结合快排。排序后直接for循环将最小的K个数填入结果。时间复杂度O（NlogK）。

思路二：最大堆或者红黑树。我们可以先创建一个大小为K的数据容器来存储最小的k个数字，然后每次从N个整数中读取一个数。如果容器中已有的数字个数少于K，则直接把这次读入的整数放入容器中；如果容器中已经有K个数了，此时我们需要找出容器中的最大值，然后与待插入的整数进行比较，谁小就留着谁。

我们再来捋一遍，容器满了之后我们要做的三件事情：一是在k个证书中找到最大数；二是有可能在这个容器中删除最大数；三是有可能要插入一个新的数字。如果用一个二叉树来实现这个数据容器，那么我们能在O(logK)时间内实现这三步操作。因此对于N个输入数字而言，总时间效率就是O(Nlogk)。

最大堆：根结点的值总是大于它的子树中任意结点的值。我们需要O(1)时间得到最大值，O(logK)时间来完成删除及插入操作；

红黑树：根据一定规则确保树在一定程度上是平衡的，从而保证在红黑树种查找、删除和插入操作都只需要O(logK)时间。

Python中没有现成的最大堆或者红黑树实现，本渣渣就不再这里折腾了。

## C++版代码实现

### 思路一

```c
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(input.empty() || k > input.size())
            return res;
        
        sort(input.begin(), input.end());
        
        for(int i = 0; i < k; ++i)
            res.push_back(input[i]);
        
        return res;
    }
};
```

### 最大堆

```c
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(len <= 0 || k > len || k <= 0)
            return vector<int>();
        vector<int> res(input.begin(), input.begin()+k);
        //建堆
        make_heap(res.begin(), res.end());
         
        for(int i=k; i < len; i++)
        {
            if(input[i] < res[0])
            {
                //先pop,然后在容器中删除
                pop_heap(res.begin(), res.end());
                res.pop_back();
                //先在容器中加入，再push
                res.push_back(input[i]);
                push_heap(res.begin(), res.end());
            }
        }
        //使其从小到大输出
        sort_heap(res.begin(), res.end());
        return res;
    }
};
```

### 红黑树

```c
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(len <= 0 || k > len || k <= 0)
            return vector<int>();
        //仿函数中的greater<T>模板，从大到小排序
        multiset<int, greater<int> > leastNums;
        vector<int>::iterator vec_it = input.begin();
        for(; vec_it != input.end(); vec_it++)
        {
            //将前k个元素插入集合
            if(leastNums.size() < k)
                leastNums.insert(*vec_it);
            else
            {
                //第一个元素是最大值
                multiset<int, greater<int> >::iterator greatest_it = leastNums.begin();
                //如果后续元素<第一个元素，删除第一个，加入当前元素
                if(*vec_it < *(leastNums.begin()))
                {
                    leastNums.erase(greatest_it);
                    leastNums.insert(*vec_it);
                }
            }
        }
        return vector<int>(leastNums.begin(), leastNums.end());
    }
};
```

## Python版代码实现

### 思路一

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        if tinput is None:
            return
        n = len(tinput)
        if n < k:
            return []
        tinput = sorted(tinput)
        return tinput[:k]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**