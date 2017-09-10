# Python3：input()函数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## Python2.x中range()函数：

&emsp;&emsp;老规矩，help！

```python
>>> help(range)
Help on built-in function range in module __builtin__:

range(...)
    range(stop) -> list of integers
    range(start, stop[, step]) -> list of integers

    Return a list containing an arithmetic progression of integers.
    range(i, j) returns [i, i+1, i+2, ..., j-1]; start (!) defaults to 0.
    When step is given, it specifies the increment (or decrement).
    For example, range(4) returns [0, 1, 2, 3].  The end point is omitted!
    These are exactly the valid indices for a list of 4 elements.
```

&emsp;&emsp;可以看出，在Python2.x中，range()返回的是可以用来迭代的列表。

## Python3.x中range()函数：

&emsp;&emsp;同样，help！

```python
>>> help(range)
Help on class range in module builtins:

class range(object)
 |  range(stop) -> range object
 |  range(start, stop[, step]) -> range object
 |
 |  Return an object that produces a sequence of integers from start (inclusive)
 |  to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
 |  start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
 |  These are exactly the valid indices for a list of 4 elements.
 |  When step is given, it specifies the increment (or decrement).
```

&emsp;&emsp;啊哦，在Python3.x中，range()返回的是一个range对象。

## 样例对比：
Python2.x：

```python
>>> range(1,10)
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Python3.x：

```python
>>> range(1,10)
range(1, 10)
>>> list(range(1,10))
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

&emsp;&emsp;通过list强制类型转换，可以在Python3.x中实现Python2.x中的range函数一样的效果。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**