# 《剑指offer》刷题笔记（链表）：从尾到头打印链表

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

链表应该是面试被提及最频繁的数据结构。链表的创建、插入节点、删除结点等操作都只需要20行左右的代码就能实现，其代码量比较适合面试。

在创建链表时，无需知道链表的长度，当插入一个结点时，我们只需要为新结点分配内存，然后调整指针的指向来确保新节点被链接到链表当中。内存分配不是在创建链表时一次性完成，而是没添加一个结点分配一次内存。由于没有闲置的内存，链表的空间效率比数组好。

## 题目描述

输入一个链表，从尾到头打印链表每个节点的值。

## 解题思路

### 直接修改输入数据

如果可以修改原来链表的结构，那么把链表中链接结点的指针反转过来，改变链表的方向，然后就可以从头到尾输出了。

但是，打印通常是一个只读操作，我们不希望打印时修改内容，所以就得想别的办法。

### 循环

后进先出，我们可以用栈实现这种顺序。没经过一个结点的时候，把该节点放到一个栈里面，当遍历完整个链表后，再从栈顶开始逐个输出结点的值，此时输出的结点的顺序已经反转过来了。

### 递归

递归在本质上就是一个栈结构，于是很自然地又想到了用递归来实现。每访问到一个结点的时候，先递归输出它后面的节点，再输出该节点自身，这样链表的输出结果就反过来了。

## C++代码实现

### 循环

```c
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> result;
        stack<int> nodes;
        
        ListNode* pNode = head;
        while(pNode != NULL){
            nodes.push(pNode->val);
            pNode = pNode->next;
        }
        while(!nodes.empty()){
            result.push_back(nodes.top());
            nodes.pop();
        }
        return result;
    }
};
```

### 递归

```
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> value;
        if(head != NULL)
        {
            value.insert(value.begin(),head->val);
            if(head->next != NULL)
            {
                vector<int> tempVec = printListFromTailToHead(head->next);
                if(tempVec.size()>0)
                value.insert(value.begin(),tempVec.begin(),tempVec.end());  
            }         
             
        }
        return value;
    }
};
```

## Python代码实现

python的实现依旧是那么简单，一直往列表头放数据就好。

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        l = []
        head = listNode
        while head:
            l.insert(0, head.val)
            head = head.next
        return l
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**