---
title: LeetCode刷题笔记（链表）：reverse-nodes-in-k-group
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

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,
Given this linked list:1->2->3->4->5；

For k = 2, you should return:2->1->4->3->5

For k = 3, you should return:3->2->1->4->5

## 解题思路

解法有两种，一种是申请额外空间来依次对k个元素进行排序，时间复杂度O(n)，空间复杂度O(k)。另一种是在原链表中进行反转，不需要额外空间，所以算法复杂度为O(n)，空间复杂度为O(1)。

本文中对第二种解法进行详细的讲解。

- 首先，单个链表的反转我想大家肯定都会，那就是遍历链表，每遇见一个结点就扔到第一位。因为涉及到第一个结点的操作，所以我们一定要有指向第一个结点的指针，这里在传参的时候需要注意。
- 接下来，我们采用整除来每次提取k个结点。需要注意的同样是参数的传递，怎么对接自己得把握好。就拿例题中的题目（list:1->2->3->4->5；k=3）来说，我的代码传入reverseList的参数应该是指向1的上一个结点的指针和指向4的指针。
- 最后，贴代码呗。

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
    
    ListNode *reverseList(ListNode *slow, ListNode *fast){
        ListNode *last = slow->next;
        ListNode *cur = last->next;
        while(cur != fast){
            last->next = cur->next;
            cur->next = slow->next;
            slow->next = cur;
            cur = last->next;
        }
        return last;
    }
    
    ListNode *reverseKGroup(ListNode *head, int k) {
        if(head == NULL || k == 1)
            return head;
        
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *slow = dummy;
        ListNode *fast = head;
        int i = 0;
        while(fast != NULL){
            i++;
            if(i % k == 0){
                slow = reverseList(slow, fast->next);
                fast = slow->next;
            }
            else
                fast = fast->next;
        }
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**