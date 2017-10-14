# 《剑指offer》刷题笔记（代码的鲁棒性）：链表中倒数第k个结点

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

所谓的鲁棒性是指程序能够判断输入是否符合规范要求，并对不合要求的输入予以合理的处理。

提高代码的鲁棒性的有效途径是进行防御性编程。防御性编程是一种变成习惯，是指预见在什么地方可能会出现问题，并为这些可能出现的问题制定处理方式。比如试图打开文件时发现文件不存在，我们可以提示用户检查文件名和路径等等，这样当异常发生时，软件的行为也尽在我们的掌握之中，而不至于出现不可预见的事情。

比如下面这道题，“链表中倒数第K个结点”，这里隐含着一个条件就是链表中结点的个数大于k。我们想，如果链表中的结点的数目不是大于k个，那么代码会出什么问题？这样的思考方式能够帮助我们发现潜在的问题并提前解决问题。

## 题目描述

输入一个链表，输出该链表中倒数第k个结点。

## 解题思路

定义两个指针，第一个指针从链表的头指针开始遍历向前走k-1，第二个指针保持不变；从第K步开始，第二个指针也是从链表的头指针开始遍历。由于两个指针的距离保持在k-1，当第一个指针到达链表的伪结点时，第二个指针正好是倒数第k个结点。

## C++版代码实现

### 鲁棒性低

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
            
        ListNode* pAhead = pListHead;
        ListNode* pBehind = NULL;
        
        for(unsigned int i = 0; i < k - 1; ++i){
            pAhead = pAhead->next;
        }
        
        pBehind = pListHead;
        while(pAhead->next != NULL){
            pAhead = pAhead->next;
            pBehind = pBehind->next;
        }
        return pBehind;
    }
};
```

### 鲁棒性高

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead == NULL || k == 0)
            return NULL;
            
        ListNode* pAhead = pListHead;
        ListNode* pBehind = NULL;
        
        for(unsigned int i = 0; i < k - 1; ++i){
            if(pAhead->next != NULL)
                pAhead = pAhead->next;
            else
                return NULL;
        }
        
        pBehind = pListHead;
        while(pAhead->next != NULL){
            pAhead = pAhead->next;
            pBehind = pBehind->next;
        }
        return pBehind;
    }
};
```

## Python 代码实现

### 鲁棒性差

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        pAhead = head
        pBehind = None
        
        for i in xrange(0, k-1):
            pAhead = pAhead.next
            return None
        
        pBehind = head
        while pAhead.next != None:
            pAhead = pAhead.next
            pBehind = pBehind.next
        
        return pBehind
```

### 鲁棒性高

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        if not head or k == 0:
            return None
        
        pAhead = head
        pBehind = None
        
        for i in xrange(0, k-1):
            if pAhead.next != None:
                pAhead = pAhead.next
            else:
                return None
        
        pBehind = head
        while pAhead.next != None:
            pAhead = pAhead.next
            pBehind = pBehind.next
        
        return pBehind
```

### 更快

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
 
class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        l=[]
        while head != None:
            l.append(head)
            head = head.next
        if k > len(l) or k < 1:
            return
        return l[-k]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**