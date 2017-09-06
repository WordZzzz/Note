
**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之一：开箱测试</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">1. 概述</font>**

&#160;&#160;&#160;&#160;NVIDIA的Jetson TX1是嵌入式系统级模块（SoM），具有四核ARM Cortex-A57，4GB LPDDR4和集成的256核Maxwell GPU。 

&#160;&#160;&#160;&#160;对于部署计算机视觉和深度学习而言非常有用，Jetson TX1运行Linux(定制Ubuntu)，并提供1TFLOPS的FP16计算性能，功耗为10瓦。

&#160;&#160;&#160;&#160;Jetson TX1可作为模块，开发套件和兼容的生态系统产品。更多详细信息可以登录[NVIDIA官网（此处有链接）](http://www.nvidia.cn/object/embedded-systems-dev-kits-modules-cn.html)进行了解，也可通过[Wikipedia（此处有链接）](http://elinux.org/Jetson_TX1)进行TX1模组参数的快速预览。

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507174659130?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" />
</div>
<p></p>

----------

**<font color="black" size=5 face="仿宋">2. 基本参数</font>**

**处理组件：**

- 四核ARM Cortex-A57
- 256核Maxwell GPU
- 4GB LPDDR4
- 16GB eMMC
- H.264 / H.265编码器和解码器
- 双ISP（图像服务处理器）

**端口和外设：**

- HDMI 2.0
- 802.11ac WiFi，蓝牙4.0
- USB3，USB2
- 千兆以太网
- 12路MIPI CSI 2.0
- 4车道PCIe gen 2.0
- SATA，2x SD卡
- 3x UART，3x SPI，4x I2C

**构成因素：**

- 400针Samtec板对板连接器
- 尺寸：50x87mm  （1.96“x 3.42”）
- 质量：45克
- 热转印板（TTP），-25C至85C工作温度
- 5.5-19.6VDC输入功率（消耗10-15W，典型负载下）

**软件支持：**

- JetPack 2.3
- Linux4Tegra R24.2 (L4T) for ARM (Ubuntu 16.04 aarch64)
- CUDA Toolkit 8
- cuDNN v5.1
- TensorRT 1.0
- VisionWorks 1.5
- OpenCV4Tegra 2.4.13
- OpenGL 4.4
- OpenGL ES 3.1
- Vulkan
- V4L2 media controller support
- gstreamer / OpenMAX
- Tegra System Profiler (TSP)
- Tegra Graphics Debugger
- PerfKit 4.5.1


**<font color="black" size=5 face="仿宋">3. 开箱测试</font>**

&#160;&#160;&#160;&#160;小五千大洋买回来的宝贝，拿到手还有点小激动（图片来自我的partner：Jack Cui）：

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507192357194?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" />
</div>
<p></p>

&#160;&#160;&#160;&#160;包装很高大上，下面来介绍一下开发套件的内容：TX1开发套件（含TX1模组）、一个19V电源适配器（尴尬的是没有插座线）、两个天线（用于TX1模组的WiFi和蓝牙）、Micro USB线和USB-OTG线。

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507192426987?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" />
</div>
<p></p>

&#160;&#160;&#160;&#160;光有上面这些开发套件包含的东西是不能正常使用的，你还需要下面这些设备：

- 电源适配器插头：

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507192456925?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" />
</div>
<p></p>

- 2K显示屏：（开发套件原装系统默认分辨率为2K）。

&#160;&#160;&#160;&#160;这里有点尴尬，除非你能保证你可以在没有显示器的情况下就能输入命令行修改分辨率（想办法调出命令行然后输入：xrandr -s 1208x720），否则还是找一块2K显示屏来吧。

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507175904716?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400" height="400" />
</div>
<p></p>

- 鼠键套装：

<p></p>
<div align=center>
<img src="http://img.blog.csdn.net/20170507180214767?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" />
</div>
<p></p>

**<font color="black" size=5 face="仿宋">4.调出桌面：</font>**

```
cd ~/NVIDIA-INSTALL
sudo ./install.sh
sudo reboot now
```
&#160;&#160;&#160;&#160;重启电脑之后，我们就可以看到ubuntu系统的桌面了。Jetson TX1出厂时默认的系统以及各依赖工具版本比较老旧，所以我们需要进行刷机来部署最新版本的JetPack。

&#160;&#160;&#160;&#160;下篇博文将以JetPack3.0为例，向大家介绍如何进行刷机。
&#160;&#160;&#160;&#160;下周日更新。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**