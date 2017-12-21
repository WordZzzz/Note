# 《剑指offer》刷题笔记（链表）：链表中环的入口结点

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

一个链表中包含环，请找出该链表的环的入口结点。

## 解题思路

我们可以用两个指针来解决此类问题。如果链表中的环有n个结点，指针P1先在链表上向前移动n步，然后两个指针以相同的速度向前移动。当第二个指针指向环的入口结点时，第一个指针已经环绕着环走了一圈又回到了入口结点。如下图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171221160256337?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

那我们接下来要考虑的就是如何获取这个环的结点个数n了。我们可以用快慢指针来实现，如果两个指针相遇，表明链表存在环。两个指针相遇的结点一定是在环中。可以从这个结点出发，一边继续向前移动一边计数，当再次回到这个结点时，就可以得到环中结点数了。

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
    ListNode* MeetingNode(ListNode* pHead){
        if(pHead == NULL)
            return NULL;
        
        ListNode* pSlow = pHead->next;
        if(pSlow == NULL)
            return NULL;
        
        ListNode* pFast = pSlow->next;
        while(pFast != NULL && pSlow != NULL){
            if(pFast == pSlow)
                return pFast;
            
            pSlow = pSlow->next;
            
            pFast = pFast->next;
            if(pFast->next != NULL)
                pFast = pFast->next;
        }
        return NULL;
    }
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        ListNode* meetingNode = MeetingNode(pHead);
        if(meetingNode == NULL)
            return NULL;
        
        // 得到环中结点的数目
        int nodesInLoop = 1;
        ListNode* pNode1 = meetingNode;
        while(pNode1->next != meetingNode){
            pNode1 = pNode1->next;
            ++nodesInLoop;
        }
        
        // 先移动pNode1，次数为环中结点的数目
        pNode1 = pHead;
        for(int i=0; i < nodesInLoop; ++i)
            pNode1 = pNode1->next;
        
        // 再移动pNode1和pNode2
        ListNode* pNode2 = pHead;
        while(pNode1 != pNode2){
            pNode1 = pNode1->next;
            pNode2 = pNode2->next;
        }
        return pNode1;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**