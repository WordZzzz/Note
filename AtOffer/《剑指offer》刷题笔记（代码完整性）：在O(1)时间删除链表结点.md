# 《剑指offer》刷题笔记（代码完整性）：在O(1)时间删除链表结点

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

同样的，这道题牛客网上也没有。

## 题目描述

给定单向链表的头指针和一个结点指针，定义一个函数在O(1)时间删除该结点。链表结点与函数的定义如下：

```
struct ListNode{
    int m_nValue;
    ListNode* m_pNext;
}

void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted);
```

## 解题思路

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171010113522746?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

方法一：顺序遍历（时间复杂度O(n)）

从单向链表中删除一个结点，最常规的做法就是从链表的头结点开始，顺序遍历查找要删除的结点，并在链表中删除该结点。之所以需要从头开始查找，是因为我们需要得到将被删除的节点的前面一个结点。在单向链表中，结点中没有指向前一个结点的指针，所以只好从链表的头结点开始顺序查找。

方法二：复制结点（时间复杂度O(1)）

当然，我们并不是非得得到前一个结点才能来删除该结点。上图C中，我们要删除结点i，先把i的下一个结点j的内容复制到i，然后把i的指针指向结点j的下一个结点。此时再删除节点j，其效果刚好是把结点i给删除了。

但是，这种思路，需要考虑删除尾结点的问题，这个时候只能顺序遍历。同时，还要注意如果链表中只有一个结点的情况，记得删除之后把头结点设置为NULL。

## C++版代码实现

### 顺序遍历

```c
void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        return;

    ListNode* pNode = *pListHead;
    while(pNode->m_pNext != pToBeDeleted)
    {
        pNode = pNode->m_pNext;            
    }

    pNode->m_pNext = nullptr;
    delete pToBeDeleted;
    pToBeDeleted = nullptr;

}
```

### 复制结点

```c
void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        return;

    // 要删除的结点不是尾结点
    if(pToBeDeleted->m_pNext != nullptr)
    {
        ListNode* pNext = pToBeDeleted->m_pNext;
        pToBeDeleted->m_nValue = pNext->m_nValue;
        pToBeDeleted->m_pNext = pNext->m_pNext;
 
        delete pNext;
        pNext = nullptr;
    }
    // 链表只有一个结点，删除头结点（也是尾结点）
    else if(*pListHead == pToBeDeleted)
    {
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
        *pListHead = nullptr;
    }
    // 链表中有多个结点，删除尾结点
    else
    {
        ListNode* pNode = *pListHead;
        while(pNode->m_pNext != pToBeDeleted)
        {
            pNode = pNode->m_pNext;            
        }
 
        pNode->m_pNext = nullptr;
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
    }
}
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**