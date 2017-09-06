**<font color="black" size=6 face="仿宋">Python3：input()函数</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

**<font color="black" size=5 face="仿宋">一、Python2.x中raw_input( )和input( )函数</font>**

&emsp;&emsp;老规矩，本渣渣先贴出help信息，再进行讲解。

&emsp;&emsp;在Python2.x中raw_input( )和input( )，两个函数都存在，其中区别为

```python
>>> help(raw_input)
Help on built-in function raw_input in module __builtin__:

raw_input(...)
    raw_input([prompt]) -> string

    Read a string from standard input.  The trailing newline is stripped.
    If the user hits EOF (Unix: Ctl-D, Windows: Ctl-Z+Return), raise EOFError.
    On Unix, GNU readline is used if enabled.  The prompt string, if given,
    is printed without a trailing newline before reading.
```

raw_input( )---将所有输入作为字符串看待，返回字符串类型

```python
>>> help(input)
Help on built-in function input in module __builtin__:

input(...)
    input([prompt]) -> value

    Equivalent to eval(raw_input(prompt)).
```

input( )-----只能接收“数字”的输入，在对待纯数字输入时具有自己的特性，它返回所输入的数字的类型（ int, float ）

example：

```python
>>> user=raw_input("please input:")         
please input:wei               #  raw_input输入  字符串  成功  
>>> user  
'wei'  
>>> user=input("please input:")            
please input:123               #  input 输入  数字  成功（返回的是数字）  
>>> user  
123  
>>> user=raw_input("please input:")  
please input:111           #  raw_input 输入  数字  成功（返回的还是当成字符串）  
>>> user  
'111'  
>>> user=input("please input:")  
please input:wei                          #  input  输入字符串   失败  
Traceback (most recent call last):  
  File "<stdin>", line 1, in ?  
  File "<string>", line 0, in ?  
NameError: name 'wei' is not defined 
```

**<font color="black" size=5 face="仿宋">二、python3.x中的input( )函数</font>**

&emsp;&emsp;在python3.x中raw_input( )和input( )进行了整合，去除了raw_input( )，仅保留了input( )函数，其接收任意任性输入，将所有输入默认为字符串处理，并返回字符串类型。

```python
>>> help(input)
Help on built-in function input in module builtins:

input(prompt=None, /)
    Read a string from standard input.  The trailing newline is stripped.

    The prompt string, if given, is printed to standard output without a
    trailing newline before reading input.

    If the user hits EOF (*nix: Ctrl-D, Windows: Ctrl-Z+Return), raise EOFError.
    On *nix systems, readline is used if available.
```

example：

```
>>> user=raw_input("please input:")                 #没有了raw_input  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
NameError: name 'raw_input' is not defined  
>>> user=input("please input:")  
please input:wei  
>>> user  
'wei'  
>>> user=input("please input:")                     #input的输出结果都是作为字符串  
please input:123  
>>> user  
'123'
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**