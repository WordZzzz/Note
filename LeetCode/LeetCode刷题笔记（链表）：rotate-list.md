---
title: LeetCode刷题笔记（链表）：rotate-list
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

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:
Given1->2->3->4->5->NULLand k =2,
return4->5->1->2->3->NULL.

## 解题思路

- 遍历链表得到链表长度并定位fast到尾结点；
- 根据链表长度，slow找到新的尾结点，slow的下一个结点便是新的头结点因为k可能比len大，所以需要取余数；
- 原先的尾结点与原先的头节点相连，dummy的next指针更新指向新的头结点，同时断开新的尾节点与新的头结点之间的联系。

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
    ListNode *rotateRight(ListNode *head, int k) {
        if(k == 0 || head == NULL || head->next == NULL)
            return head;
        
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *fast = head;
        ListNode *slow = head;
        int len;
        //计算链表长度，同时fast指向尾结点
        for(len = 1; fast->next != NULL; ++len)
            fast = fast->next;
        //根据得到的链表长度，找到需要切开的结点
        for(int i = 1; i < len - k % len; ++i)
            slow = slow->next;
        fast->next = dummy->next;   //尾结点连接到头节点，形成环状链表
        dummy->next = slow->next;   //fast的下一个结点作为新的头结点
        slow->next = NULL;          //slow结点和新的头结点断开
        
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**