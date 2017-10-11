# 《剑指offer》刷题笔记（代码完整性）：调整数组顺序使奇数位于偶数前面

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

书上这道题没要求相对位置不变，所以这道题我们以牛客网为准，毕竟有测试平台。

## 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

## 解题思路

方法一：

类似冒泡算法，前偶后奇就交换。时间复杂度O(n^2)。

方法二：

空间换时间，再创建一个数组，或者双向队列。时间复杂度O(n)，空间复杂度O(n)。

方法三：

双向队列，一次循环插入。

## C++版代码实现：

### 冒泡

```c
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        for (int i = 0; i < array.size(); i++)
        {
            for (int j = array.size() - 1; j  > i; j--)
            {
                if (array[j] % 2 == 1 && array[j - 1] % 2 == 0) //前偶后奇交换
                {
                    swap(array[j], array[j-1]);
                }
            }
        }
    }
};
```

### 新建数组

```
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        vector<int> result;
        int num = array.size();
        for(int i=0; i < num; i++)
            {
            if(array[i] % 2 == 1)
                result.push_back(array[i]);
        }
        for(int i=0; i < num; i++)
            {
            if(array[i] % 2 == 0)
                result.push_back(array[i]);
        }
        array = result;
    }
};
```

### 双向队列

```
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        deque<int> result;
        int num = array.size();
        for(int i=0; i < num; i++)
            {
            if(array[num-i-1] % 2 == 1)
                result.push_front(array[num-i-1]);
            if(array[i] % 2 == 0)
                result.push_back(array[i]);
        }
        array.assign(result.begin(),result.end());
    }
};
```

## Python 代码实现：

### 冒泡：

```python
# -*- coding:utf-8 -*-
class Solution:
    def reOrderArray(self, array):
        # write code here
        for i in range(0, len(array)):
            for j in range(len(array)-1, i, -1):
                if array[j-1] % 2 == 0 and array[j] % 2 == 1:
                    array[j], array[j-1] = array[j-1], array[j]
        return array
```

### 新建列表

```
# -*- coding:utf-8 -*-
class Solution:
    def reOrderArray(self, array):
        # write code here
        result = []
        for i in range(len(array)):
            if array[i] % 2 != 0:
                result.append(array[i])
        for i in range(len(array)):
            if array[i] % 2 == 0:
                result.append(array[i])
        return result
```

### 双向队列

```
# -*- coding:utf-8 -*-
from collections import deque
class Solution:
    def reOrderArray(self, array):
        # write code here
        odd = deque()
        l = len(array)
        for i in range(l):
            if array[l-i-1] % 2 != 0:
                odd.appendleft(array[l-i-1])
            if array[i] % 2 == 0:
                odd.append(array[i])
        return list(odd)
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**