# 《剑指offer》刷题笔记（举例让抽象具体化）：包含min函数的栈

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

定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

## 解题思路

我们很自然的可以想到，可以利用两个栈来实现该操作：一个栈sData用来存放数据，另一个栈sMin用来辅助更新最小值状态。

栈内压入3、4、2、1之后接连两次弹出栈顶数字之后再压入0时，数据栈、辅助栈和最小值状态举例如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171021102238518?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171021102303045?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++版代码实现

```c
class Solution {
public:
    void push(int value) {
        sData.push(value);
        if(sMin.empty())
            sMin.push(value);
        if(sMin.top() > value)
            sMin.push(value);
    }
    void pop() {
        if(sData.top() == sMin.top())
            sMin.pop();
        sData.pop();
    }
    int top() {
        return sData.top();
    }
    int min() {
        return sMin.top();
    }
private:
    stack<int> sData;
    stack<int> sMin;
};
```

## Python版代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.sData = []
        self.sMin = []
    def push(self, node):
        # write code here
        self.sData.append(node)
        if len(self.sMin) == 0:
            self.sMin.append(node)
        if self.sMin[-1] > node:
            self.sMin.append(node)
    def pop(self):
        # write code here
        if self.sData[-1] == self.sMin[-1]:
            self.sMin.pop()
        self.sData.pop()
    def top(self):
        # write code here
        return self.sData[-1]
    def min(self):
        # write code here
        return self.sMin[-1]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**