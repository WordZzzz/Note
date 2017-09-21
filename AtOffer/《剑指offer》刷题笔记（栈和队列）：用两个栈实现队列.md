# 《剑指offer》刷题笔记（栈和队列）：用两个栈实现队列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

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
