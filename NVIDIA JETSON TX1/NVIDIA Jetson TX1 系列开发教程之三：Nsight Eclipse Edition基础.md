**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之三：Nsight Eclipse Edition基础</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

&#160;&#160;&#160;&#160;Nsight Eclipse Edition是专为NVIDIA定制的Eclipse开发环境，主要区别是在菜单中加入了CUDA工程的建立、CUDAToolKit和NVIDIA的NVCC编译器等开发工具，方便开发者开发基于CUDA强大计算能力的各种工程项目。
&#160;&#160;&#160;&#160;根据上个教程安装完JetPcak3.0的各个功能模块之后，就可以在桌面的搜索框找出Nsight Eclipse Edition这个软件：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603170714026?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

&#160;&#160;&#160;&#160;本次教程以CUDA自带例程oceanFFT为例，介绍Nsight Eclips Edition的简单使用。

1.双击Nsight软件，弹出对话框，选择工作空间，即工作文件存放路径。确定后点击OK：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222057914?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

2.File->New->CUDA C/C++ Project，新建CUDA工程：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222149954?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

3.Project name：命名工程名称，Project type：提取CUDA例程，选定后点击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222210845?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

4.选取CUDA例程，这里我们选择oceanFFT，选定后点击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222405743?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

5.基础设置默认即可，当然，对于TX1的PTX和GPU，标准的选择还是5.3，而不是2.0，选定后点击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222424384?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

6.选择链接，点击Management，创建新的链接，链接内容如第7步所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222443915?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

7.输入IP和用户名，通过SSH登陆，填完后点击Finish：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222504197?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

8.选择Project Path、Toolkit和CPU类型，选定后点击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222524428?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

9.编译版本默认即可，选定后点击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222544772?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

10.工程文件：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222611007?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

11.Ctrl+B,构建：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603222627492?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

12.在TX1上找到相应目录，运行相应二进制文件，运行效果如下：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170603224024916?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

&#160;&#160;&#160;&#160;下篇博文将以JetPack3.0为例，向大家介绍Nsight进阶开发。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**