# Python3：string中的split()函数

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## Python2.x中split()函数：

&emsp;&emsp;老规矩，help！

```python
>>> help(str.split)
Help on method_descriptor:

split(...)
    S.split([sep [,maxsplit]]) -> list of strings

    Return a list of the words in the string S, using sep as the
    delimiter string.  If maxsplit is given, at most maxsplit
    splits are done. If sep is not specified or is None, any
    whitespace string is a separator and empty strings are removed
    from the result.
```

&emsp;&emsp;可以看出，在Python2.x中，split()返回的是字符串列表。

## Python3.x中split()函数：

&emsp;&emsp;同样，help！

```python
>>> help(str.split)
Help on method_descriptor:

split(...)
    S.split(sep=None, maxsplit=-1) -> list of strings

    Return a list of the words in S, using sep as the
    delimiter string.  If maxsplit is given, at most maxsplit
    splits are done. If sep is not specified or is None, any
    whitespace string is a separator and empty strings are
    removed from the result.
```

&emsp;&emsp;啊哦，在Python3.x中，split()返回的也是字符串列表。

## 样例：

```python
>>> str = "this is string example....wow!!!"
>>> print (str.split( ))
['this', 'is', 'string', 'example....wow!!!']
>>> print (str.split('i',1))
['th', 's is string example....wow!!!']
>>> print (str.split('w'))
['this is string example....', 'o', '!!!']
```

加入正则表达式：

```python
>>> mySent='This book is the best book on Python or M.L. I have ever laid eyes upon'
>>> mySent.split()                                      #不带参数切分
['This', 'book', 'is', 'the', 'best', 'book', 'on', 'Python', 'or', 'M.L.', 'I', 'have', 'ever', 'laid', 'eyes', 'upon']
>>> import re
>>> regEx = re.compile('\\W')                               #正则表达式，定义分隔符是除单词、数字外的任意字符串
>>> listOfTokens = regEx.split(mySent)                      #根据正则表达式的规则进行切分
>>> listOfTokens
['This', 'book', 'is', 'the', 'best', 'book', 'on', 'Python', 'or', 'M', 'L', '', 'I', 'have', 'ever', 'laid', 'eyes', 'upon']
>>> [tok for tok in listOfTokens if len(tok) > 0]           #去掉空格
['This', 'book', 'is', 'the', 'best', 'book', 'on', 'Python', 'or', 'M', 'L', 'I', 'have', 'ever', 'laid', 'eyes', 'upon']
>>> [tok.lower() for tok in listOfTokens if len(tok) > 2]   #全部小写，去掉长度小于3的单词
['this', 'book', 'the', 'best', 'book', 'python', 'have', 'ever', 'laid', 'eyes', 'upon']
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**