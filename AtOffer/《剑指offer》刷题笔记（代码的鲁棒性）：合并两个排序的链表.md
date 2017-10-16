# 《剑指offer》刷题笔记（代码的鲁棒性）：合并两个排序的链表

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

在面试过程中，最容易犯两种错误：一是在写代码之前没有对合并的过程想清楚，最终合并出来的链表要么中间断开了要么并没有做到递增排序；二是代码在鲁棒性方面存在问题，程序一旦有特殊的输入（如空链表）就会崩溃。

## 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

## 解题思路

递归：

新创建一个指针就可以，比较两个链表的值，然后做相应递归更新。

循环：

需要创建两个指针，一个指向合并链表的表头，另一个用于更新，不断指向合并链表的表尾。最后返回指向表头的指针即可。

需要注意的是，我为了简化代码，新建的是指向带有头结点的链表（链表有带头结点和不带头结点点两种）。如果全部初始化为NULL（python是None），那么我在循环之前，就得加个判断来给合并链表的第一个结点赋值。

## C++版代码实现

### 递归

```c
/*
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
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        //边界判断
        if(pHead1 == NULL)
            return pHead2;
        else if(pHead2 == NULL)
            return pHead1;
        //创建头指针
        ListNode* pMergeHead = NULL;
        if(pHead1->val < pHead2->val){
            pMergeHead = pHead1;
            pMergeHead->next = Merge(pHead1->next, pHead2);
        }
        else{
            pMergeHead = pHead2;
            pMergeHead->next = Merge(pHead1, pHead2->next);
        }
        return pMergeHead;
    }
};
```

### 循环

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
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        //边界判断
        if(pHead1 == NULL)
            return pHead2;
        else if(pHead2 == NULL)
            return pHead1;
        //创建头尾指针
        ListNode* pMergeTail = new ListNode(0);
        ListNode* pMergeHead = new ListNode(0);
        //尾指针赋值
        pMergeTail = pMergeHead;
        //循环开始
        while(pHead1 && pHead2){
            if(pHead1->val < pHead2->val){
                pMergeTail->next = pHead1;
                pHead1 = pHead1->next;
            }
            else{
                pMergeTail->next = pHead2;
                pHead2 = pHead2->next;
                }
            pMergeTail = pMergeTail->next;
        }
        //剩下的链表部分直接添加
        pMergeTail->next = pHead1 ? pHead1 : pHead2;
        return pMergeHead->next;
    }
};
```

## Python 代码实现

### 递归

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if pHead1 is None:
            return pHead2
        elif pHead2 is None:
            return pHead1
        
        pMergeHead = ListNode(0)
        
        if pHead1.val < pHead2.val:
            pMergeHead = pHead1
            pMergeHead.next = self.Merge(pHead1.next, pHead2)
        else:
            pMergeHead = pHead2
            pMergeHead.next = self.Merge(pHead1, pHead2.next)
            
        return pMergeHead
```

### 循环

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if pHead1 is None:
            return pHead2
        elif pHead2 is None:
            return pHead1
        
        pMergeTail = pMergeHead = ListNode(0)
        
        while pHead1 and pHead2:
            if pHead1.val < pHead2.val:
                pMergeTail.next = pHead1
                pHead1 = pHead1.next
            else:
                pMergeTail.next = pHead2
                pHead2 = pHead2.next
            pMergeTail = pMergeTail.next
            
        pMergeTail.next = pHead1 or pHead2
        return pMergeHead.next
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**