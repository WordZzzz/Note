# 《剑指offer》刷题笔记（综合）：把字符串转换成整数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

### 输入描述:

输入一个字符串,包括数字字母符号,可以为空

### 输出描述:

如果是合法的数值表达则返回该数字，否则返回0

## 示例1

输入

```
+2147483647
    1a33
```

输出

```
2147483647
    0
```

## 解题思路

题目虽然简单，但是要写出完整的代码也是需要经过全面的思考才可以。

给出一个题目，我们首先要考虑的就是边界问题。对于这个题目，边界问题有：

- 空指针；
- 空字符串""；
- 带有正负号；
- 只有正负号；
- 上下溢出；
- 错误标志输出。

代码中用两个函数来实现该功能，其中标志位g_nStatus用来表示是否为异常输出，标志位用来表示是否为负数。需要注意的也就只有上面提到的边界问题。具体实现如下。

## C++版代码实现

```c
class Solution {
public:
    enum Status {kValid = 0, kInvalid};
    int g_nStatus = kValid;
    
    int StrToInt(string str) {
        g_nStatus = kInvalid;
        long long num = 0;
        const char* cstr = str.c_str();
        //判断是否为空指针和是否为空字符串
        if(cstr != NULL && *cstr != '\0'){
            bool minus = false;
            //正负号区分
            if(*cstr == '+')
                cstr++;
            else if(*cstr == '-'){
                cstr++;
                minus = true;
            }
            //如果不是只有正负号，就进入下一个函数
            if(*cstr != '\0')
                num = StrToIntCore(cstr, minus);
        }
        return (int)num;
    }
    
    long long StrToIntCore(const char* cstr, bool minus){
        long long num = 0;
        while(*cstr != '\0'){
            //判断是否为非法值
            if(*cstr >= '0' && *cstr <= '9'){
                int flag = minus ? -1 : 1;
                num = num * 10 + flag * (*cstr - '0');
                //判断是否溢出
                if((!minus && num > 0x7fffffff) || (minus && num < (signed int)0x80000000)){
                    num = 0;
                    break;
                }
                cstr++;
            }
            else{
                num = 0;
                break;
            }
        }
        #判断是否正常结束
        if(*cstr == '\0')
            g_nStatus = kValid;
        return num;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**