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

书上倒是写的听清楚。用到了dfs的思想，通过前序遍历来序列化或者反序列化。这块你只要自己写的格式能对应上，都是可以的。比如可以按照书中那样用$表示null并且用逗号分隔每个节点，也可以直接用下面第一种方法代码所示的0xFFFFFFFF标识null。需要注意的是，如果使用string来实现序列化之后的存储，那么在每个结点之后也要加分隔符，例如','。

下面贴出了三种实现方式

- 基于vector，好处就是你不需要在每个结点之后再加分隔符；
- 基于string，其他平台序列化之后的返回值类型都是string，牛客网搞特殊来了个char *，所以最后需要进行类型转换；
- 基于stringstream，代码更加简洁；

## C++版代码实现

### vector

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

### string

```c
class Solution {
public:
    void serializeHelper(TreeNode *node, string& s)
    {
        if (node == NULL)
        {
            s.push_back('#');
            s.push_back(',');
            return;
        }
        s += to_string(node->val);
        s.push_back(',');
        serializeHelper(node->left, s);
        serializeHelper(node->right, s);
    }
    char* Serialize(TreeNode *root)
    {
        if (root == NULL)
            return NULL;
        string s = "";
        serializeHelper(root, s);
 
        char *ret = new char[s.length() + 1];
        strcpy(ret, s.c_str());
        return ret;
    }
     
    TreeNode *deserializeHelper(string &s)
    {
        if (s.empty())
            return NULL;
        if (s[0] == '#')
        {
            s = s.substr(2);
            return NULL;
        }
        TreeNode *ret = new TreeNode(stoi(s));
        s = s.substr(s.find_first_of(',') + 1);
        ret->left = deserializeHelper(s);
        ret->right = deserializeHelper(s);
        return ret;
    }
     
    TreeNode* Deserialize(char *str)
    {
        if (str == NULL)
            return NULL;
        string s(str);
        return deserializeHelper(s);
    }
};
```

### stringstream

```c
class Solution {
public:
    string sHelper(TreeNode *node)
    {
        if (node == NULL)
            return "#";
        return to_string(node->val) + "," +
                sHelper(node->left) + "," +
                sHelper(node->right);
    }
 
    char* Serialize(TreeNode *root)
    {
        string s = sHelper(root);
        char *ret = new char[s.length() + 1];
        strcpy(ret, const_cast<char*>(s.c_str()));
        return ret;
    }
 
 
    TreeNode *dHelper(stringstream &ss)
    {
        string str;
        getline(ss, str, ',');
        if (str == "#")
            return NULL;
        else
        {
            TreeNode *node = new TreeNode(stoi(str));
            node->left = dHelper(ss);
            node->right = dHelper(ss);
            return node;
        }
    }
 
    TreeNode* Deserialize(char *str) {
        stringstream ss(str);
        return dHelper(ss);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**