# 《剑指offer》刷题笔记（举例让抽象具体化）：栈的压入、弹出序列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言：

当一眼看不出问题中隐藏的规律时，我们可以试着用一两个具体的例子模拟操作的过程，说不定这样那就能通过具体的例子找到抽象的规律。

## 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## 解题思路

借用一个辅助的栈，遍历压栈顺序，先讲第一个放入栈中，这里是1，然后判断栈顶元素是不是出栈顺序的第一个元素，这里是4，很显然1≠4，所以我们继续压栈，直到相等以后开始出栈，出栈一个元素，则将出栈顺序向后移动一位，直到不相等，这样循环等压栈顺序遍历完成，如果辅助栈还不为空，说明弹出序列不是该栈的弹出顺序。

下面给出例子中的示意图：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171025224243366?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171025224302956?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++版代码实现

```c
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.size() == 0)
            return false;
        vector<int> stackData;
        for(int i = 0, j = 0; i < pushV.size();){
            stackData.push_back(pushV[i++]);
            while(j < popV.size() && stackData.back() == popV[j]){
                stackData.pop_back();
                j++;
            }
        }
        return stackData.empty();
    }
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def IsPopOrder(self, pushV, popV):
        # write code here
        if len(pushV) == 0:
            return False
        stackData = []
        for i in pushV:
            stackData.append(i)
            while len(stackData) and stackData[-1] == popV[0]:
                stackData.pop()
                popV.pop(0)
        if len(stackData):
            return False
        return True
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**