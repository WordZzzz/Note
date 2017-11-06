# 《剑指offer》刷题笔记（举例让抽象具体化）：二叉搜索树的后序遍历序列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言：

当一眼看不出问题中隐藏的规律时，我们可以试着用一两个具体的例子模拟操作的过程，说不定这样那就能通过具体的例子找到抽象的规律。

## 题目描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

## 解题思路

深度优先遍历中的先序遍历、中序遍历、后序遍历，都是针对在遍历中根结点的位置来命名的，先序遍历，第一个元素是根结点，后序遍历，最后一个元素是根结点。

二叉搜索树中，在后序遍历得到的序列中，最后一个数字是树的根结点的值。数组中前面的数字可以分为两部分：第一部分是左子树结点的值，他们都比根节点的值小；第二部分是右子树结点的值，他们都比根结点的值大。

递归循环都可以实现。

## C++版代码实现

### 递归

```c
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        return bst(sequence,0,sequence.size()-1);
    }
    bool bst(vector<int> sequence,int begin,int end){
        if(sequence.empty()||begin>end)
            return false;
        //获取根节点，后序遍历最后一个元素就是根节点
        int root = sequence[end];
        //在二叉树搜索树中左子树的结点小于根节点
        int i = begin;
        for(; i < end; ++i)
            if(sequence[i] > root)
                break;
        //在二叉搜索树中右子树的节点大于根节点
        for(int j = i; j < end; ++j)
            if(sequence[j] < root)
                return false;
        //判断左子树是不是二叉搜索树
        bool left = true;
        if(i > begin)
            left = bst(sequence, begin, i-1);
        //判断右子树是不是二叉搜索树
        bool right=true;
        if(i < end - 1)
            right = bst(sequence, i, end-1);
        return left && right;
    }
};
```

### 循环

```c
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence)
    {
        int length = sequence.size();
        if(length == 0)
            return false;
        int i = 0;
        while(--length)
        {
            while(sequence[i++] < sequence[length]);
            while(sequence[i++] > sequence[length]);
            if(i < length)
                return false;
            i = 0;
        }
        return true;
    }
};
```

## Python版代码实现

### 递归

```python
# -*- coding:utf-8 -*-
class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        if not sequence:
            return False
        if len(sequence) == 1:
            return True
        length=len(sequence)
        # 获取根节点，后序遍历最后一个元素就是根节点
        root=sequence[-1]
        # 在二叉树搜索树中左子树的结点小于根节点
        i=0
        while  sequence[i] < root:
            i=i+1
        # 在二叉搜索树中右子树的节点大于根节点
        k=i
        for j in range(i, length-1):
            if sequence[j] < root:
                return False
        left_s = sequence[:k]
        right_s = sequence[k:length-1]
        # 判断左子树是不是二叉搜索树
        leftIs = True
        if len(left_s) > 0:
            leftIs = self.VerifySquenceOfBST(left_s)
        # 判断右子树是不是二叉搜索树
        rightIs = True
        if len(right_s) > 0:
            rightIs = self.VerifySquenceOfBST(right_s)
        return leftIs and rightIs
```

### 循环

```python
# -*- coding:utf-8 -*-
class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        if not sequence:
            return False
        if len(sequence)==1:
            return True
        i = 0
        length=len(sequence)
        while length:
            length -= 1
            while sequence[i] < sequence[length]:
                i += 1
            while sequence[i] > sequence[length]:
                i += 1
            if i < length:
                return False
            i = 0
        return True
```


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**