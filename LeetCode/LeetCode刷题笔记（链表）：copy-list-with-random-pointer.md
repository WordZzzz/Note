---
title: LeetCode刷题笔记（链表）：copy-list-with-random-pointer
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

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

## 解题思路

我们可以将复杂链表的复制过程分解为三个步骤。在写代码的时候我们每一步定义一个函数，这样每个函数完成一个功能，整个过程的逻辑也就非常清晰明了了。

大部分人首先想到的可能是先复制复杂指针的label和next，然后再查找random并更新。查找random又分为两种，一种是每次都从头查找，时间复杂度为O(n^2)；另一种是空间换时间，复制label和next的同时建立一个hash表来存放新旧复杂指针的对应关系，所以后续只需一步就能找到random，算法时间复杂度为O(n)。

我们这里采用三步走战略，也是剑指offer上推崇的方法：

- 第一步：复制复杂指针的label和next。但是这次我们把复制的结点跟在元结点后面，而不是直接创建新的链表；
- 第二步：设置复制出来的结点的random。因为新旧结点是前后对应关系，所以也是一步就能找到random；
- 第三步：拆分链表。奇数是原链表，偶数是复制的链表。

有图思路更清晰：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171108132929521?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171108132947451?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171108133004592?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

这道题，我就不照搬之前的博客了，直接贴一种更简洁的代码。

## C++版代码实现

```c
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        RandomListNode *copy, *p;
        if(head == NULL)
            return NULL;
        
        //复制链表
        for(p = head; p; p = p->next){
            copy = new RandomListNode(p->label);
            copy->next = p->next;
            p = p->next = copy;
        }
        
        //复制random
        for(p = head; p; p = copy->next){
            copy = p->next;
            copy->random = (p->random ? p->random->next:NULL);
        }
        
        //split链表
        for(p = head, head = copy = p->next; p;){
            p = p->next = copy->next;
            copy = copy->next = (p ? p->next:NULL);
        }
        return head;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**