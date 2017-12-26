# 《剑指offer》刷题笔记（回溯法）：机器人的运动范围

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

## 解题思路


和上一道题十分相似，只不过这次的限制条件变成了坐标位数之和。对于求坐标位数之和，我们单独用一个函数实现，然后套入上一道题的代码中即可。


## C++版代码实现

```c
class Solution {
public:
    int movingCount(int threshold, int rows, int cols)
    {
        bool* visited = new bool[rows * cols];
        for(int i = 0; i < rows * cols; i++)
            visited[i] = false;
        
        int count = movingCountCore(threshold, rows, cols, 0, 0, visited);
        delete[] visited;
        
        return count;
    }
    
    int movingCountCore(int threshold, int rows, int cols, int row, int col, bool* visited){
        int count = 0;
        if(row >= 0 && row < rows && col >= 0 && col < cols && getDigitSum(row) + getDigitSum(col) <= threshold && !visited[row * cols + col]){
            visited[row * cols + col] = true;
            
            count = 1 + movingCountCore(threshold, rows, cols, row+1, col, visited)
                + movingCountCore(threshold, rows, cols, row-1, col, visited)
                + movingCountCore(threshold, rows, cols, row, col+1, visited)
                + movingCountCore(threshold, rows, cols, row, col-1, visited);
        }
        return count;
    }
    
    int getDigitSum(int number){
        int sum = 0;
        while(number){
            sum += number % 10;
            number /= 10;
        }
        return sum;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**