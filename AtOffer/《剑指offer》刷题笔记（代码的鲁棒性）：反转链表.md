# 《剑指offer》刷题笔记（代码的鲁棒性）：反转链表

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入一个链表，反转链表后，输出链表的所有元素。

## 解题思路

题目比较简单，主要是需要考虑在调整某一结点的m_pNext之前，先把它的下一个节点保存下来，以免断链子了。同时还要满足下面的测试用例。

测试用例：

- 输入的链表头指针是NULL。
- 输入的链表只有一个结点。
- 输入的链表有多个结点。

## C++版代码实现

```c
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
    ListNode* pReversedHead = nullptr;
    ListNode* pNode = pHead;
    ListNode* pPrev = nullptr;
    while(pNode != nullptr)
    {
        ListNode* pNext = pNode->next;

        if(pNext == nullptr)
            pReversedHead = pNode;

        pNode->next = pPrev;

        pPrev = pNode;
        pNode = pNext;
    }

    return pReversedHead;
}
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**