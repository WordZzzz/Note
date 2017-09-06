**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之二：刷机与开发前准备</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">1.  JetPack3.0下载与安装</font>**

本人PC端使用虚拟机安装Ubuntu14.04系统进行开发，需要有效联网。

1.首先在自己的home目录下创建Jetson文件夹：

```
        $ mkdir ~/Jetson
        $ cd ~/Jetson
```

2.下载可执行文件到新建的Jetson文件夹下：
https://developer.nvidia.com/embedded/downloads

3.在虚拟机上运行安装脚本：

```
        $ chmod +x  ./JetPack-L4T-<version>-linux-x64.run
        $ ./JetPack-L4T-<version>-linux-x64.run
```

之后出现如下图所示，单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514162457089?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

之后会显示安装路径，单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514163056458?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

4.接下来是开发板选型，这里我选TX1，单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514163208366?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

5.输入root用户密码：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514163407807?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

6.联网获取安装包信息，此处必须联网，否则出不来安装信息，选完安装信息之后单击Next（ps：一般默认即可，只安装Jetpack不刷机的话就把下面的Target-Jetson TX1的Action一栏全部改为no action；由于我的已经安装完毕，所以Action一栏都是no action）：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514164423772?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

7.接受所选组件的许可协议，全部同意并单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514164513150?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

8.报错的话就按提示安装依赖库后再进行Jetpack安装：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514165036562?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

9.安装完成后如下图所示，单机Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514165440464?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

10.点击Finish即可完成安装，最好不要勾选移除安装包：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514165617027?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

**<font color="black" size=5 face="仿宋">2.  刷机</font>**
&#160;&#160;&#160;&#160;刷机过程需要重复上述1-5步，并且在第6步中Target-Jetson TX1的Action一栏改为需要的各个软件安装版本。所以刷机从第6步开始介绍.
6.联网获取安装包信息，此处必须联网，否则出不来安装信息，选完安装信息之后单击Next（一般默认即可）：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514170240735?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

7.接受所选组件的许可协议，全部同意并单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514164513150?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>
8.该组件管理器将继续安装。一旦主机的安装步骤完成后，单击下一步按钮继续安装目标组件。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514165440464?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

9.a.如果在6中取消选择Flash OS的组件管理器（即不烧写系统，只烧写某些组件），将需要输入IP地址，用户名和密码来建立ssh连接到目标设备：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514171202482?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

9.b.如果在6中选择Flash OS的组件管理器，需要选择适合的特定环境中的网络布局：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514171121200?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>
10.a.如果选择了通过路由器/交换机设备访问互联网的布局，你会被要求选择哪个接口用于上网。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514171519491?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

10.b.如果您选择的设备通过主机IP获得通过指定DHCP服务器主机和访问互联网上的布局，您必须选择哪个接口是用于上网，并且将被用于目标接口。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514171554051?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

11.烧写确认，单击Next：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514173025649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

12.之后会弹出POST窗口来引导你开启USB强制恢复模式：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170521110312924?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

&#160;&#160;&#160;&#160;TX1与主机使用usb连接，开启USB强制恢复模式（关机情况下，按一下POWER键，再长按REC键的同时点按RESET键，两秒后松开REC键）此时虚拟机会弹出NVIDIA设备：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514172001604?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

可以lsusb确认一下：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514172036136?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="200" /></div>
<p></p>

13.在POST窗口中回车，开始安装：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514172117995?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

14.系统安装完成后板子会重启。如果TX1重启后出现了Ubuntu的GUI界面，说明系统已经安装完成。重启完之后POST窗口会提示你输入IP地址，这里如果输入正确，等待十分钟左右就会进入下一个界面；如果失败，最好查看一下TX1现在的IP。接下来就需要安装CUDA、OpenCV等组件。在按Enter继续安装之前，需要确保TX1已经连网外网，如果校园网需要登录网关这样的，先将网关登录好再继续，因为安装组件的时候，需要安装一些依赖库，需要有外网的情况下才能进行。按Enter继续后，会出现提示信息，确定TX1的IP地址，手动输入TX1的IP地址后，按回车继续，稍等一会儿，会出现如下对话框（和9a里的一样，如果执行的9a而不是9b，这里就不会再出现确认信息）：
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514171202482?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

15.一路NEXT，再次进入POST界面，此时使用SSH远程服务，无需下载线。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514172710005?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

