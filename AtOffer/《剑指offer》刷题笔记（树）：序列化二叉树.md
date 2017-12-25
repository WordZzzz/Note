# 《剑指offer》刷题笔记（树）：序列化二叉树

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

请实现两个函数，分别用来序列化和反序列化二叉树。

## 解题思路

这道题其实没什么好说的，在牛客网上，即使你在反序列化的时候直接输出root也能通过测试。

书上倒是写的听清楚。用到了dfs的思想，通过前序遍历来序列化或者反序列化。这块你只要自己写的格式能对应上，都是可以的。比如可以按照书中那样用$表示null并且用逗号分隔每个节点，也可以直接用下面代码所示的0xFFFFFFFF标识null。

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
    vector<int> buf;
    
    void dfs1(TreeNode *root){
        if(!root)
            buf.push_back(0xFFFFFFFF);
        else{
            buf.push_back(root->val);
            dfs1(root->left);
            dfs1(root->right);
        }
    }
    
    TreeNode* dfs2(int* &str){
        if(*str == 0xFFFFFFFF){
            ++str;
            return NULL;
        }
        TreeNode* res = new TreeNode(*str);
        str++;
        res->left = dfs2(str);
        res->right = dfs2(str);
        return res;
    }
    
    char* Serialize(TreeNode *root) {    
        buf.clear();
        dfs1(root);
        int bufSize = buf.size();
        int *res = new int[bufSize];
        for(int i = 0;i < bufSize; i++) res[i] = buf[i];
        return (char*)res;
    }
    TreeNode* Deserialize(char *str) {
        int *p = (int*)str;
        return dfs2(p);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**