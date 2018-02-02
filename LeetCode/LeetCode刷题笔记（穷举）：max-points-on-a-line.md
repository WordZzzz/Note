---
title: LeetCode刷题笔记（穷举）：max-points-on-a-line
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Exhaustive
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

Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

给定2D平面上的n个点，找出位于同一直线上的最大点数。

## 解题思路

两点确定一条直线，同时，对于斜率不为零的直线，都有y=kx+b。对于重复点，肯定也算是共线，所以，我们首先计算重复点的个数，然后计算斜率为0时共线的点个数，最后再计算其他斜率下共线的点的个数。

需要两重循环，第一重循环遍历起始点a，第二重循环遍历剩余点b。
 
- 如果a和b重合，duplicate累加；
- 如果a和b不重合，且两者斜率为0，则numVertical累加；
- 如果a和b不重合，且两者斜率不为零，则计算斜率并计入map中，同时更新map中对应值；
- 内循环结束后，返回max(local + duplicate, numVertical + duplicate)与之前的最大值res的最大值，local和numVertical算是不同斜率下的最大值；
- 外循环结束后，返回res。

## C++版代码实现

```c
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    int maxPoints(vector<Point>& points) {
        if(points.size() <= 2)
            return points.size();
        int res = 0;
        for(int i = 0; i < points.size() - 1; ++i){
            unordered_map<double, int> map;
            int numVertical = 0, local = 1, duplicate = 0;
            for(int j = i + 1; j < points.size(); ++j)
                if(points[i].x == points[j].x)
                    if(points[i].y == points[j].y)  //重复点
                        duplicate++;
                    else                            //垂直点
                        numVertical == 0 ? numVertical = 2 : numVertical++;
                else {
                    double slope = (points[i].y - points[j].y) * 1.0 / (points[i].x - points[j].x);
                    map[slope] == 0 ? map[slope] = 2 : map[slope]++;
                    local = max(local, map[slope]);
                }
            local = max(local + duplicate, numVertical + duplicate);
            res = max(res, local);
        }
        return res;
    }
};
```


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**