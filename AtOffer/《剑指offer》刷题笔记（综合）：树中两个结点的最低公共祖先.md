# 《剑指offer》刷题笔记（综合）：树中两个结点的最低公共祖先

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入两个树节点，求他们的最低公共祖先。

## 解题思路

求树中两个节点的最低公共祖先，不能说只是一个题目，而应该说是一组题目。不同条件下的题目解法是完全不一样的，比如，是否是二叉树、二叉排序树；如果不是二叉树，是否有指向父节点的指针（是的话可以转换成求两个链表的第一个公共节点，不是的话可以转换成求两个链表的最后一个公共节点）。

下面的代码针对的是普通树，并且没有指向父节点的指针，所以我们转换成求两个链表的最后一个公共节点。代码中GetNodePath用来得到根节点pRoot开始到达节点pNode的路径，这条路径保存在path中，函数GetLastCommonNode用来得到两个路径path1和path2的最后一个公共节点。函数GetLastCommonParent先调用GetNodePath得到pRoot到达pNode1的路径path1，再得到pRoot到达pNode2的路径path2，接着调用GetLastCommonNode得到path1和path2的最后一个公共节点，即我们要找的最低公共祖先。

## C++版代码实现

```c
bool GetNodePath(const TreeNode* pRoot, const TreeNode* pNode, list<const TreeNode*>& path)
{
    if(pRoot == pNode)
        return true;
 
    path.push_back(pRoot);
 
    bool found = false;

    vector<TreeNode*>::const_iterator i = pRoot->m_vChildren.begin();
    while(!found && i < pRoot->m_vChildren.end())
    {
        found = GetNodePath(*i, pNode, path);
        ++i;
    }
 
    if(!found)
        path.pop_back();
 
    return found;
}

const TreeNode* GetLastCommonNode
(
    const list<const TreeNode*>& path1, 
    const list<const TreeNode*>& path2
)
{
    list<const TreeNode*>::const_iterator iterator1 = path1.begin();
    list<const TreeNode*>::const_iterator iterator2 = path2.begin();
    
    const TreeNode* pLast = nullptr;
 
    while(iterator1 != path1.end() && iterator2 != path2.end())
    {
        if(*iterator1 == *iterator2)
            pLast = *iterator1;
 
        iterator1++;
        iterator2++;
    }
 
    return pLast;
}

const TreeNode* GetLastCommonParent(const TreeNode* pRoot, const TreeNode* pNode1, const TreeNode* pNode2)
{
    if(pRoot == nullptr || pNode1 == nullptr || pNode2 == nullptr)
        return nullptr;
 
    list<const TreeNode*> path1;
    GetNodePath(pRoot, pNode1, path1);
 
    list<const TreeNode*> path2;
    GetNodePath(pRoot, pNode2, path2);
 
    return GetLastCommonNode(path1, path2);
}
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**