# 《剑指offer》刷题笔记：二叉树的深度

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：
输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

## 解题思路：
DFS:题目很简单，用递归遍历只需一行。如果传入的树指针为空指针，则直接返回0；否则，递归调用TreeDepth，遍历左右子树并返回最大值。
BFS:层次遍历，需要使用队列。如果队列不为空，则在循环内不断的pop根节点、push左右子树，同时累加depth。

## C++版代码实现：
### 递归：
```
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
    int TreeDepth(TreeNode* pRoot)
    {
        return pRoot ? max(TreeDepth(pRoot->left),TreeDepth(pRoot->right))+1 :0;
    }
};


/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};
```
### 层序遍历：
```
*/
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
        if(!pRoot) return 0;
        queue<TreeNode*> que;
        int depth = 0;
        que.push(pRoot);
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i=0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return depth;
    }
};
```

## Python版代码实现：
### 递归：
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0
        return max(self.TreeDepth(pRoot.left),self.TreeDepth(pRoot.right))+1
```
### 层次遍历：
```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        # write code here
        if not pRoot:
            return 0
        a=[pRoot]
        depth=0
        while a:
            b=[]
            for node in a:
                if node.left:
                    b.append(node.left)
                if node.right:
                    b.append(node.right)
            a=b
            depth=depth+1
        return depth
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**