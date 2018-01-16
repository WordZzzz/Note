---
title: LeetCode刷题笔记（链表）：partition-list
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

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given1->4->3->2->5->2and x = 3,
return1->2->2->4->3->5.

## 解题思路

牛客网上面的答案都是新建两个链表，小于x的放到一个链表里面，不小于的放到另一个链表里面，这种答案感觉好没劲哦。所以最后我采用的是O(n)时间复杂度，O(1)空间复杂度的解法。

具体说来，还是用快慢指针遍历链表，slow指向连续小于x的最后一个元素，fast指向当前元素不小于x但是下个元素小于x的元素。理解清楚这两个指针的对应关系之后，我们很容易将fast指向的元素的下一个元素追加到slow之后，同时更新slow和fast的指。

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
    ListNode *partition(ListNode *head, int x) {
        if(head == NULL)
            return NULL;
        
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *fast = dummy;
        ListNode *slow = dummy;
        while(fast != NULL && fast->next != NULL){
            if(fast->next->val >= x)
                fast = fast->next;
            else{
                if(fast == slow){
                    fast = fast->next;
                    slow = slow->next;
                }
                else{
                    ListNode *tmp = fast->next;
                    fast->next = tmp->next;
                    tmp->next = slow->next;
                    slow->next = tmp;
                    slow = slow->next;
                    
                }
            }
        }
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**