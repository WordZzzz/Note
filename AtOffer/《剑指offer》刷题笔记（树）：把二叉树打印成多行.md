# 《剑指offer》刷题笔记（树）：把二叉树打印成多行

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

## 解题思路

层序遍历，之前求树的深度的时候简单用过。

在线测试的测试结果需要是一个二维数组（vector<vector<int> >），而不是原著中的每一层输出一行（其实意思都差不多）。

queue是先进先出规则，我们初始化两个queue（q1和q2）交替来存储每一层结点（这种结构在下一个题目中也将用到，只不过queue变成了stack），从而达到所谓的每一层输出一行。

---2018/02/05更新开始---

第一次做这道题的时候，竟然错把两个队列的实现当成了最优解。今天再回来看这道题，明明可以一个队列就能解决嘛，我也忘了当时自己是怎么想的了。

所以这次将单队列的代码补上，同时保留双队列的代码。

---2018/02/05更新结束---


## C++版代码实现

### 单队列

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
        vector<vector<int> > ret;
        queue<TreeNode*> que;
        if(pRoot) que.push(pRoot);
        while(!que.empty()){
            vector<int> tmp;
            int n = que.size();
            while(n--){
                TreeNode* node = que.front();
                tmp.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
                que.pop();
            }
            ret.push_back(tmp);
        }
        return ret;
    }
};
```

### 双队列

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
            queue<TreeNode*> q1;
            queue<TreeNode*> q2;
            q1.push(pRoot);
            while(!q1.empty()||!q2.empty()){
                vector<int> v1;
                vector<int> v2;
                while(!q1.empty()){
                    v1.push_back(q1.front()->val);
                    if(q1.front()->left!=NULL) q2.push(q1.front()->left);
                    if(q1.front()->right!=NULL) q2.push(q1.front()->right);
                    q1.pop();
                }
                if(v1.size()!=0)
                ret.push_back(v1);
                while(!q2.empty()){
                    v2.push_back(q2.front()->val);
                    if(q2.front()->left!=NULL)   q1.push(q2.front()->left);
                    if(q2.front()->right!=NULL)  q1.push(q2.front()->right);
                    q2.pop();
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