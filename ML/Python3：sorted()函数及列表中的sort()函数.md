**<font color="black" size=6 face="仿宋">Python3：sorted()函数及列表中的sort()函数</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

**<font color="black" size=5 face="仿宋">一、sort，sorted函数介绍：</font>**

&emsp;&emsp;Sort函数是list列表中的函数，而sorted可以对list或者iterator进行排序。

&emsp;&emsp;下面我们使用help来查看他们的用法及功能：
sort：

```python
>>> help(list.sort)
Help on method_descriptor:

sort(...)
    L.sort(key=None, reverse=False) -> None -- stable sort *IN PLACE*
```

sorted：
Python3.x:
```python
>>> help(sorted)
Help on built-in function sorted in module builtins:

sorted(iterable, /, *, key=None, reverse=False)
    Return a new list containing all items from the iterable in ascending order.

    A custom key function can be supplied to customize the sort order, and the
    reverse flag can be set to request the result in descending order.

```

Python2.x:

```python
>>> help(sorted)
Help on built-in function sorted in module __builtin__:

sorted(...)
    sorted(iterable, cmp=None, key=None, reverse=False) --> new sorted list
```

&emsp;&emsp;好吧，Python3.x和Python2.x的sorted函数有点不太一样，少了cmp参数。下面本渣渣主要基于Python2.x的sorted函数进行讲解，Python3.x直接忽略cmp这个参数即可，为了保证代码通用性，不建议大家在今后的编程中使用cmp参数。

**<font color="black" size=5 face="仿宋">二、sort和sorted的比较：</font>**

&emsp;&emsp;用sort函数对列表排序时会影响列表本身，而sorted不会。
举例：

```python
>>> a = [1,2,1,4,3,5]
>>> a.sort()
>>> a
[1, 1, 2, 3, 4, 5]
>>> a = [1,2,1,4,3,5]
>>> sorted(a)
[1, 1, 2, 3, 4, 5]
>>> a
[1, 2, 1, 4, 3, 5]
```

&emsp;&emsp;Python2.x的sorted函数：sorted(iterable，cmp，key，reverse）
参数：
- iterable可以是list或者iterator；
- cmp是带两个参数的比较函数；
- key 是带一个参数的函数；
- reverse为False或者True；

举例说明:
（1）用cmp函数排序:

```python
>>> list1 = [('david', 90), ('mary',90), ('sara',80),('lily',95)]
>>> sorted(list1,cmp = lambda x,y: cmp(x[0],y[0]))
[('david', 90), ('lily', 95), ('mary', 90), ('sara', 80)]
>>> sorted(list1,cmp = lambda x,y: cmp(x[1],y[1]))
[('sara', 80), ('david', 90), ('mary', 90), ('lily', 95)]
```

（2）用key函数排序:

```python
>>> list1 = [('david', 90), ('mary',90), ('sara',80),('lily',95)]
>>> sorted(list1,key = lambda list1: list1[0])
[('david', 90), ('lily', 95), ('mary', 90), ('sara', 80)]
>>> sorted(list1,key = lambda list1: list1[1])
[('sara', 80), ('david', 90), ('mary', 90), ('lily', 95)]
```

（3）用reverse排序:

```python
>>> sorted(list1,reverse = True)
[('sara', 80), ('mary', 90), ('lily', 95), ('david', 90)]
```

（4）用operator.itemgetter函数排序:

```python
>>> from operator import itemgetter
>>> sorted(list1, key=itemgetter(1))
[('sara', 80), ('david', 90), ('mary', 90), ('lily', 95)]
>>> sorted(list1, key=itemgetter(0))
[('david', 90), ('lily', 95), ('mary', 90), ('sara', 80)]
```

&emsp;&emsp;介绍operator.itemgetter函数:

```python
>>> import operator
>>> a = [1,2,3]
>>> b = operator.itemgetter(0)
>>> b(a)
1
```

&emsp;&emsp;operator.itemgetter函数获取的不是值，而是定义了一个函数。

（5）多级排序:

```python
>>> sorted(list1, key=itemgetter(0,1))
[('david', 90), ('lily', 95), ('mary', 90), ('sara', 80)]
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**