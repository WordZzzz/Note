# 《剑指offer》刷题笔记（数组）：构建乘积数组

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：
给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0] * A[1] * ... * A[i-1] * A[i+1] * ... * A[n-1]。不能使用除法。

## 解题思路：
B[i]=A[0] * A[1] * ... * A[i-1] * A[i+1] * ... * A[n-1]
1.从左到右算 B[i]=A[0] * A[1] * ... * A[i-1]
2.从右到左算 B[i] * =A[i+1] * ... * A[n-1]
拒绝嵌套循环。


## C++版代码实现：

```c
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        int n = A.size();
    	vector<int> B(n);
        int ret=1;
        for(int i=0; i<n; ret*=A[i++])
            B[i] = ret;
        ret=1;
        for(int i=n-1; i>=0; ret*=A[i--])
            B[i] *=ret;
        return B;
    }
};
```

## Python版代码实现：

```python
# -*- coding:utf-8 -*-
class Solution:
    def multiply(self, A):
        # write code here
        num = len(A)
        B = [None] * num
        B[0] = 1
        for i in range(1, num):
            B[i] = B[i-1] * A[i-1]
        tmp = 1
        for i in range(num-2, -1, -1):
            tmp *= A[i+1]   
            B[i] *= tmp
        return B
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**
