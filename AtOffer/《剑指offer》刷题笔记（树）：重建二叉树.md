# 《剑指offer》刷题笔记（树）：重建二叉树

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

树是一种实际编程中经常遇到的数据结构。由于树的操作会设计大量的指针，因此与树有关的面试题都不太容易。当面试官想考查应聘者在有复杂指针操作的情况下写代码的能力，他往往会想到用与树有关的面试题。

面试的时候提到的树，大部分都是二叉树。所谓二叉树是树的一种特殊结构，在二叉树中每个结点最多只能有两个子结点。在二叉树中最重要的操作莫过于遍历，即按照某一顺序访问树中的所有结点。通常树有如下几种遍历方式：

- 前序遍历：先访问根结点，再访问左子结点，最后访问右子结点。
- 中序遍历：先访问左子结点，再访问根结点，最后访问右子结点。
- 后序遍历：先访问左子结点，再访问右子结点，最后访问根结点。

前中后都是指的根结点在遍历中的位置。每一种遍历都有递归和循环两种不同的实现方式，每一种遍历的递归实现都比循环实现要简捷很多。我们需要对着三种遍历的六种实现方法都了如指掌。

- 宽度优先遍历：先访问树的第一层结点，再访问树的第二层结点，一直访问到最下面一层节点。在同一层结点中，以从左到右的顺序依次访问。

特例：
- 二叉搜索树。在二叉搜索树中，左子结点总是小于或等于根结点，而右子结点总是大于或等于根结点。我们可以平均在O(logn)的时间内根据数值在二叉搜索树中找到一个结点。
- 堆。堆分为最大堆和最小堆。在最大堆中根结点的值最大，在最小堆中结点的值最小。有很多需要快速找到最大值或者最小值的问题都可以用堆来解决。
- 红黑树。红黑树是把树中的结点定义为红、黑两种颜色，并通过规则确保从根结点到叶结点的最长路径的长度不超过最短路径的两倍。
- 
在C++的STL中，set、multiset、map、multimap等数据结构都是基于红黑树实现的。

## 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

## 解题思路

前序遍历序列中，第一个数字总是树的根结点的值。在中序遍历序列中，根结点的值在序列的中间，左子树的结点的值位于根结点的值的左边，而右子树的结点的值位于根结点的值的右边。剩下的我们可以递归来实现，具体如图：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170927101737391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++代码实现

```c
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre, vector<int> vin) {
        int vinlen = vin.size();
        if(vinlen == 0)
            return NULL;
        vector<int> left_pre, right_pre, left_vin, right_vin;
        //创建根节点，根节点肯定是前序遍历的第一个数
        TreeNode* head = new TreeNode(pre[0]);
        //找到中序遍历根节点所在位置,存放于变量gen中
        int gen = 0;
        for(int i = 0; i < vinlen; i++)
        {
            if (vin[i] == pre[0])
            {
                gen = i;
                break;
            }
        }
        //对于中序遍历，根节点左边的节点位于二叉树的左边，根节点右边的节点位于二叉树的右边
        //利用上述这点，对二叉树节点进行归并
        for(int i = 0; i < gen; i++)
        {
            left_vin.push_back(vin[i]);
            left_pre.push_back(pre[i+1]);//前序第一个为根节点
        }
        for(int i = gen + 1; i < vinlen; i++)
        {
            right_vin.push_back(vin[i]);
            right_pre.push_back(pre[i]);
        }
        //取出前序和中序遍历根节点左边和右边的子树
        //递归，再对其进行上述所有步骤，即再区分子树的左、右子子数，直到叶节点
        head->left=reConstructBinaryTree(left_pre,left_vin);
        head->right=reConstructBinaryTree(right_pre,right_vin);
        return head;
    }
};
```

## Python代码实现

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        if len(pre) == 0:
            return None
        if len(pre) == 1:
            return TreeNode(pre[0])
        else:
            flag = TreeNode(pre[0])
            flag.left = self.reConstructBinaryTree(pre[1:tin.index(pre[0])+1],tin[:tin.index(pre[0])])
            flag.right = self.reConstructBinaryTree(pre[tin.index(pre[0])+1:],tin[tin.index(pre[0])+1:] )
        return flag
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**