# 《剑指offer》刷题笔记（树）：二叉树的下一个结点

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

## 解题思路

如果一个结点有右子树，那么它的下一个结点就是它的右子树中的最左子结点；
如果给定结点是其父结点的左子结点，那么它的下一个结点就是它的父结点；
如果一个结点既没有右子树，并且它还是它父结点的右子结点，那我们就需要一直向上遍历，知道找到一个是其父结点的左子结点的结点。

对最后一种情况做下解释，比如我们为了找到下图中结点i的下一个结点，我们沿着指向父结点的指针向上遍历，先到达结点e。由于结点e是它父结点的b的右结点，我们继续遍历到b。b是其父结点a的左子结点，因此b的父结点a就是结点i的下一个结点。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171222173052841?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++版代码实现

```c
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        if(pNode == NULL)
            return NULL;
        
        TreeLinkNode* pNext = NULL;
        //当前结点有右子树，则下一结点是右子树的最左结点
        if(pNode->right != NULL){
            TreeLinkNode* pRight = pNode->right;
            while(pRight->left != NULL)
                pRight = pRight->left;
            pNext = pRight;
        }
        //当前结点无右子树，则需要向上找到其父结点（可以是当前结点）可以作为左子树的时候
        else if(pNode->next != NULL){
            TreeLinkNode* pCurrent = pNode;
            TreeLinkNode* pParrent = pNode->next;
            while(pParrent != NULL && pCurrent == pParrent->right){
                pCurrent = pParrent;
                pParrent = pParrent->next;
            }
            pNext = pParrent;
        }
        return pNext;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**