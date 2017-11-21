# 《剑指offer》刷题笔记（时间效率）：数组中出现次数超过一半的数字

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

## 解题思路

思路一：结合快排。数组排序后，如果符合条件的数存在，则一定是数组中间那个数。（比如：1，2，2，2，3；或2，2，2，3，4；或2，3，4，4，4等等）
这种方法虽然容易理解，但由于涉及到快排sort，其时间复杂度为O(NlogN)并非最优；

思路二：书上的解法，时间复杂度为O(N)。如果有符合条件的数字，则它出现的次数比其他所有数字出现的次数和还要多。其实和第一个思路差不多，都是利用次数。
在遍历数组时保存两个值：一是数组中一个数字，一是次数。遍历下一个数字时，若它与之前保存的数字相同，则次数加1，否则次数减1；若次数为0，则保存下一个数字，并将次数置为1。遍历结束后，所保存的数字即为所求。然后再判断它是否符合条件即可。


## C++版代码实现

### 思路一

```c
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        if(numbers.empty()) return 0;
         
        sort(numbers.begin(), numbers.end()); // 排序，取数组中间那个数
        int middle = numbers[numbers.size() / 2];
         
        int count=0; // 出现次数
        for(int i=0; i < numbers.size(); ++i)
        {
            if(numbers[i] == middle) ++count;
        }
         
        return (count > numbers.size()/2) ? middle :  0;
    }
};
```

### 思路二

```c
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        if(numbers.empty()) 
            return 0;
         
        // 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
        int result = numbers[0];
        int times = 1; // 次数
         
        for(int i=1; i < numbers.size(); ++i)
        {
            if(times == 0)
            {
                // 更新result的值为当前元素，并置次数为1
                result = numbers[i];
                times = 1;
            }
            else if(numbers[i] == result)
            {
                ++times; // 相同则加1
            }
            else
            {
                --times; // 不同则减1               
            }
        }
         
        // 判断result是否符合条件，即出现次数大于数组长度的一半
        times = 0;
        for(int i=0; i < numbers.size(); ++i)
        {
            if(numbers[i] == result) 
            ++times;
        }
         
        return (times > numbers.size()/2) ? result : 0;
    }
};
```

## Python版代码实现

### 思路一

```python
import collections
class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        tmp = collections.Counter(numbers)
        x = len(numbers)/2
        for k, v in tmp.items():
            if v > x:
                return k
        return 0
```

### 思路二

```python
# -*- coding:utf-8 -*-
class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        if len(numbers) == 0:
            return 0
        # 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
        result = numbers[0]
        times = 1
        
        for i in range(len(numbers)):
            if times == 0:
                # 更新result的值为当前元素，并置次数为1
                result = numbers[i]
                times = 1
            elif numbers[i] == result:
                times += 1 #相同则加1
            else:
                times -= 1 #不同则减1 
        
        # 判断result是否符合条件，即出现次数大于数组长度的一半
        times = 0
        for i in range(len(numbers)):
            if numbers[i] == result:
                times += 1
        if times > len(numbers) /2:
            return result
        else:
            return 0
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**