# 《剑指offer》刷题笔记（知识迁移能力）：和为S的连续正数序列

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

## 输出描述

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

## 解题思路

延续我们上一道题的思路，可以先给定两个数（phigh = 2,plow = 1），然后开始遍历求和，如果curSum大了，plow加1，如果curSum小了，phigh就加1；每次遇到curSum和sum相等了，就将当前plow和phigh之间的数据全部压入临时tmp中，最后再将tmp压入最终结果result。

## C++版代码实现

```
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > result;
        int phigh = 2,plow = 1;
         
        while(phigh > plow){
            int curSum = (phigh + plow) * (phigh - plow + 1) / 2;
            if( curSum < sum)
                phigh++;
             
            if( curSum == sum){
                vector<int> res;
                for(int i = plow; i <= phigh; i++)
                    res.push_back(i);
                result.push_back(res);
                plow++;
            }
             
            if(curSum > sum)
                plow++;
        }
         
        return result;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**