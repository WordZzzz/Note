**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之八：Qt安装与配置</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">写在前面：</font>**
很多朋友私下问我如何在NVIDIA TX1上安装和配置Qt，所以我就自己试着配置了一把，现在贴出了分享给大家。这篇文章是关于安装QT 5.5.1支持的Qt Creator 3.3.1 版本。

**<font color="black" size=5 face="仿宋">一、安装Qt</font>**

1.安装Qt Creator，打开终端执行如下命令：
```
$ sudo apt-get install qt5-default qtcreator -y
```
2.安装Qt示例和文档：
```
$ sudo apt-get install qt5-doc qt5-doc-html qtbase5-doc-html qtbase5-examples -y
```

**<font color="black" size=5 face="仿宋">二、配置Qt</font>**

1.搜索Qt并打开应用程序，当然也可以用命令行直接打开：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730161851878?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>

2.Tools->Options->Build & Run->Compilers，单击add选择添加GCC编译器，GCC编译器默认路径为/usr/bin/gcc，添加完路径之后还要修改开发平台，如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730161841734?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>

3.切换到Kit下，添加开发套件。名称可以随便写，需要注意的是必须先配置上一步的GCC，这一步才能直接添加GCC，否则就会像Desktop套件一样报错：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730162006680?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>

**<font color="black" size=5 face="仿宋">三、测试Qt示例</font>**

1.打开示例，随便选一个都可以，在这里我选择的是看起来比较酷炫的图标排列例程：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730162312438?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="500" /></div>
<p></p>

2.工程路径和打开例程方式，这里我们默认即可：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730162347984?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="500" /></div>
<p></p>

3.F5构建项目，然后运行结果如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170730162440533?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="800" /></div>
<p></p>

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**


