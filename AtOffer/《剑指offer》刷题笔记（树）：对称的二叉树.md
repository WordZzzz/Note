# 《剑指offer》刷题笔记（树）：对称的二叉树

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

## 解题思路

我们可以通过比较二叉树的前序遍历序列和对称前序遍历序列来判断二叉树是不是对称的。如果两个序列是一样的，那么二叉树就是对称的。需要注意的是，我们一定要加上null值得判断，要不然如果一棵树所有结点数值都一样的时候，我们就没办法区分是否对称了。

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
    bool isSymmetrical(TreeNode* pRoot)
    {
        return isSymmetrical(pRoot, pRoot);
    }
    bool isSymmetrical(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot1 == NULL && pRoot2 == NULL)
            return true;
        if(pRoot1 == NULL || pRoot2 == NULL)
            return false;
        if(pRoot1->val != pRoot2->val)
            return false;
        return isSymmetrical(pRoot1->left, pRoot2->right)
            && isSymmetrical(pRoot1->right, pRoot2->left);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**