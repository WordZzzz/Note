---
title: LeetCode刷题笔记（链表）：reorder-list
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - List
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

Given a singly linked list L: L 0→L 1→…→L n-1→L n,
reorder it to: L 0→L n →L 1→L n-1→L 2→L n-2→…

You must do this in-place without altering the nodes' values.

For example,
Given{1,2,3,4}, reorder it to{1,4,2,3}.

## 解题思路

要求in-place，所以不能使用辅助空间。可以采用快慢指针先找到中间点，将原列表分成两个部分，然后再将后半部分反转链表，最后再根据题目规则一前一后合并成一个链表。


## C++版代码实现

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    
    ListNode *getMiddle(ListNode *head){
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast->next != NULL && fast->next->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
    
    ListNode *reverseList(ListNode *head){
        ListNode *pReversedHead = NULL;
        ListNode *pNode = head;
        ListNode *pPrev = NULL;
        
        while(pNode != NULL){
            ListNode *pNext = pNode->next;
            if(pNext == NULL)
                pReversedHead = pNode;
            pNode->next = pPrev;
            pPrev = pNode;
            pNode = pNext;
        }
        return pReversedHead;
    }
    
    void mergeList(ListNode *left, ListNode *right){
        while(left != NULL && right != NULL){
            ListNode *curLeft = left->next;
            ListNode *curRight = right->next;
            left->next = right;
            right->next = curLeft;
            left = curLeft;
            right = curRight;
        }
    }
    
    void reorderList(ListNode *head) {
        if(head == NULL || head->next == NULL)
            return ;
        
        ListNode *middle = getMiddle(head);
        ListNode *right = reverseList(middle->next);
        middle->next = NULL;
        ListNode *left = head;
        mergeList(left, right);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**