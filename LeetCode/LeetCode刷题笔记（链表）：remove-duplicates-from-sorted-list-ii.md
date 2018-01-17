---
title: LeetCode刷题笔记（链表）：remove-duplicates-from-sorted-list-ii
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

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given1->2->3->3->4->4->5, return1->2->5.
Given1->1->1->2->3, return2->3.

## 解题思路

因为第一个元素就有可能是重复节点，所以我们需要新建一个头节点指针，这一点在之前的几道题都有提到过。然后设置快慢指针，fast从head开始遍历，slow总是比fast慢一步；如果fast当前元素和它的下一个元素不相等，则更新slow、fast；否则，遍历重复元素，直到fast落到最后一个重复元素上，然后更新slow（跳过重复节点，直接指向fast的下一个节点）、fast。

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
    ListNode *deleteDuplicates(ListNode *head) {
        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *fast = head;
        ListNode *slow = dummy;
        while(fast != NULL && fast->next != NULL){
            if(fast->next->val != fast->val)
                //更新slow
                slow = fast;
            else{
                //遍历重复的元素，fast落到重复元素的最后一个元素上。
                while(fast->next && fast->next->val == fast->val)
                    fast = fast->next;
                //删掉slow和fast之间的元素
                slow->next = fast->next;
            }
            //更新fast
            fast = fast->next;
        }
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**