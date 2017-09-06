**<font color="black" size=6 face="仿宋">Python3：字典中的items()函数</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

**<font color="black" size=5 face="仿宋">一、Python2.x中items()：</font>**

&emsp;&emsp;和之前一样，本渣渣先贴出来python中help的帮助信息：

```python
>>> help(dict.items)
Help on method_descriptor:

items(...)
    D.items() -> list of D's (key, value) pairs, as 2-tuples
```

```python
>>> help(dict.iteritems)
Help on method_descriptor:

iteritems(...)
    D.iteritems() -> an iterator over the (key, value) items of D
```

```python
>>> help(dict.viewitems)
Help on method_descriptor:

viewitems(...)
    D.viewitems() -> a set-like object providing a view on D's items
```

&emsp;&emsp;在Python2.x中，items( )用于 返回一个字典的拷贝列表【Returns a copy of the list of all items (key/value pairs) in D】，占额外的内存。

&emsp;&emsp;iteritems() 用于返回本身字典列表操作后的迭代【Returns an iterator on all items(key/value pairs) in D】，不占用额外的内存。

```python
>>> d={1:'one',2:'two',3:'three'}
>>> type(d.items())
<type 'list'>
>>> type(d.iteritems())
<type 'dictionary-itemiterator'>
>>> type(d.viewitems())
<type 'dict_items'>
```

**<font color="black" size=5 face="仿宋">二、Python3.x中items()：</font>**

```python
>>> help(dict.items)
Help on method_descriptor:

items(...)
    D.items() -> a set-like object providing a view on D's items
```

&emsp;&emsp;Python 3.x 里面，iteritems() 和 viewitems() 这两个方法都已经废除了，而 items() 得到的结果是和 2.x 里面 viewitems() 一致的。在3.x 里 用 items()替换iteritems() ，可以用于 for 来循环遍历。

```python
>>> d={1:'one',2:'two',3:'three'}
>>> type(d.items())
<class 'dict_items'>
```
简单的例子：

```python
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59, 'Paul': 74 }
sum = 0
for key, value in d.items():
	sum = sum + value
	print(key, ':' ,value)
print('平均分为:' ,sum /len(d))
```

输出结果：
```python
D:\Users\WordZzzz\Desktop>python3 test.py
Adam : 95
Lisa : 85
Bart : 59
Paul : 74
平均分为: 78.25
```

关于python3.x中items具体的应用，可以通过下面的传送门到达机器学习实战中找到：


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**