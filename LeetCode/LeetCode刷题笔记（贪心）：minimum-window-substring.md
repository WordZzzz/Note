---
title: LeetCode刷题笔记（贪心）：minimum-window-substring
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Greedy
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,
S ="ADOBECODEBANC"
T ="ABC"

Minimum window is"BANC".

Note: 
If there is no such window in S that covers all characters in T, return the emtpy string"".

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

## 解题思路

这道题被自己的无知给坑了，在java里面传人substring的两个参数分别是截取字符串起始位置和截止位置，在本题中应该写成substring(head, head+d);在C++里面传人substr的两个参数分别是截取字符串起始位置和截取字符串长度，在本题中应该写成substr(head, d)。

这道题的思路是：
- begin开始指向0， end一直后移，直到begin - end区间包含T中所有字符。
记录窗口长度d
- 然后begin开始后移移除元素，直到移除的字符是T中的字符则停止，此时T中有一个字符没被
包含在窗口，
 继续后移end，直到T中的所有字符被包含在窗口，重新记录最小的窗口d。
- 如此循环知道end到S中的最后一个字符。
时间复杂度为O(n)

下面贴一下大神的模板，对于大多数子字符串问题，我们给了一个字符串，并且需要找到满足一些限制的子字符串。一般的方法是使用两个指针辅助的hashmap。模板如下。

```c
int findSubstring(string s){
        vector<int> map(128,0);
        int counter; // check whether the substring is valid
        int begin=0, end=0; //two pointers, one point to tail and one  head
        int d; //the length of substring

        for() { /* initialize the hash map here */ }

        while(end < s.size()){

            if(map[s[end++]]-- ?){  /* modify counter here */ }

            while(/* counter condition */){ 
                 
                 /* update d here if finding minimum*/

                //increase begin to make it invalid/valid again
                
                if(map[s[begin++]]++ ?){ /*modify counter here*/ }
            }  

            /* update d here if finding maximum*/
        }
        return d;
  }
```

有一点需要提到的是，当要求查找最大子字符串时，我们应该在内部while循环之后更新最大值，以保证子字符串是有效的。另一方面，当要求查找最小子字符串时，我们应该更新内部while循环内的最小值。

解决最多两个不同字符的最长子字符串的代码如下：

```c
int lengthOfLongestSubstringTwoDistinct(string s) {
        vector<int> map(128, 0);
        int counter=0, begin=0, end=0, d=0; 
        while(end < s.size()){
            if(map[s[end++]]++ == 0) counter++;
            while(counter > 2) if(map[s[begin++]]-- == 1) counter--;
            d = max(d, end - begin);
        }
        return d;
    }
```

解决最长子字符串不重复字符的代码如下：


```c
int lengthOfLongestSubstring(string s) {
        vector<int> map(128,0);
        int counter=0, begin=0, end=0, d=0; 
        while(end < s.size()){
            if(map[s[end++]]++ > 0) counter++; 
            while(counter>0) if(map[s[begin++]]-- > 1) counter--;
            d=max(d, end - begin); //while valid, update d
        }
        return d;
    }
```

## C++版代码实现

```c
class Solution {
public:
    string minWindow(string s, string t) {
        vector<int> map(128,0);
        for(auto c: t) map[c]++;
        int counter = t.size(), begin = 0, end = 0, d = INT_MAX, head = 0;
        while(end < s.size()){
            if(map[s[end++]]-- >0) counter--; //in t
            while(counter == 0){ //valid
                if(end - begin < d)  d = end - (head = begin);
                if(map[s[begin++]]++ == 0) counter++;  //make it invalid
            }  
        }
        return d == INT_MAX? "":s.substr(head, d);
    }
};
```

[参考链接：minimum-window-substring](https://leetcode.com/problems/minimum-window-substring/discuss/26808)

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**