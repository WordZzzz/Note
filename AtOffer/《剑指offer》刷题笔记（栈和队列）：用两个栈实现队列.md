# 《剑指offer》刷题笔记（栈和队列）：用两个栈实现队列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

栈是一个非常常见的数据结构，它在计算机领域被广泛应用，比如操作系统会给每个线程创建一个栈用来存储函数调用时各个函数的参数、返回地址及临时变量等。栈的特点是后进先出，即最后被压入（push）栈的元素会第一个被弹出（pop）。通常栈是一个不考虑排序的数据结构，所以我们需要O(n)时间才能找到栈中最大或者最小的元素。如果想要在O(1)时间内得到栈的最大或者最小值，我们需要对栈进行特殊的设计，以后会讲到。

队列是另外一种很重要的数据结构。与栈不同，队列的特点是先进先出。栈和队列虽然是特点针锋相对的两个数据结构，但有意思的是它们却相互联系，比如用两个栈实现队列，当然也可以用两个队列实现栈。

## 题目描述：
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

## 解题思路：
栈：后进先出（LIFO）;
队列：先进先出（FIFO）.
入队：将元素进栈A
出队：判断栈B是否为空，如果为空，则将栈A中所有元素pop，并push进栈B，栈B出栈；
 如果不为空，栈B直接出栈


## C++版代码实现：

```c
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        int tmp;
        if(stack2.empty())
            {
            while(!stack1.empty())
                {
                tmp = stack1.top();
                stack2.push(tmp);
                stack1.pop();
            }
        }
        tmp = stack2.top();
        stack2.pop();
        return tmp;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

## Python版代码实现：

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []
    def push(self, node):
        # write code here
        self.stack1.append(node)
    def pop(self):
        # return xx
        if self.stack2 == []:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**
