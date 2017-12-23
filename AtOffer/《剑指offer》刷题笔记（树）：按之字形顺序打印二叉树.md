# 《剑指offer》刷题笔记（树）：按之字形顺序打印二叉树
----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

## 解题思路

这道题和上一道《把二叉树打印成多行》的思路是一样的，只不过，我们需要考虑如何来实现奇偶行的不同顺序输出。首先，肯定不能再上一道题的基础上对偶数行进行reverse，那样效率太低，根本不实用。

如果还和上一道题一样用queue（左右结点的遍历顺序稍作更改），我们会发现，对于一个三层的二叉树（1-2-3-4-5-6-7），前两行都没问题，到了第三行，就乱了，无论你怎么更改第二个queue的遍历顺序。原因就在于，我上一行输出的最后一个结点，它的子结点要紧接着作为下一个输出才对，而queue是先进先出，所以你上一行输出的最后一个结点，它的子结点只能在同行中最后输出。

先入先出不行，我们就用先入后出的stack。对于一个三层的二叉树（1-2-3-4-5-6-7），第一个stack压入1，弹出1后第二个stack顺序压入2、3，顺序弹出3（压7、6）、2（压5、4）后第一个stack顺序压入7、6、5、4，顺序弹出4、5、6、7后结束。没毛病。

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
        vector<vector<int> > Print(TreeNode* pRoot) {
            vector<vector<int>> ret;
            if(pRoot==NULL) return ret;
            stack<TreeNode*> s1;
            stack<TreeNode*> s2;
            s1.push(pRoot);
            while(!s1.empty()||!s2.empty()){
                vector<int> v1;
                vector<int> v2;
                while(!s1.empty()){
                    v1.push_back(s1.top()->val);
                    if(s1.top()->left!=NULL) s2.push(s1.top()->left);
                    if(s1.top()->right!=NULL) s2.push(s1.top()->right);
                    s1.pop();
                }
                if(v1.size()!=0)
                ret.push_back(v1);
                while(!s2.empty()){
                    v2.push_back(s2.top()->val);
                    if(s2.top()->right!=NULL)   s1.push(s2.top()->right);
                    if(s2.top()->left!=NULL)  s1.push(s2.top()->left);
                    s2.pop();
                }
                if(v2.size()!=0)
                ret.push_back(v2);
            }
            return ret;
        }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**