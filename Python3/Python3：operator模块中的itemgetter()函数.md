**<font color="black" size=6 face="仿宋">Python3：operator模块中的itemgetter()函数</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

&emsp;&emsp;和之前一样，本渣渣先贴出来python中help的帮助信息：

```python
>>> import operator
>>> help(operator.itemgetter)
Help on class itemgetter in module operator:

class itemgetter(builtins.object)
 |  itemgetter(item, ...) --> itemgetter object
 |
 |  Return a callable object that fetches the given item(s) from its operand.
 |  After f = itemgetter(2), the call f(r) returns r[2].
 |  After g = itemgetter(2, 5, 3), the call g(r) returns (r[2], r[5], r[3])
 |
 |  Methods defined here:
 |
 |  __call__(self, /, *args, **kwargs)
 |      Call self as a function.
 |
 |  __getattribute__(self, name, /)
 |      Return getattr(self, name).
 |
 |  __new__(*args, **kwargs) from builtins.type
 |      Create and return a new object.  See help(type) for accurate signature.
 |
 |  __reduce__(...)
 |      Return state information for pickling
 |
 |  __repr__(self, /)
 |      Return repr(self).
```

&emsp;&emsp;operator.itemgetter函数
operator模块提供的itemgetter函数用于获取对象的哪些维的数据，参数为一些序号（即需要获取的数据在对象中的序号），下面看例子。

```python
a = [1,2,3] 
>>> b=operator.itemgetter(1)      //定义函数b，获取对象的第1个域的值
>>> b(a) 
2 
>>> b=operator.itemgetter(1,0)   //定义函数b，获取对象的第1个域和第0个的值
>>> b(a) 
(2, 1) 
```

&emsp;&emsp;要注意，operator.itemgetter函数获取的不是值，而是定义了一个函数，通过该函数作用到对象上才能获取值。


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**