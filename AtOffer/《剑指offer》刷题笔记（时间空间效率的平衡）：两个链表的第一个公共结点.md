# 《剑指offer》刷题笔记（时间空间效率的平衡）：两个链表的第一个公共结点

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入两个链表，找出它们的第一个公共结点。

## 解题思路

我们一定要理解题目：输入两个链表，找出它们的第一个公共结点。从链表结点的定义可以看出，这两个链表是单向链表。如果两个单向链表有公共的结点，那么这两个链表从某一个结点开始，他妈嗯的next都指向同一个结点。但是，我们都知道，每个单向链表的结点只有一个next，因此，因此，因此，敲重点了，从第一个公共结点开始，之后他们所有的结点必定重合，也就是最终肯定是Y型而不是X型。

理解了题目，接下来的一切都好说了。

方法一：

我们可以把两个链表拼接起来，一个pHead1在前，一个pHead2在前，这样，生成了两个相同长度的链表，那么我们只要一同遍历这两个表，就一定能找到公共结点。时间复杂度O(m+n)，空间复杂度O(m+n)。

方法二：

我们也可以先让把长的链表的头砍掉，让两个链表长度相同，这样，同时遍历也能找到公共结点。此时，时间复杂度O(m+n)，空间复杂度为O(MAX(m,n))。

## C++版代码实现

### 简单粗暴

```c
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        ListNode *p1 = pHead1;
        ListNode *p2 = pHead2;
        while(p1!=p2){
            p1 = (p1==NULL ? pHead2 : p1->next);
            p2 = (p2==NULL ? pHead1 : p2->next);
        }
        return p1;
    }
};
```

### 节省空间

```c
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        unsigned int len1 = getLength(pHead1);
        unsigned int len2 = getLength(pHead2);
        int nLengthDif = len1-len2;
        
        ListNode* pHeadLong = pHead1;
        ListNode* pHeadShort = pHead2;
        
        if(len1 < len2){
            pHeadLong = pHead2;
            pHeadShort = pHead1;
            nLengthDif = len2-len1;
        }
        
        for(int i = 0; i < nLengthDif; ++i){
            pHeadLong = pHeadLong->next;
        }
        
        while(pHeadLong != NULL && pHeadShort != NULL && pHeadShort != pHeadLong){
            pHeadLong = pHeadLong->next;
            pHeadShort = pHeadShort->next;
        }
        
        return pHeadLong;
    }
    
    unsigned int getLength( ListNode* pHead){
        unsigned int len = 0;
        ListNode* pNode = pHead;
        while(pNode != NULL){
            ++ len;
            pNode = pNode->next;
        }
        return len;
    }
};
```

## Python版代码实现

### 简单粗暴

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        p1 = pHead1
        p2 = pHead2
        while p1!=p2:
            p1 = p1.next if p1!=None else pHead2
            p2 = p2.next if p2!=None else pHead1
        return p1
```

### 节省空间

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        def GetListLength(pHead):
            length = 0
            pNode = pHead
            while pNode != None:
                length += 1
                pNode = pNode.next
            return length
        
        length1 = GetListLength(pHead1)
        length2 = GetListLength(pHead2)
        lengthDif = abs(length1 - length2)
        
        pListHeadLong = pHead1
        pListHeadShort = pHead2
        
        if length1 < length2:
            pListHeadLong = pHead2
            pListHeadShort = pHead1
        
        for i in range(lengthDif):
            pListHeadLong = pListHeadLong.next
        
        while (pListHeadLong != None) and (pListHeadShort != None) and (pListHeadLong != pListHeadShort):
            pListHeadLong = pListHeadLong.next
            pListHeadShort = pListHeadShort.next
        return pListHeadLong
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**