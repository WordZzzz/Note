# 《剑指offer》刷题笔记系列综述

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

&emsp;&emsp;WordZzzz原计划按照牛客网上面的《剑指offer》的通过率高低来把66道编程题刷一遍，但是后来发现，相对于牛客网上大神的解答，WordZzzz更倾向于原书作者的详解。所以，从今天开始，本博主打算按照《剑指offer》纪念版上的题目顺序来进行后续的总结，以作者何大大思路为主，以牛客网上各路大声的代码为辅，对每一道题的解法进行详细的记录，方便自己日后查看，也为订阅本专栏的各位可爱的亲人们提供一些参考。

- 《剑指offer》何大大的源码（含测试代码）传送门：https://github.com/zhedahht/CodingInterviewChinese2
- 当然我自己也fork了一下：https://github.com/WordZzzz/CodingInterviewChinese2
- 再来个码云的传送门吧：https://gitee.com/WordZzzz/CodingInterviewChinese2
- 至于《剑指offer》纪念版的电子版当然我也有，网上最清晰的资源，需要的请私信我。有条件的还是建议购买正版书籍，支持何大大，支持正版！

&emsp;&emsp;另外，下面这道开胃菜牛客上没有，所以先贴出来了。

## 赋值运算符函数

&emsp;&emsp;如下为类型CMyString的生命，请为该类型添加赋值运算符函数。

```c
class CMyString
{
public:
    CMyString(char* pData = NULL);
    CMyString(char* CMyString& str);
    ~CMyString(void);
    
private:
    char* m_pData;
};
```

```c
class CMyString
{
public:
    CMyString(char* pData = NULL);
    CMyString(char* CMyString& str);
    ~CMyString(void);
    
private:
    char* m_pData;
};
```

### 考察细节

- 是否会把返回值的类型声明为该类型的引用，并在函数结束前返回实例自身的引用（即*this）。只有返回一个引用，才可以允许连续赋值。否则如果函数的返回值是void，应用该赋值运算符将不能做连续赋值。假设有3个CMyString的对象：str1、str2和str3，在程序中语句str1=str2=str3是不能通过编译的。
- 是否把传入的参数的类型声明为常量引用。如果传入的参数不是引用而是实例，那么从形参到实参会调用一次复制构造函数，吧参数声明为引用可以避免这样的消耗，能提高代码的效率。同时，我们在赋值运算符函数中不会改变传入的实例的状态，因此应该为传入的引用参数加上const关键字。
- 是否释放实例自身已有的内存。如果我们忘记在分配新内存之前释放自身已有的空间，程序将出现内存泄漏。
- 是否判断传入的参数和但钱的实例是不是同一个实例。如果是同一个，则不进行赋值操作，直接返回。如果事先不判断就进行赋值，那么在释放自身的内存的时候就会导致严重的问题：当*this和传入的参数是同一个实例时，那么一旦释放了自身的内存，传入的参数的内存也同时被释放了，因此再也找不到需要赋值的内容了。

### 经典的解法

```c
CMyString& CMyString::operator = (const CMyString &str)
{
    if(this == &str)
        return *this;
    delete []m_pData;
    m_pData = NULL;
    m_pData = new char[strlen(str.m_pData) + 1];
    strcpy(m_pData, str.m_pData);
    
    return *this;
}
```

&emsp;&emsp;这是一般教材上提供的参考代码。明眼人可能会发现，上述代码在分配内存之前先用delete释放了实例m\_pData的内存。如果此时内存不足导致new char抛出异常，m\_pData将是一个空指针，这昂非常容易导致程序的崩溃。

### 考虑异常安全性的解法

&emsp;&emsp;要想在赋值运算符函数中实现异常安全性，一般有两种方法。一个简单的办法是我们先用new分配新内存再用delete释放已有的内容；一个更好的方法是创建临时实例，再交换临时实例和原来的实例。参考代码如下：

```cpp
CMyString& CMyString::operator = (const CMyString &str)
{
    if(this != &str)
    {
        CMyString strTemp(str);
        
        cahr* pTemp = strTemp.m_pData;
        strTemp.m_pData = m_pData;
        m_pData = pTemp;
    }
    
    return *this;
}
```

&emsp;&emsp;程序比较简单，值得注意的是，strTemp是一个局部变量，程序运行到if的外面的时候，也就出了该变量的作用域，就会自动调用其析构函数，把strTemp.m\_pData所指向的内存释放掉。由于strTemp.m\_pData指向的内存就是实例之前m_pData的内存，这就相当于自动调用析构函数释放实例的内存。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**