16.安装完之后，程序自动关闭POST，回到如下界面，点击FINISH完成安装。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514172808920?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

**<font color="black" size=5 face="仿宋">3.  测试</font>**
Jetpack自动编译所有例程，CUDA例程可以在以下目录中找到：
```
<JetPack_Install_Dir> / NVIDIA_CUDA- <版本> _samples
```
可以通过重新编译运行示例：
```
SMS=53 EXTRA_LDFLAGS=--unresolved-symbols=ignore-in-shared-libs TARGET_ARCH=aarch64 make
```
运行：
CUDA的例程编译后的二进制文件目录：
```
/home/ubuntu/NVIDIA_CUDA-<version>_Samples/bin/aarch64/linux/release/
```
命令行运行或双击运行：
例如，当您运行oceanFFT样品，将显示如下画面。
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170514174244191?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>


**<font color="black" size=5 face="仿宋">4.  开发前准备工作</font>**

**更新源：**

&#160;&#160;&#160;&#160;因为默认源会找不到我们下述要用的一些依赖库和安装包，所以需要加入国内源，本例中采用的是中科大的源。值得注意的是，如果你之后回头来刷固件，一定还要把源换回来，否则有可能会安装失败。所以我们先做一下备份：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vim /etc/apt/sources.list
```
在sources.list中将被#注释掉的源全部打开，随后在底部添加新的源：

```
deb http://mirrors.ustc.edu.cn/ubuntu-ports trusty main universe restricted multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports trusty-security main universe restricted multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports trusty-updates main universe restricted multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports trusty main universe restricted multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports trusty-security main universe restricted multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports trusty-updates main universe restricted multiverse
```

保存文件后，进行更新：
```
sudo apt-get update
sudo apt-get upgrade
```

**浏览器安装：**

&#160;&#160;&#160;&#160;<font color="red">直接能上外网的请忽略此步骤。</font>

&#160;&#160;&#160;&#160;由于身在校内，且实验室网络连接都是交换机模式，每一台设备连接外网时都需要登陆，所以就涉及到浏览器的安装。当然，大家也可以直接用PYTHON写个登陆脚本也是可以的。

&#160;&#160;&#160;&#160;NVIDIA Jetson TX1自带系统没有浏览器是个很尴尬的事情。我们尝试了两款浏览器，分别是Epiphany和Firefox。前者相对稳定，但是用一段时间之后就再也不能跳转至校园网登陆界面；后者经常闪退，但是至少还给我输入账号密码的机会。最后我们选择的是Firefox。

&#160;&#160;&#160;&#160;安装浏览器也需要联网，so，还得先用WiFi连接一下个人热点。

安装Epiphany：

```
sudo apt-get install epiphany-browser
```

安装Firefox：

```
sudo apt-get install firefox-browser
```

**输入法安装：**

&#160;&#160;&#160;&#160;命令行操作一般不需要使用输入法，但是如果我们想在GUI上进行一些操作，比如上网查资料等，输入法还是得有的。

安装google输入法：

```
sudo apt-get install ibus-googlepinyin
sudo reboot now
```

&#160;&#160;&#160;&#160;安装完后重启系统，随后在GUI界面右上角找到文本输入设置(Text Entry Settings)，在里面将自己新下载的google输入法添加进去即可，操作和WINDOWS下添加输入法一样，故不在此赘述。

**lrzsz安装：**

&#160;&#160;&#160;&#160;NVIDIA Jetson TX1的SSH服务（端口号：22）已经配好，所以我们可以直接使用Xshell或者其他支持SSH远程登陆的客户端软件通过SSH服务来连接TX1，从而方便多人在WINDOWS上远程操控TX1。当然，Xshell是不支持图形界面的，如果想远程登录图形界面，可以配置一下vncserver，但是会消耗资源，个人不建议使用。

&#160;&#160;&#160;&#160;远程登录难免涉及到文件传输，文件传输有两种方法，大文件最好用FTP来传输，推荐使用File Zilla；小文件Xshell自己就可以搞定，需要在TX1上安装lrzsz。

安装lrzsz：

```
sudo apt-get install lrzsz
```

sz发送TX1文件到本地：

```
sz filename
```

rz发送本地文件到服务器：

```
rz
```

&#160;&#160;&#160;&#160;下篇博文将以JetPack3.0为例，向大家介绍如何使用Nsight进行程序开发。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**