# 《剑指offer》刷题笔记（举例让抽象具体化）：二叉树中和为某一值的路径

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

输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

## 解题思路

还是深度优先搜索，这次我们用前序遍历，就是先对root做一些该有的处理，再遍历左右子树。这里我们用到两个全局变量，result来存放最终结果，tmp用来存放临时结果。

每次遍历，我们先把root的值压入tmp，然后判断现有root是否同时满足：

- 与给定数值相减为0；
- 左子树为空；
- 右子树为空。

如果满足条件，就将tmp压入result中，否则，依次遍历左右子树。需要注意的是，遍历左右子树的时候，全局变量tmp是不清空的，直到到了根结点才请空tmp。

## C++版代码实现

### 递归

```c
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    vector<vector<int>> result;
    vector<int> tmp;
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root == NULL)
            return result;
        tmp.push_back(root->val);
        if((expectNumber - root->val) == 0 && root->left == NULL && root->right == NULL)
            result.push_back(tmp);
        FindPath(root->left, expectNumber-root->val);
        FindPath(root->right, expectNumber-root->val);
        tmp.pop_back();
        return result;
    }
};
```

## Python版代码实现

### 递归

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        # write code here
        if not root:
            return []
        if root and not root.left and not root.right and root.val == expectNumber:
            return [[root.val]]
        res = []
        left = self.FindPath(root.left, expectNumber-root.val)
        right = self.FindPath(root.right, expectNumber-root.val)
        for i in left+right:
            res.append([root.val]+i)
        return res
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**