# 《剑指offer》刷题笔记（树）：二叉搜索树的第k个结点

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

给定一颗二叉搜索树，请找出其中的第k大的结点。例如， 5 / \ 3 7 /\ /\ 2 4 6 8 中，按结点数值大小顺序第三个结点的值为4。

## 解题思路

我感觉大家都很容易就能想到中序遍历，因为二叉搜索树本来就是排序的，左结点小于根结点小于右结点，这个顺序正好和中序遍历的顺序一样。参考代码如下，其中遍历右子树的时候多加了对target == NULL 的判断，意思是如果target有值了就没必要往下再遍历了。

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
};
*/
class Solution {
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot == NULL || k < 1)
            return NULL;
        return KthNodeCore(pRoot, k);
    }
    
    TreeNode* KthNodeCore(TreeNode* pRoot, int &k){
        TreeNode* target = NULL;
        if(pRoot->left != NULL)
            target = KthNodeCore(pRoot->left, k);
        if(target == NULL){
            if(k == 1)
                target = pRoot;
            k--;
        }
        if(target == NULL && pRoot->right != NULL)
            target = KthNodeCore(pRoot->right, k);
        return target;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**