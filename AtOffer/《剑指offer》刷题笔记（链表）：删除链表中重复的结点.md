# 《剑指offer》刷题笔记（链表）：删除链表中重复的结点

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

## 解题思路

删除重复结点，只需要记录当前结点前的最晚访问过的不重复结点pPre、当前结点pCur、指向当前结点后面的结点pNext的三个指针即可。如果当前节点和它后面的几个结点数值相同，那么这些结点都要被剔除，然后更新pPre和pCur；如果不相同，则直接更新pPre和pCur。

需要考虑的是，如果第一个结点是重复结点我们该怎么办？这里我们分别处理一下就好，如果第一个结点是重复结点，那么久把头指针也更新一下。

## C++版代码实现

```c
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(pHead == NULL)
            return NULL;
        ListNode* pPre = NULL;  //指向当前结点前最晚访问过的不重复结点
        ListNode* pCur = pHead; //指向当前处理的结点
        ListNode* pNext = NULL; //指向当前结点后面的结点
        
        while(pCur != NULL){
            //如果当前结点与下一结点值相同，则进一步处理
            if(pCur->next != NULL && pCur->next->val == pCur->val){
                pNext = pCur->next;
                
                //找到pNext，它指向最后一个与pCur的val相同的结点，那pCur到pNext之间的结点都是要删除的
                while(pNext->next != NULL && pNext->next->val == pCur->val)
                    pNext = pNext->next;
                
                //如果pCur指向链表中第一个元素，pCur -> ... -> pNext ->... , 要删除pCur到pNext, 将指向链表第一个元素的指针pHead指向pNext->next。
                if(pCur == pHead)
                    pHead = pNext->next;
                //如果pCur不指向链表中第一个元素，pPre -> pCur ->...->pNext ->... ，要删除pCur到pNext，即pPre->next = pNext->next
                else
                    pPre->next = pNext->next;
                //当前处理的p要向链表尾部移动
                pCur = pNext->next;
            }
            //如果当前结点与下一结点值不同，更新pPre和pCur。
            else{
                pPre = pCur;
                pCur = pCur->next;
            }
        }
        return pHead;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**