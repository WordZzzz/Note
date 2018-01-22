---
title: LeetCode刷题笔记（链表）：add-two-numbers
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

You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

## 解题思路

两个链表求和，需要考虑进位问题。

首先新建链表，用于存储求和之后的结点；循环的终止条件是，直到l1或l2都为空且进位标志位为0；循环内，每次都进行求和运算和更新head的值。

在求carry的时候我们采用判断而不是除法，这一点大家需要注意，并且在以后的程序设计中也要尽量较少乘除法这种比较耗时的运算操作。

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
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
        if(l1 == NULL || l2 == NULL)
            return (l1 == NULL) ? l2 : l1;
        
        int carry = 0;                      //进位
        int sum;                            //求和
        ListNode *dummy = new ListNode (0); //新建链表
        ListNode *head = dummy;
        while(l1 || l2 || carry){
            sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry;
            ListNode *cur = new ListNode(sum % 10);
            carry = (sum >= 10) ? 1 : 0;
            head->next = cur;
            head = cur;
            
            if(l1) l1 = l1->next;
            if(l2) l2 = l2->next;
        }
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**