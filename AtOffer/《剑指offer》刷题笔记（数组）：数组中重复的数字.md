# 《剑指offer》刷题笔记（数组）：数组中重复的数字

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

## 解题思路

如果先排序，再找出重复的数字是一件很容易的事情，只需要从头到尾遍历排序后的数组就可以了。排序一个长度为n的数组需要O(nlogn)的时间。

还可以利用hash表来解决这个问题，从头到尾按顺序扫描数组中的每个数，每扫描到一个数字的时候，都可以用O(1)的时间来判断哈希表里是否已经包含了该数字。如果哈希表里还没有这个数字，就把它加入到哈希表中；如果已经存在，就说明找到了一个重复的数字。算法复杂度为O(n)，但是是以一个大小为O(n)的哈希表为代价的。


还可以把当前序列当成是一个下标和下标对应值是相同的数组（时间复杂度为O(n),空间复杂度为O(1)）；
遍历数组，判断当前位的值和下标是否相等： 
1. 若相等，则遍历下一位； 
2. 若不等，则将当前位置i上的元素和a[i]位置上的元素比较：若它们相等，则成功！若不等，则将它们两交换。换完之后a[i]位置上的值和它的下标是对应的，但i位置上的元素和下标并不一定对应；重复2的操作，直到当前位置i的值也为i，将i向后移一位，再重复2.

本文复现的是最后一种方法。

## C++版代码实现

```c
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        //非法输入1
        if(numbers == NULL || length <= 0)
            return false;
        //非法输入2
        for(int i=0; i < length; ++i){
            if(numbers[i] < 0 || numbers[i] > length-1)
                return false;
        }
        //遍历查找第一个重复的数字
        for(int i=0; i < length; ++i){
            while(numbers[i] != i){
                if(numbers[i] == numbers[numbers[i]]){
                    *duplication = numbers[i];
                    return true;
                }
                //不相等就交换
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
            }
        }
        return false;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**