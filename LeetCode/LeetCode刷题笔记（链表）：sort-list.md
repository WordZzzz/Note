---
title: LeetCode刷题笔记（链表）：sort-list
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

Sort a linked list in O(n log n) time using constant space complexity.

## 解题思路

因为题目要求复杂度为O(nlogn),故可以考虑归并排序的思想。

前一段时间刚刚总结了八大排序，感兴趣的同学们可以由此穿越：[数据结构与算法系列之一：八大排序之归并排序](https://wordzzzz.github.io/2018/01/07/DS/)。

递归版归并排序的一般步骤为：

- 将待排序数组（链表）取中点并一分为二；
- 递归地对左半部分进行归并排序；
- 递归地对右半部分进行归并排序；
- 将两个半部分进行合并（merge）,得到结果。

所以对应此题目，可以划分为三个小问题：

- 找到链表中点 （快慢指针思路，快指针一次走两步，慢指针一次走一步，快指针在链表末尾时，慢指针恰好在链表中点）；
- 写出merge函数，即如何合并链表。 
- 写出mergesort函数，实现上述步骤。


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
    
    //快慢指针获取链表中点
    ListNode *getMiddle(ListNode *head){
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast->next != NULL && fast->next->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
    
    //合并链表
    ListNode *mergeList(ListNode *left, ListNode *right){
        if(left == NULL)
            return right;
        if(right == NULL)
            return left;
        
        ListNode *tmp = new ListNode(0);
        ListNode *head = tmp;
        
        while(left != NULL && right != NULL){
            if(left->val > right->val){
                head->next = right;
                right = right->next;
            }
            else{
                head->next = left;
                left = left->next;
            }
            head = head->next;
        }
        if(left == NULL)
            head->next = right;
        if(right == NULL)
            head->next = left;
        return tmp->next;
    }
    
    //排序函数
    ListNode *sortList(ListNode *head) {
        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode *middle = getMiddle(head);
        ListNode *right = sortList(middle->next);
        middle->next = NULL;        //左链表的结尾不能再指向右链表的开头
        ListNode *left = sortList(head);
        return mergeList(left, right);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**