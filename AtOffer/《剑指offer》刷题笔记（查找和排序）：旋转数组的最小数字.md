# 《剑指offer》刷题笔记（查找和排序）：旋转数组的最小数字

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

查找和排序是面试时考查算法的重点，重点掌握二分查找、归并排序和快速排序。

查找相对而言较为简单，不外乎顺序查找、二分查找、哈希表查找和二叉排序树查找。必须要信手拈来。

哈希表表和二叉排序树查找的重点在于考察对应的数据结构而不是算法。哈希表最主要的优点是我们利用它能够在O(1)时间查找某一元素，是效率最高的查找方式，但是需要二外的空间来实现哈希表。与二叉排序树查找算法对应的数据结构是二叉搜索树，后面会涉及到。如果要在排序的数组（或者部分排序的数组）中查找一个数字或者统计某个数字出现的次数，我们都可以尝试用二分查找算法。

排序比查找要复杂一些，我们需要比较插入排序、冒泡排序、归并排序、快速排序等不同算法的优劣，必须要烂熟于胸，能够从额外空间消耗、平均时间复杂度和最差时间复杂度等方面去比较他们的优缺点。同时，快排经常要求重写。

## 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## 解题思路

### 直接查找

算法复杂度O(n)。

### 再次排序

再次排序后输出第一个数字，算法复杂度O(n*logn)。

### 分段二分查找

算法复杂度O(logn)。

- 我们用两个指针分别指向数组的第一个元素和最后一个元素。按照题目中旋转的规则，第一个元素应该是大于或者等于最后一个元素的（不完全对，有特例）。
- 接着我们可以找到数组中间的元素。如果中间元素位于前面的递增子数组，那么它应该大于或者等于第一个指针指向的元素。此时最小元素应该位于该中间元素之后，然后我们把第一个指针指向该中间元素，移动之后第一个指针仍然位于前面的递增子数组中。
- 同样，如果中间元素位于后面的递增子数组，那么它应该小于或者等于第二个指针指向的元素。此时最小元素应该位于该中间元素之前，然后我们把第二个指针指向该中间元素，移动之后第二个指针仍然位于后面的递增子数组中。
- 第一个指针总是指向前面递增数组的元素，第二个指针总是指向后面递增数组的元素。最终它们会指向两个相邻的元素，而第二个指针指向的刚好是最小的元素，这就是循环结束的条件。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170929105837807?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

特殊情况：
- 如果把排序数组的0个元素搬到最后面，这仍然是租住的一个需安装，我们的代码需要支持这种情况。如果发现数组中的一个数字小于最后一个数字，就可以直接返回第一个数字了。
- 下面这种情况，即第一个指针指向的数字、第二个指针指向的数字和中间的数字三者相等，我们无法判断中间的数字1是数以前面的递增子数组还是后面的递增子数组。正样的话，我们只能进行顺序查找。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170929105856996?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## C++代码实现

### 再次排序

```c
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
       sort(rotateArray.begin(),rotateArray.end());
         
        return rotateArray[0];
         
    }
};
```

### 分段二分法

```c
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int size = rotateArray.size();
        if(size == 0){
            return 0;
        }
        int left = 0;
        int right = size - 1;
        int mid = 0;
        // rotateArray[left] >= rotateArray[right] 确保旋转
        while(rotateArray[left] >= rotateArray[right]){
            // 分界点
            if(right - left == 1){
                mid = right;
                break;
            }
            mid = left + (right - left) / 2;
            // rotateArray[left] rotateArray[right] rotateArray[mid]三者相等
            // 无法确定中间元素是属于前面还是后面的递增子数组
            // 只能顺序查找
            if(rotateArray[left] == rotateArray[right] && rotateArray[left] == rotateArray[mid]){
                return MinOrder(rotateArray,left,right);
            }
            // 中间元素位于前面的递增子数组
            // 此时最小元素位于中间元素的后面
            if(rotateArray[mid] >= rotateArray[left]){
                left = mid;
            }
            // 中间元素位于后面的递增子数组
            // 此时最小元素位于中间元素的前面
            else{
                right = mid;
            }
        }
        return rotateArray[mid];
    }
private:
    // 顺序寻找最小值
    int MinOrder(vector<int> &num,int left,int right){
        int result = num[left];
        for(int i = left + 1;i < right;++i){
            if(num[i] < result){
                result = num[i];
            }
        }
        return result;
    }
};
```

## Python代码实现

```python
# -*- coding:utf-8 -*-
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        if len(rotateArray) == 0:
            return 0
        pre = -7e20
        for num in rotateArray:
            if num < pre :
                return num
            pre = num
        return rotateArray[0]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**