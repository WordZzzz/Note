# 《剑指offer》刷题笔记（时间空间效率的平衡）：丑数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

## 解题思路

同样的，我们首先想到的可能就是遍历判断，但是每个数都要计算一次是不是ugly，很是麻烦。于是我们想，能不能只对ugly进行计算呢，显然是可以的。

我们用一个数组来从小到大存放ugly，那现在的问题就是排序问题了，我们如何来保障这里面的数组是排好序的呢？

我们把当前数组里面最大的ugly记为M，那么，下一个ugly肯定是前面的数与2或者3或者5的乘积中的最小值。然后我们拿这个最小值和M进行比较，如果小于M，就说明已经存在于数组中，如果大于M，则说明需要添加进数组。需要注意的是，我们每次判断之后，我们只需要第一个比M大的值，其他的以后会重新计算。程序中通过t2、t3、t5分别记录上次计算用到的ugly的相应序号。

## C++版代码实现

```c
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index <= 0)
            return index;
        vector<int> res(index);
        res[0] = 1;
        int t2=0, t3=0, t5=0;
        for(int i=1; i < index; ++i){
            res[i] = min(res[t2] * 2, min(res[t3] * 3, res[t5] * 5));
            if(res[i] == res[t2] * 2)
                t2++;
            if(res[i] == res[t3] * 3)
                t3++;
            if(res[i] == res[t5] * 5)
                t5++;
        }
        return res[index - 1];
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetUglyNumber_Solution(self, index):
        # write code here
        if index <= 0:
            return index;
        res = [1]
        t2 = t3 = t5 = 0
        for i in range(1,index):
            res.append(min(res[t2] * 2, min(res[t3] * 3, res[t5] * 5)))
            if res[i] == res[t2] * 2:
                t2 += 1
            if res[i] == res[t3] * 3:
                t3 += 1
            if res[i] == res[t5] * 5:
                t5 += 1
        return res[-1]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**