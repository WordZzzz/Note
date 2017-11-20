# 《剑指offer》刷题笔记（分解让复杂问题简单）：二叉搜索树与双向链表

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

在计算机领域有一类算法叫分治法，即“分而治之”。采用的就是各个击破的思想，我们把分解后的小问题各个解决，然后把小问题的解决方案结合起来解决大问题。

## 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

## 解题思路

哇偶，中序遍历啊！中序遍历，递归和循环都可以撒~

二叉搜索树是一种排序的数据结构，每个结点都有两个指向子结点的指针。在双向链表中，每个结点也有两个指针，他们分别指向前一个结点和后一个结点。在二叉搜索树中，左子结点的值总是小于父结点的值，右子结点的值总是大于父结点的值。因此我们在转换成排序双向链表时，原先指向左子结点的指针调整为链表中指向前一结点的指针，原先指向右子结点的指针调整为链表中指向后一个结点指针。

中序遍历在二叉搜索树中的特点是按照从小到大的顺序遍历二叉树的每一个结点。下图中，我们可以把树分成三个部分：值为10的结点、根结点为6的左子树、根结点为14的右子树。根绝排序链表的定义，值为10的结点将和它的左子树的最大一个结点链接起来，同时它还将和右子树最小的结点链接起来。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171120114509493?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

按照中序遍历的顺序，当我们遍历转换到根结点时，它的左子树已经转换成一个排序的链表了，并且处在链表中最后一个的结点是当前值最大的结点。我们把值为8的结点和根结点链接起来，10就成了最后一个借点，接着我们就去遍历转换右子树，并把根结点和右子树中最小的结点链接起来。

下面个分别用C/C++和Python来实现，其中递归1是书中的解法，递归2是更加容易理解的解法，原理都是一样的，只不过递归1中涉及到指针的指针，容易把自己搞蒙。


## C++版代码实现

### 递归1

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
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        TreeNode *pLastNodeInList = NULL;
        ConvertNode(pRootOfTree, &pLastNodeInList);
        
        //需要返回头结点，所以需要遍历到头结点（最左子叶）
        TreeNode *pHeadOfList = pLastNodeInList;
        while(pHeadOfList != NULL && pHeadOfList->left != NULL)
            pHeadOfList = pHeadOfList->left;
        return pHeadOfList;
    }
    void ConvertNode(TreeNode* pNode, TreeNode** pLastNodeInList){
        if(pNode == NULL)
            return;
        
        TreeNode* pCurrent = pNode;
        //递归左子树
        if(pCurrent->left != NULL)
            ConvertNode(pCurrent->left, pLastNodeInList);
        //处理指针
        pCurrent->left = *pLastNodeInList;
        if(*pLastNodeInList != NULL)
            (*pLastNodeInList)->right = pCurrent;
        *pLastNodeInList = pCurrent;
        //递归右子树
        if(pCurrent->right != NULL)
            ConvertNode(pCurrent->right, pLastNodeInList);
    }
};
```

### 递归2

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
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree == NULL)
            return NULL;
        if(pRootOfTree->left == NULL && pRootOfTree->right == NULL)
            return pRootOfTree;
        //遍历左子树
        TreeNode* pLeft = Convert(pRootOfTree->left);
        TreeNode* pCurrent = pLeft;
        //定位至左子树最右的一个结点
        while(pCurrent != NULL && pCurrent->right != NULL)
            pCurrent = pCurrent->right;
        //如果左子树不为空，则将当前pRootOfTree加到左子树链表
        if(pLeft != NULL){
            pCurrent->right = pRootOfTree;
            pRootOfTree->left = pCurrent;
        }
        //遍历右子树
        TreeNode* pRight = Convert(pRootOfTree->right);
        //如果右子树不为空，则将当前pRootOfTree加到右子树链表
        if(pRight != NULL){
            pRight->left = pRootOfTree;
            pRootOfTree->right= pRight;
        }
        
        return pLeft != NULL?pLeft:pRootOfTree;
    }
};
```

### 循环

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
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        stack<TreeNode*> s;
        TreeNode* pHeadOfList = NULL;
        TreeNode* pLastNodeInList = NULL;
        TreeNode* pCurrent = pRootOfTree;
        while(pCurrent != NULL || !s.empty()){
            while(pCurrent != NULL){
                s.push(pCurrent);
                pCurrent = pCurrent->left;
            }
            if(!s.empty()){
                pCurrent = s.top();
                s.pop();
                if(pLastNodeInList != NULL){
                    pLastNodeInList->right = pCurrent;
                    pCurrent->left = pLastNodeInList;
                }
                //如果为空，则说明是最左边子结点，未来的头结点
                else
                    pHeadOfList = pCurrent;
                pLastNodeInList = pCurrent;
                pCurrent = pCurrent->right;
            }
        }
        return pHeadOfList;
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
    def Convert(self, pRootOfTree):
        # write code here
        if not pRootOfTree:
            return None
        if not pRootOfTree.left and not pRootOfTree.right:
            return pRootOfTree
        #遍历左子树
        pLeft = self.Convert(pRootOfTree.left)
        pCurrent = pLeft
        #定位至左子树最右的一个结点
        while pCurrent and pCurrent.right:
            pCurrent = pCurrent.right
        #如果左子树不为空，则将当前pRootOfTree加到左子树链表
        if pLeft:
            pCurrent.right = pRootOfTree
            pRootOfTree.left = pCurrent
        #遍历右子树
        pRight = self.Convert(pRootOfTree.right)
        #如果右子树不为空，则将当前pRootOfTree加到右子树链表
        if pRight:
            pRight.left = pRootOfTree
            pRootOfTree.right = pRight
        
        return pLeft if pLeft else pRootOfTree
```

### 循环

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        # write code here
        s = []
        pCurrent = pRootOfTree
        pHeadOfList = None
        pLastNodeInList = None
        while pCurrent or s:
            while pCurrent:
                s.append(pCurrent)
                pCurrent = pCurrent.left
            if s:
                pCurrent = s.pop()
                if pLastNodeInList:
                    pLastNodeInList.right = pCurrent
                    pCurrent.left = pLastNodeInList
                else:
                    pHeadOfList = pCurrent
                pLastNodeInList = pCurrent
                pCurrent = pCurrent.right
        return pHeadOfList
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**