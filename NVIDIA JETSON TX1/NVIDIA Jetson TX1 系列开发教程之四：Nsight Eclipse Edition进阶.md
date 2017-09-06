**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之四：Nsight Eclipse Edition进阶</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

&#160;&#160;&#160;&#160;上一篇博文简单介绍了如何使用Nsight Eclipse Edition来导入CUDA例程并构建应用程序到NVIDIA Jetson TX1开发板上。本篇博文将继续介绍Nsight Eclipse Edition的进阶使用，通过OpenCV测试程序和GStreamer测试程序，分别介绍如何添加库链接和库路径到工程文件中，全是干货。友情提示，记得打开开发板并接入到局域网哦~

**<font color="black" size=5 face="仿宋">1.  OpenCV测试程序</font>**

&#160;&#160;&#160;&#160;JetPack3.0为TX1预装的OpenCV版本为OpenCV2.4.13，环境变量都已配置好，所以我们无需在开发套件上进行任何操作。

1.新建CUDA C++工程，如下图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604153758615?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

2.填入工程名称，工程类型为空工程，工具链为CUDA Toolkit 8.0，单击Next：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604154103462?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

3.基础设置，这里我们之前已经说过了，对于TX1，全部选5.3。当然按默认的2.0一般也不会报错：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604154321262?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

4.目标系统，默认的是主机。如果你之前设置过，这次你只需要单击下拉条就会出现之前的设置，选中就好。我在这里还是重复一遍操作吧，单击Manage：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604154558719?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

5.上一步骤之后出现远程连接对话框，填入开发板IP地址和用户名，其他的默认就好，然后单击Finish退出：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604154853351?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="450" /></div>
<p></p>

6.把Local System关掉，然后选择远程连接的工程路径、工具箱路径和CPU类型地，选完后单击Finish。7、8、9会分别对这三项进行详细说明：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604155215776?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

7.Project Path：在步骤6中单击Project Path后面的Browse，出现下图所示对话框，里面显示的是远程连接的开发套件的文件目录，可以进行简单的新建删除等功能。这里我们选择好自己的工程路径。中间可能会出现链接提示，有时还会让你填写TX1的密码，大家乖乖填上就好：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604155631075?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

8.Toolkit：在步骤6中单击Tookit后面的Manage，弹出下图所示对话框，第一次打开的话里面的Toolkit path 是空的。我们不需要自己去找工具箱的路径，只需要单击Detect，系统就会帮我们自动填写上工具箱路径。中间可能会出现链接提示，有时还会让你填写TX1的密码，大家乖乖填上就好：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604161321037?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

9.CPU Architecture：下拉菜单中有很多类型，我们的TX1对应的是AArch64：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604161207942?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400" height="200" /></div>
<p></p>

10.对新建的工程添加源文件：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604161538150?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

11.因为我们例程用的是OpenCV的hpp头文件，所以源文件最好也写成C++的源文件。填入文件名，选择默认的C++源文件模板：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604162124259?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

12.测试代码如下：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604162232987?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="300" /></div>
<p></p>

```
/*
 * test_opencv.cpp
 *
 *  Created on: Jun 3, 2017
 *      Author: wordzzzz
 */

#include "opencv2/highgui/highgui.hpp"

int main()
{
        cv::Mat img(512, 512, CV_8UC3, cv::Scalar(0));

        cv::putText(img,
                "Hello, OpenCV on Jetson!",
                cv::Point(10, img.rows / 2),
                cv::FONT_HERSHEY_DUPLEX,
                1.0,
                CV_RGB(118, 185, 0),
                2);
        cv::imshow("Hello!", img);
        cv::waitKey();
}

```

13.查看库链接。前面已经说到，JetPack3.0已经预装了OpenCV2.4.13，各种环境变量都已设置好，具体信息我们可以通过如下命令在TX1上进行查看：
```
pkg-config --cflags --libs opencv
```
&#160;&#160;&#160;&#160;查看结果如下图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604162731793?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="80" /></div>
<p></p>

```
ubuntu@tegra-ubuntu:~$ pkg-config --cflags --libs opencv
-I/usr/include/opencv -L/usr/local/cuda-8.0/lib64 -lopencv_calib3d -lopencv_contrib -lopencv_core -lopencv_features2d -lopencv_flann -lopencv_gpu -lopencv_highgui -lopencv_imgproc -lopencv_legacy -lopencv_ml -lopencv_objdetect -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_ts -lopencv_video -lopencv_videostab -lopencv_detection_based_tracker -lopencv_esm_panorama -lopencv_facedetect -lopencv_imuvstab -lopencv_tegra -lopencv_vstab -lcufft -lnpps -lnppi -lnppc -lcudart -latomic -ltbb -lrt -lpthread -lm -ldl
```

14.打开性能设置，即在工程右键，然后选择Properties：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604163304172?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="600" /></div>
<p></p>

15.重点来了：添加库链接。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604165804309?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="450" /></div>
<p></p>


<font color="red">&#160;&#160;&#160;&#160;如果你在第12步直接构建上述代码，肯定会出错，因为我们现在所有的设置，都是在为我们的工程文件编写编译命令。熟悉g++/gcc的朋友们可能会有印象，就是我们直接用g++/gcc编译文件的时候，如果用到哪个链接库，一般都是在后面加上-l这种链接的，否则会找不到相应的库链接。</font>

16.经过上述步骤后再CTR+B进行构建，我们可以看到构建命令包括了我们添加的库链接，没有报错表明构建成功：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604171314582?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="250" /></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604170640220?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="100" /></div>
<p></p>


**<font color="black" size=5 face="仿宋">2.  Gstreamer测试程序</font>**

1.新建项目，过程不再赘述，然后添加源文件，这次我们用C模板，而不是C++。

```
/*
 * basic-tutorial-1.c
 *
 *  Created on: Jun 1, 2017
 *      Author: wordzzzz
 */

#include <gst/gst.h>

int main(int argc, char *argv[]) {
  GstElement *pipeline;
  GstBus *bus;
  GstMessage *msg;

  /* Initialize GStreamer */
  gst_init (&argc, &argv);

  /* Build the pipeline */
  pipeline = gst_parse_launch ("playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm", NULL);

  /* Start playing */
  gst_element_set_state (pipeline, GST_STATE_PLAYING);

  /* Wait until error or EOS */
  bus = gst_element_get_bus (pipeline);
  msg = gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE, GST_MESSAGE_ERROR | GST_MESSAGE_EOS);

  /* Free resources */
  if (msg != NULL)
    gst_message_unref (msg);
  gst_object_unref (bus);
  gst_element_set_state (pipeline, GST_STATE_NULL);
  gst_object_unref (pipeline);
  return 0;
}
```


2.添加头文件路径和库链接：此处比OpenCV多了头文件路径的添加，因为gstremer-1.0的头文件路径貌似没有加入到环境变量中。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604172555604?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604173708168?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

<font color="red">&#160;&#160;&#160;&#160;上图中添加的路径，切记是TX1上的路径，而不是虚拟机里的路径。之前用过QT进行过交叉编译，感觉被洗脑了。Nsight的构建，是通过你的设置产生的编译命令，直接在TX1上进行编译的，而不是先生成在虚拟机里再拷贝到TX1上。所以你只要保证你添加的头文件路径和库路径能在TX1上找到就行，没必要把TX1上的库都拷过来，这样反而会出错。</font>

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170604172613325?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="500" /></div>
<p></p>

3.CTRL+B构建后在TX1上运行，会有视频播放出来。


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**