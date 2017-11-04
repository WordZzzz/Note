# 《剑指offer》刷题笔记（举例让抽象具体化）：从上往下打印二叉树

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

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

## 解题思路

再熟悉不过的层序遍历，BFS即可实现。用队列来进行层序遍历，同时用一个vector容器来存储每一层的值。

举例如下：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171104152007252?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++版代码实现

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
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int>  result;
        queue<TreeNode*> que;
        TreeNode* fr;
        if(root == NULL)
            return result;
        que.push(root);
        while(!que.empty()){
            fr = que.front();
            result.push_back(fr->val);
            if(fr->left != NULL)
                que.push(fr->left);
            if(fr->right != NULL)
                que.push(fr->right);
            que.pop();
        }
        return result;
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回从上到下每个节点值列表，例：[1,2,3]
    def PrintFromTopToBottom(self, root):
        # write code here
        result=[]
        if not root:
            return []
        que=[]
        que.append(root)
        while len(que):
            t=que.pop(0)
            result.append(t.val)
            if t.left:
                que.append(t.left)
            if t.right:
                que.append(t.right)
        return result
```

**系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～**

**完的汪(∪｡∪)｡｡｡zzz**
