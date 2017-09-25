# 《剑指offer》刷题笔记（字符串）：替换空格

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

C/C++中每个字符串都以字符‘\0’作为结尾，这样我们就能很方便地找到字符串的最后尾部。但是由于这个特点，每个字符串中都有一个额外字符的开销，稍不留神就会造成字符串的越界。

为了节省内存，C/C++把常量字符串放到单独的一个内存区域。当几个指针赋值给相同的常量字符串时，他们实际上会指向相同的内存地址，在下面的代码中，str1 == str2成立，因为它们指向同一地址。

```c
char* str1 = "hello world";
char* str2 = "hello world";
```

但用常量内存初始化数组，情况就不同了，在下面的代码中，str3 == str4不成立，因为这是两个字符串数组，会为它们分配两个长度为12个字节的空间，他们的初始地址是不同的，所以str3和str4的值也不相同。

```c
char str3[] = "hello world";
char str4[] = "hello world";
```

## 题目描述

请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

## 解题思路

### 时间复杂度为O(n^2)的解法

最直观的做法是从头到尾扫描字符串，每一个碰到空格字符的时候做替换。由于是把1个字符替换成3个字符，我们必须要把空格后面所有的字符都后移两个字节，否则就有两个字符被覆盖。

假设字符串的长度是n。对每个空格字符，需要移动后面O(n)个字符，因此对含有O(n)个空格字符的字符串而言总的时间效率是O(n^2)。

### 时间复杂度为O(n)的解法

先遍历一遍字符串，统计处空格的总数，由此计算出替换之后的字符串的总长度。然后用两个指针从字符串的后面开始复制和替换。P1指向原始字符串的末尾，而P2指向替换之后的字符串的末尾，然后向前移动P1，逐个把它指向的字符复制到P2指向的位置，直到碰到第一个空格位置。碰到第一个空格之后，把P1向前移动一格，P2之前插入字符串“%20”，同时把P2向前移动三格。具体如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170925111500819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>


由于所有的字符都只复制一次，因此时间效率为O(n)。

## C++代码实现

```c
class Solution {
public:
	void replaceSpace(char *str,int length) {
		if(str == NULL || length <= 0)
            return;
        
        int originalLength = 0;
        int numberOfBlank = 0;
        int i = 0;
        while(str[i] != '\0'){
            ++ originalLength;
            if(str[i] == ' ')
                ++ numberOfBlank;
            ++ i;
        }
        
        int newLength = originalLength + numberOfBlank * 2;
        if(newLength >= length){
            return;
        }
        
        int indexOfOriginal = originalLength;
        int indexOfNew = newLength;
        
        while(indexOfOriginal >= 0 && indexOfNew > indexOfOriginal){
            if(str[indexOfOriginal] == ' '){
                str[indexOfNew--] = '0';
                str[indexOfNew--] = '2';
                str[indexOfNew--] = '%';
            }
            else{
                str[indexOfNew--] = str[indexOfOriginal];
            }
            -- indexOfOriginal;
        }
	}
};
```

## Python代码实现

### 手动替换

```python
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        res = ''
        for ele in s:
            if ele.strip():
                res += ele
            else:
                res += '%20'
        return res
```

### 调用replace

```python
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        return s.replace(" ", "%20")
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**