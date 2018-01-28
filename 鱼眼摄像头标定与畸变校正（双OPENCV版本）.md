---
title: 鱼眼摄像头标定与畸变校正（双OPENCV版本）
comments: true
mathjax: true
categories:
  - OPENCV
tags:
  - OPENCV2
  - OPENCV3
  - 畸变校正
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/fisheye_calibration**
- **软件版本：VS2013+OPENCV2.4.13 OR VS2013+OPENCV3.4.0**
- **编&emsp;&emsp;者：WordZzzz**

----------

&emsp;&emsp;最近在整理自己以前做过的一些东西，这是基于opencv的鱼眼摄像头畸变校正程序的[github地址](https://github.com/WordZzzz/fisheye_calibration)。

其中：

- normal_calibrate：基于OPENCV2与OPENCV3通用的函数实现，可实现USB摄像头实时畸变校正；
- fishey_calibrate：基于OPENCV3独有的fishyey结构体实现，可实现USB摄像头实时畸变校正；
- fishey_calibrate_img：基于OPENCV3独有的fishyey结构体实现，可实现单张图片畸变校正；

&emsp;&emsp;当然，还是要贴上官方文档的：

- [Camera calibration With OpenCV2](https://docs.opencv.org/2.4/doc/tutorials/calib3d/camera_calibration/camera_calibration.html)；
- [Camera calibration With OpenCV3](https://docs.opencv.org/trunk/db/d58/group__calib3d__fisheye.html)；

**<font color="red" size=3 face="仿宋">希望对需要的人能有所帮助，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**