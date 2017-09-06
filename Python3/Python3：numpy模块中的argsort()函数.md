**<font color="black" size=6 face="仿宋">Python3：numpy模块中的argsort()函数</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

&emsp;&emsp;argsort函数是Numpy模块中的函数：

```python
>>> import numpy
>>> help(numpy.argsort)
Help on function argsort in module numpy.core.fromnumeric:


argsort(a, axis=-1, kind='quicksort', order=None)
Returns the indices that would sort an array.

Perform an indirect sort along the given axis using the algorithm specified
by the `kind` keyword. It returns an array of indices of the same shape as
`a` that index data along the given axis in sorted order.
```

&emsp;&emsp;从中可以看出argsort函数返回的是数组值从小到大的索引值

Examples：

One dimensional array:一维数组
```python
>>> x = np.array([3, 1, 2])
>>> np.argsort(x)
array([1, 2, 0])
```
Two-dimensional array:二维数组
```python
>>> x = np.array([[0, 3], [2, 2]])
>>> x
array([[0, 3],
[2, 2]])

>>> np.argsort(x, axis=0) #按列排序
array([[0, 1],
[1, 0]])

>>> np.argsort(x, axis=1) #按行排序
array([[0, 1],
[0, 1]])
```

Examples：

```python
>>> x = np.array([3, 1, 2])
>>> np.argsort(x) #按升序排列
array([1, 2, 0])
>>> np.argsort(-x) #按降序排列
array([0, 2, 1])

>>> x[np.argsort(x)] #通过索引值排序后的数组
array([1, 2, 3])
>>> x[np.argsort(-x)]
array([3, 2, 1])
```

另一种方式实现按降序排序：

```python
>>> a = x[np.argsort(x)]
>>> a
array([1, 2, 3])
>>> a[::-1]
array([3, 2, 1]) 
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**