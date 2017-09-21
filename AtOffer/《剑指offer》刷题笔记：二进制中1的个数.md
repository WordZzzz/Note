# 《剑指offer》刷题笔记（位运算）：二进制中1的个数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述：
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

## 解题思路：
方法一：用1（循环左移）与n的每一位进行位与，来判断1的个数；注意，如果用n进行循环右移，一定要注意负数的情况，因为如果是负数，n右移之后高位补位为1，如果一开始不对n加以判断和处理，就会进入死循环。

方法二：目前我认为最优的解。n=(n-1) & n; 
举个栗子：一个二进制数n为1100，那么n-1为1011，1011&1100=1000，我们会发现，这样操作之后，相当于把n最右边的一个1变为0，那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

方法三：python的解法给了我另一种思路，为什么不用位数作为循环终止条件呢？

## C++版代码实现：

### 方法一：

```c
class Solution {
public:
     int  NumberOf1(int n) {
        int count = 0;
        int flag = 1;
        while (flag != 0) {
            if ((n & flag) != 0) {
                count++;
            }
            flag = flag << 1;
        }
        return count;
     }
};
```

### 方法二：

```c
class Solution {
public:
     int  NumberOf1(int n) {
        int count = 0;
        while (n != 0) {
            ++count;
            n = (n - 1) & n;
        }
        return count;
     }
};
```

## Python版代码实现：

### 方法三：

```python
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        return sum([(n>>i & 1) for i in range(0,32)])
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**