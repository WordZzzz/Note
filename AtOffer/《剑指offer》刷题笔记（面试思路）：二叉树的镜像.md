# 《剑指offer》刷题笔记（面试思路）：二叉树的镜像

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 解题思路：
还是BFS和DFS的套路，要么递归实现要么利用队列进行层序遍历。

## C++版代码实现：
### 递归：
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
    void Mirror(TreeNode *pRoot) {
		if(pRoot){
            swap(pRoot->left, pRoot->right);
            Mirror(pRoot->left);
            Mirror(pRoot->right);
        }
    }
};
```
### 层序遍历：
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
    void Mirror(TreeNode *pRoot) {
		if(!pRoot) return;
        TreeNode* node;
        queue<TreeNode*> que;
        que.push(pRoot);
        while(!que.empty()){
            node = que.front();
            que.pop();
            swap(node->left, node->right);
            if(node->left) que.push(node->left);
            if(node->right) que.push(node->right);
        }
    }
};
```

## Python版代码实现：
### 递归：
```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回镜像树的根节点
    def Mirror(self, root):
        # write code here
        if root != None:
            root.left,root.right = root.right,root.left
            self.Mirror(root.left)
            self.Mirror(root.right)
```
### 层序遍历：
```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回镜像树的根节点
    def Mirror(self, root):
        # write code here
        if not root: return
        a=[root]
        while a:
            b=[]
            for node in a:
                node.left,node.right = node.right,node.left
                if node.left:
                    b.append(node.left)
                if node.right:
                    b.append(node.right)
            a=b
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**
