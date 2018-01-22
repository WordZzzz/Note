---
title: LeetCode刷题笔记（链表）：remove-nth-node-from-end-of-list
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

Given a linked list, remove the n th node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
Note: 
Given n will always be valid.
Try to do this in one pass.

## 解题思路

删除倒数第n个结点，我们就需要找到倒数第n个结点。于是，我们可以利用快慢指针来实现。

首先，fast走n步指向第n个结点；接着，fast和slow一起走，直到fast指向尾结点；最后，删除元素（这里需要注意的是加入pre来判断删除元素是否为头结点）。

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
    ListNode *removeNthFromEnd(ListNode *head, int n) {
        if(head == NULL)
            return NULL;
        
        ListNode *slow = head;
        ListNode *fast = head;
        ListNode *pre = NULL;
        //fast先走n步，到达第n个结点
        while(--n)
            fast = fast->next;
        //fast和slow一起走，直到fast走到链表尾部
        while(fast->next != NULL){
            pre = slow;
            slow = slow->next;
            fast = fast->next;
        }
        //此处用于判断删除的结点是否为头结点
        if(pre != NULL)
            pre->next = slow->next;
        else
            head = head->next;
        
        return head;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**