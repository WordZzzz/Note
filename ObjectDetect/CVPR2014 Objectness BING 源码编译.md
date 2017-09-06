**<font color="black" size=6 face="仿宋">CVPR2014 Objectness BING 源码编译</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：VS2013+OpenCV2.4.13**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">一、资源</font>**

1.论文作者主页：http://mmcheng.net/zh/bing/
2.代码下载地址：http://mmcheng.net/zh/code-data/
3.数据集下载地址：http://mmcheng.net/zh/bing/

**<font color="black" size=5 face="仿宋">二、环境配置</font>**

&emsp;&emsp;如果你用的是Visual Studio 2012，正好电脑也支持X64平台，那么你只需要配置一下VS2012下的OpenCV（版本要求2.4.8以上），下载的程序就可以直接用了。
&emsp;&emsp;我做了一些排雷的动作，尝试了Visual Studio 2013中Release、Debug中的x64和win32版本，即：Release+x64、Debug+x64、Release+win32、Debug+win32.但是因为本渣渣能力有限，最终没能把Visual Studio 2013中的Debug+win32版本跑出来。

1.解压下载的源码：
&emsp;&emsp;解压后文件夹内容如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903110934676?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

2.解压VOC2007数据集：
&emsp;&emsp;在上图的源码文件中我们也可以看到有个VOC2007文件夹，里面只有ImageSets一个文件夹（里面是训练时会用到的文本文档）。在这一步，我们要把下载好的800多MB的数据集解压到源码的VOC2007文件夹下。（注意在解压过程中当出现是否覆盖的选项时，一律选择跳过,虽然覆盖了也不是很影响效果。）

3.用VS2013打开解决方案，提示升级VC++编译器和库，直接点击确定就好：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111124791?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

4.X64->WIN32:

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111218240?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111248523?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111311467?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

添加win32配置管理器，注意一定要从x64继承下来其他东西。

5.配置LibLinear：
- 右键LibLinear，选为启动项目；
- 在linear.cpp文件中，修改print_string_stdout函数为：extern "C" static void print_string_stdout(const char *s)
- 静态库配置：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111729176?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

6.LibLinear代码生成：
&emsp;&emsp;最重要的就是这一块的东西了，我直接把四种配置的截图贴在这里，注意，MT对应Release，MTD对应Debug，但是作者的Debug版本用的是DLL(/MDd)，所以本渣渣在Debug版本中也没做更改：

Release+x64：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111535986?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

Debug+x64：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111413318?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

Release+win32：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111515044?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

Debug+win32（失败）：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903111445504?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>


&emsp;&emsp;ctrl+F5之后在相应目录下生成LibLinear.lib静态库，记下这个地址。
7.配置Objectness：
&emsp;&emsp;以Debug+win32的配置为例进行介绍（虽然配置失败，但是这些属性设置是通用的，所以没有更新截图）

- 右键Objectness，选为启动项目；
- 打开Debug属性，链接器->附加库目录，添加LibLinear.lib的目录；（注意:链接库依赖项 要设为是（yes））

- 用_popcnt函数实现_popcnt64函数功能：需要自己动手在INT64类型基础上写函数。要加头文件#include<intrin.h>在stdafx.h中。
```css
inline INT64 __popcnt64(INT64 x)
{
       return __popcnt((unsigned int)(x )) +__popcnt((unsigned int)(x>> 32));
}
```

8.配置opencv：
&emsp;&emsp;这里大家可以参考浅墨的博客进行配置，当然本渣渣要是有时间了也会写一篇博客并在此更新链接。浅墨opencv配置链接：
http://blog.csdn.net/poem_qianmo/article/details/19809337

&emsp;&emsp;这里我只贴出一些需要填写路径的截图：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903150731185?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903150751508?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903150810256?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903150831273?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903150858382?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;**<font color="red" size=3 face="仿宋">需要注意的是上图部分，Debug版本就用d结尾的库，Release版本就用不带d结尾的库，千万不要像浅墨那样两个版本都塞进去，否则会莫名其妙报错。</font>**

9.再次配置Objectness的代码生成，和第6步是一样的。

&emsp;&emsp;ctrl+F5之后，运行成功。

10.效果展示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903151152107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" /></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170903151223762?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

11.其他：
&emsp;&emsp;如果想优化代码，可以打开openmp，并且设置一下优化等级，本渣渣在这里只是为了看处理效果，所以就没做优化的配置。关于优化配置可参考链接（有些情况不一定好使）：http://www.cnblogs.com/larch18/p/4560690.html

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**