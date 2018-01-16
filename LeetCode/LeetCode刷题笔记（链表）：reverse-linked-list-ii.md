---
title: LeetCode刷题笔记（链表）：reverse-linked-list-ii
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

Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given1->2->3->4->5->NULL, m = 2 and n = 4,

return1->4->3->2->5->NULL.

Note: 
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

## 解题思路

可能是因为反转链表i太简单了，所以牛客网只有ii么，应该是这样的，哈哈哈。

头指针是必不可少的，因为有可能会要求全部反转。首先定位到需要反转的第一个元素，然后每次都将它后面的元素往它前面放。他们之间的关系大家最好自己画图捋一遍，这样记得比较清楚。

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
    ListNode *reverseBetween(ListNode *head, int m, int n) {
        if(head == NULL)
            return NULL;
        
        int diff = n - m;
        
        ListNode *preHead = new ListNode(0);
        preHead->next = head;
        ListNode *preCur = preHead;
        ListNode *cur = head;
        
        for(int i = 1; i < m; ++i){
            preCur = cur;
            cur = cur->next;
        }
        
        for(int i = 0; i < diff; ++i){
            ListNode *tmp = cur->next;
            cur->next = tmp->next;
            tmp->next = preCur->next;
            preCur->next = tmp;
        }
        return preHead->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**