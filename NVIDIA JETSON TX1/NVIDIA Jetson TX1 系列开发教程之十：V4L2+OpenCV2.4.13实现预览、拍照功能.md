**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之十：V4L2+OpenCV2.4.13实现预览、拍照功能</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">写在前面：</font>**
本篇博文主要用来讲解上篇博文中关于V4L2框架的程序部分，并在其基础上实现预览和拍照功能的实现.其中，预览分辨率640*480，用键盘检测来改变标志位，当标志位改变时进行拍照，拍照分辨率1920*1080，随后自动切换回预览模式，肉眼基本看不出切换卡顿。

**<font color="black" size=5 face="仿宋">一、V4L2框架：</font>**
V4L2程序是基于标准的V4L2框架来写的，大部分程序注释都已给出，如果还有什么不清楚的地方，可以评论、私信或自行查阅V4L2开发手册。

下图是标准V4L2程序流程图。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170817103503714?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

博客中的程序，在标准V4L2程序的基础上进一步进行了封装，主要是为了实现拍照与预览模式的切换。
```
/*
 * v4l2cap.h
 *
 *  Created on: Jul 26, 2017
 *      Author: wordzzzz
 */

#ifndef V4L2CAP_H_
#define V4L2CAP_H_

#include "include.h"

#define CLEAR(x) memset(&(x), 0, sizeof(x))

#define TRUE 1
#define FALSE 0

class V4L2Capture {
public:
	V4L2Capture(char *devName, int width, int height, int width_cap, int height_cap);
	virtual ~V4L2Capture();

	int openDevice();
	int closeDevice();
	int initDevice();
	int initDeviceCap();
	int startCapture();
	int stopCapture();
	int freeBuffers();
	int getFrame(void **,size_t *);
	int backFrame();
	int pre2cap();
	int cap2pre();
	int preBegin();
	int preEnd();

	int initBuffers();

	struct cam_buffer
	{
		void* start;
		unsigned int length;
	};
	char *devName;
	int widthCap;
	int heightCap;
	int width;
	int height;
	int fd_cam;
	cam_buffer *buffers;
	unsigned int n_buffers;
	int frameIndex;
};

#endif /* V4L2CAP_H_ */

```

```
/*
 * v4l2cap.cpp
 *
 *  Created on: Jul 26, 2017
 *      Author: wordzzzz
 */

#include "v4l2cap.h"

V4L2Capture::V4L2Capture(char *devName, int width, int height, int width_cap, int height_cap) {
	// TODO Auto-generated constructor stub
	this->devName = devName;
	this->fd_cam = -1;
	this->buffers = NULL;
	this->n_buffers = 0;
	this->frameIndex = -1;
	this->width=width;
	this->height=height;
	this->widthCap=width_cap;
	this->heightCap=height_cap;
}

V4L2Capture::~V4L2Capture() {
	// TODO Auto-generated destructor stub
}

/**********打开设备**********/
int V4L2Capture::openDevice() {
	/*设备的打开*/
	printf("video dev : %s\n", devName);
	fd_cam = open(devName, O_RDWR);
	if (fd_cam < 0) {
		perror("Can't open video device");
	}
	return TRUE;
}

/**********关闭设备**********/
int V4L2Capture::closeDevice() {
	if (fd_cam > 0) {
		int ret = 0;
		if ((ret = close(fd_cam)) < 0) {
			perror("Can't close video device");
		}
		return TRUE;
	} else {
		return FALSE;
	}
}

/**********初始化设备（预览模式）**********/
int V4L2Capture::initDevice() {
	int ret;
	struct v4l2_capability cam_cap;		//显示设备信息
	struct v4l2_cropcap cam_cropcap;	//设置摄像头的捕捉能力
	struct v4l2_fmtdesc cam_fmtdesc;	//查询所有支持的格式：VIDIOC_ENUM_FMT
	struct v4l2_crop cam_crop;		    //图像的缩放
	struct v4l2_format cam_format;		//设置摄像头的视频制式、帧格式等

	/* 使用IOCTL命令VIDIOC_QUERYCAP，获取摄像头的基本信息*/
	ret = ioctl(fd_cam, VIDIOC_QUERYCAP, &cam_cap);
	if (ret < 0) {
		perror("Can't get device information: VIDIOCGCAP");
	}
	printf(
			"Driver Name:%s\nCard Name:%s\nBus info:%s\nDriver Version:%u.%u.%u\n",
			cam_cap.driver, cam_cap.card, cam_cap.bus_info,
			(cam_cap.version >> 16) & 0XFF, (cam_cap.version >> 8) & 0XFF,
			cam_cap.version & 0XFF);

	/* 使用IOCTL命令VIDIOC_ENUM_FMT，获取摄像头所有支持的格式*/
	cam_fmtdesc.index = 0;
	cam_fmtdesc.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	printf("Support format:\n");
	while (ioctl(fd_cam, VIDIOC_ENUM_FMT, &cam_fmtdesc) != -1) {
		printf("\t%d.%s\n", cam_fmtdesc.index + 1, cam_fmtdesc.description);
		cam_fmtdesc.index++;
	}

	/* 使用IOCTL命令VIDIOC_CROPCAP，获取摄像头的捕捉能力*/
	cam_cropcap.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	if (0 == ioctl(fd_cam, VIDIOC_CROPCAP, &cam_cropcap)) {
		printf("Default rec:\n\tleft:%d\n\ttop:%d\n\twidth:%d\n\theight:%d\n",
				cam_cropcap.defrect.left, cam_cropcap.defrect.top,
				cam_cropcap.defrect.width, cam_cropcap.defrect.height);
		/* 使用IOCTL命令VIDIOC_S_CROP，获取摄像头的窗口取景参数*/
		cam_crop.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
		cam_crop.c = cam_cropcap.defrect;		//默认取景窗口大小
		if (-1 == ioctl(fd_cam, VIDIOC_S_CROP, &cam_crop)) {
			//printf("Can't set crop para\n");
		}
	} else {
		printf("Can't set cropcap para\n");
	}

	/* 使用IOCTL命令VIDIOC_S_FMT，设置摄像头帧信息*/
	cam_format.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	cam_format.fmt.pix.width = width;
	cam_format.fmt.pix.height = height;
	cam_format.fmt.pix.pixelformat = V4L2_PIX_FMT_MJPEG;		//要和摄像头支持的类型对应
	cam_format.fmt.pix.field = V4L2_FIELD_INTERLACED;
	ret = ioctl(fd_cam, VIDIOC_S_FMT, &cam_format);
	if (ret < 0) {
		perror("Can't set frame information");
	}
	/* 使用IOCTL命令VIDIOC_G_FMT，获取摄像头帧信息*/
	cam_format.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	ret = ioctl(fd_cam, VIDIOC_G_FMT, &cam_format);
	if (ret < 0) {
		perror("Can't get frame information");
	}
	printf("Current data format information:\n\twidth:%d\n\theight:%d\n",
			cam_format.fmt.pix.width, cam_format.fmt.pix.height);
	return TRUE;
}

/**********初始化设备（拍照模式）**********/
int V4L2Capture::initDeviceCap() {
	int ret;
	struct v4l2_format cam_format;		//设置摄像头的视频制式、帧格式等

	/* 使用IOCTL命令VIDIOC_S_FMT，设置摄像头帧信息*/
	cam_format.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	cam_format.fmt.pix.width = widthCap;
	cam_format.fmt.pix.height = heightCap;
	cam_format.fmt.pix.pixelformat = V4L2_PIX_FMT_MJPEG;		//要和摄像头支持的类型对应
	cam_format.fmt.pix.field = V4L2_FIELD_INTERLACED;
	ret = ioctl(fd_cam, VIDIOC_S_FMT, &cam_format);
	if (ret < 0) {
		perror("Can't set frame information");
	}
	/* 使用IOCTL命令VIDIOC_G_FMT，获取摄像头帧信息*/
	cam_format.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	ret = ioctl(fd_cam, VIDIOC_G_FMT, &cam_format);
	if (ret < 0) {
		perror("Can't get frame information");
	}
	printf("Current data format information:\n\twidth:%d\n\theight:%d\n",
			cam_format.fmt.pix.width, cam_format.fmt.pix.height);
	return TRUE;
}

/**********申请缓存**********/
int V4L2Capture::initBuffers() {
	int ret;
	/* 使用IOCTL命令VIDIOC_REQBUFS，申请帧缓冲*/
	struct v4l2_requestbuffers req;
	CLEAR(req);
	req.count = 4;
	req.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	req.memory = V4L2_MEMORY_MMAP;
	ret = ioctl(fd_cam, VIDIOC_REQBUFS, &req);
	if (ret < 0) {
		perror("Request frame buffers failed");
	}
	if (req.count < 2) {
		perror("Request frame buffers while insufficient buffer memory");
	}
	buffers = (struct cam_buffer*) calloc(req.count, sizeof(*buffers));
	if (!buffers) {
		perror("Out of memory");
	}
	for (n_buffers = 0; n_buffers < req.count; n_buffers++) {
		struct v4l2_buffer buf;
		CLEAR(buf);
		// 查询序号为n_buffers 的缓冲区，得到其起始物理地址和大小
		buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
		buf.memory = V4L2_MEMORY_MMAP;
		buf.index = n_buffers;
		ret = ioctl(fd_cam, VIDIOC_QUERYBUF, &buf);
		if (ret < 0) {
			printf("VIDIOC_QUERYBUF %d failed\n", n_buffers);
			return FALSE;
		}
		buffers[n_buffers].length = buf.length;
		//printf("buf.length= %d\n",buf.length);
		// 映射内存
		buffers[n_buffers].start = mmap(
				NULL, // start anywhere
				buf.length, PROT_READ | PROT_WRITE, MAP_SHARED, fd_cam,
				buf.m.offset);
		if (MAP_FAILED == buffers[n_buffers].start) {
			printf("mmap buffer%d failed\n", n_buffers);
			return FALSE;
		}
	}
	return TRUE;
}

/**********释放缓存**********/
int V4L2Capture::freeBuffers() {
	unsigned int i;
	for (i = 0; i < n_buffers; ++i) {
		if (-1 == munmap(buffers[i].start, buffers[i].length)) {
			printf("munmap buffer%d failed\n", i);
			return FALSE;
		}
	}
	free(buffers);
	return TRUE;
}

/**********开始采集**********/
int V4L2Capture::startCapture() {
	unsigned int i;
	for (i = 0; i < n_buffers; i++) {
		struct v4l2_buffer buf;
		CLEAR(buf);
		buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
		buf.memory = V4L2_MEMORY_MMAP;
		buf.index = i;
		if (-1 == ioctl(fd_cam, VIDIOC_QBUF, &buf)) {
			printf("VIDIOC_QBUF buffer%d failed\n", i);
			return FALSE;
		}
	}
	enum v4l2_buf_type type;
	type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	if (-1 == ioctl(fd_cam, VIDIOC_STREAMON, &type)) {
		printf("VIDIOC_STREAMON error");
		return FALSE;
	}
	return TRUE;
}


/**********停止采集**********/
int V4L2Capture::stopCapture() {
	enum v4l2_buf_type type;
	type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	if (-1 == ioctl(fd_cam, VIDIOC_STREAMOFF, &type)) {
		printf("VIDIOC_STREAMOFF error\n");
		return FALSE;
	}
	return TRUE;
}

/**********获取图像**********/
int V4L2Capture::getFrame(void **frame_buf, size_t* len) {
	struct v4l2_buffer queue_buf;
	CLEAR(queue_buf);
	queue_buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
	queue_buf.memory = V4L2_MEMORY_MMAP;
	if (-1 == ioctl(fd_cam, VIDIOC_DQBUF, &queue_buf)) {
		printf("VIDIOC_DQBUF error\n");
		return FALSE;
	}
	//printf("queue_buf.index=%d\n",queue_buf.index);
	//pthread_rwlock_wrlock(&rwlock);
	*frame_buf = buffers[queue_buf.index].start;
	*len = buffers[queue_buf.index].length;
	frameIndex = queue_buf.index;
	//pthread_rwlock_unlock(&rwlock);
	return TRUE;
}

/**********返回队列**********/
int V4L2Capture::backFrame() {
	if (frameIndex != -1) {
		struct v4l2_buffer queue_buf;
		CLEAR(queue_buf);
		queue_buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
		queue_buf.memory = V4L2_MEMORY_MMAP;
		queue_buf.index = frameIndex;
		if (-1 == ioctl(fd_cam, VIDIOC_QBUF, &queue_buf)) {
			printf("VIDIOC_QBUF error\n");
			return FALSE;
		}
		return TRUE;
	}
	return FALSE;
}

/**********预览切换至拍照**********/
int V4L2Capture::pre2cap() {
	if(V4L2Capture::stopCapture() == FALSE){
		printf("StopCapture fail~~\n");
		exit(2);
	}
	if(V4L2Capture::freeBuffers() == FALSE){
		printf("FreeBuffers fail~~\n");
		exit(2);
	}
	if(V4L2Capture::closeDevice() == FALSE){
		printf("CloseDevice fail~~\n");
		exit(1);
	}
	if(V4L2Capture::openDevice() == FALSE){
		printf("OpenDevice fail~~\n");
		exit(1);
	}
	if(V4L2Capture::initDeviceCap() == FALSE){
		printf("InitDeviceCap fail~~\n");
		exit(1);
	}
	if(V4L2Capture::initBuffers() == FALSE){
		printf("InitBuffers fail~~\n");
		exit(2);
	}
	if(V4L2Capture::startCapture() == FALSE){
		printf("StartCapture fail~~\n");
		exit(2);
	}
	return TRUE;
}

/**********拍照切换至预览**********/
int V4L2Capture::cap2pre() {
	if(V4L2Capture::stopCapture() == FALSE){
		printf("StopCapture fail~~\n");
		exit(2);
	}
	if(V4L2Capture::freeBuffers() == FALSE){
		printf("FreeBuffers fail~~\n");
		exit(2);
	}
	if(V4L2Capture::closeDevice() == FALSE){
		printf("CloseDevice fail~~\n");
		exit(1);
	}
	if(V4L2Capture::openDevice() == FALSE){
		printf("OpenDevice fail~~\n");
		exit(1);
	}
	if(V4L2Capture::initDevice() == FALSE){
		printf("InitDevice fail~~\n");
		exit(1);
	}
	if(V4L2Capture::initBuffers() == FALSE){
		printf("InitBuffers fail~~\n");
		exit(2);
	}
	if(V4L2Capture::startCapture() == FALSE){
		printf("StartCapture fail~~\n");
		exit(2);
	}
	return TRUE;
}

/**********预览开启**********/
int V4L2Capture::preBegin() {
	if(V4L2Capture::openDevice() == FALSE){
		printf("OpenDevice fail~~\n");
		exit(1);
	}
	printf("first~~\n");
	if(V4L2Capture::initDevice() == FALSE){
		printf("InitDevice fail~~\n");
		exit(1);
	}
	printf("second~~\n");
	if(V4L2Capture::initBuffers() == FALSE){
		printf("InitBuffers fail~~\n");
		exit(2);
	}
	printf("third~~\n");
	if(V4L2Capture::startCapture() == FALSE){
		printf("StartCapture fail~~\n");
		exit(2);
	}
	printf("fourth~~\n");
	return TRUE;
}

/**********预览结束**********/
int V4L2Capture::preEnd() {
	if(V4L2Capture::stopCapture() == FALSE){
		printf("StopCapture fail~~\n");
		exit(2);
	}
	if(V4L2Capture::freeBuffers() == FALSE){
		printf("FreeBuffers fail~~\n");
		exit(2);
	}
	if(V4L2Capture::closeDevice() == FALSE){
		printf("CloseDevice fail~~\n");
		exit(1);
	}
	return TRUE;
}
```


**<font color="black" size=5 face="仿宋">二、键盘检测：</font>**
主要实现的功能是通过模拟kbhit功能，实现在线程中完成对键盘的检测功能（检测到‘c’时标志位置1）。
```
/*
 * pthread.h
 *
 *  Created on: Jul 27, 2017
 *      Author: wordzzzz
 */

#ifndef PTHREAD_H_
#define PTHREAD_H_

#include "include.h"

static __inline
int tty_reset(void);

static __inline
int tty_set(void);

static __inline
int kbhit(void);

void *thread(void *arg);

static struct termios ori_attr, cur_attr;

extern uchar flag;

#endif /* PTHREAD_H_ */

```

```
/*
 * pthread.cpp
 *
 *  Created on: Jul 27, 2017
 *      Author: wordzzzz
 */

#include "thread.h"

static __inline
int tty_reset(void)
{
        if (tcsetattr(STDIN_FILENO, TCSANOW, &ori_attr) != 0)
                return -1;

        return 0;
}

static __inline
int tty_set(void)
{

        if ( tcgetattr(STDIN_FILENO, &ori_attr) )
                return -1;

        memcpy(&cur_attr, &ori_attr, sizeof(cur_attr) );
        cur_attr.c_lflag &= ~ICANON;
//        cur_attr.c_lflag |= ECHO;
        cur_attr.c_lflag &= ~ECHO;
        cur_attr.c_cc[VMIN] = 1;
        cur_attr.c_cc[VTIME] = 0;

        if (tcsetattr(STDIN_FILENO, TCSANOW, &cur_attr) != 0)
                return -1;

        return 0;
}

static __inline
int kbhit(void)
{

        fd_set rfds;
        struct timeval tv;
        int retval;

        /* Watch stdin (fd 0) to see when it has input. */
        FD_ZERO(&rfds);
        FD_SET(0, &rfds);
        /* Wait up to five seconds. */
        tv.tv_sec  = 0;
        tv.tv_usec = 0;

        retval = select(1, &rfds, NULL, NULL, &tv);
        /* Don't rely on the value of tv now! */

        if (retval == -1) {
                perror("select()");
                return 0;
        } else if (retval)
                return 1;
        /* FD_ISSET(0, &rfds) will be true. */
        else
                return 0;
        return 0;
}

void *thread(void *arg)
{
    int tty_set_flag;
    tty_set_flag = tty_set();
    while(1) {

		if( kbhit() ) {
			const int key = getchar();
			printf("%c pressed\n", key);
			//检测到'c'则标志位置1
			if(key == 'c')
				flag=1;
			//检测到'q'则退出程序
			if(key == 'q')
				exit(0);
				break;
		} else {
//              fprintf(stderr, "<no key detected>\n");
		}
    }

    if(tty_set_flag == 0)
            tty_reset();
    return 0;
}

```


**<font color="black" size=5 face="仿宋">三、主函数：</font>**
include.h里面包含我们所有用到的头文件。

```
/*
 * include.h
 *
 *  Created on: Jul 26, 2017
 *      Author: wordzzzz
 */

#ifndef INCLUDE_H_
#define INCLUDE_H_

extern "C" {
#include <unistd.h>
#include <error.h>
#include <fcntl.h>
#include <pthread.h>
#include <termios.h>
#include <linux/videodev2.h>
#include <sys/mman.h>
#include <sys/time.h>
#include <sys/ioctl.h>
#include <sys/types.h>

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
}

#include <iostream>
#include <iomanip>
#include <string>

#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace std;

#endif /* INCLUDE_H_ */

```
主函数主要实现预览模式的开启、键盘检测线程开启，预览和拍照模式的切换等功能，总体程序比较简单。
```
/*
 * main.cpp
 *
 *  Created on: Jul 26, 2017
 *      Author: wordzzzz
 */

#include "include.h"
#include "v4l2cap.h"
#include "thread.h"

#define IMAGEWIDTH_CAP 1920 //拍照分辨率
#define IMAGEHEIGHT_CAP 1080//拍照分辨率

#define IMAGEWIDTH 640      //预览分辨率
#define IMAGEHEIGHT 480     //预览分辨率

#define TRUE 1
#define FALSE 0

uchar flag;

int main()
{
	IplImage* img,* img_cap;
	CvMat cvmat,cvmat_cap;
	double t;
	flag=0;
	unsigned char *frame = NULL;
	unsigned long frameSize = 0;
	string videoDev="/dev/video1";//制定设备号

	V4L2Capture *vcap = new V4L2Capture(const_cast<char*>(videoDev.c_str()),
			IMAGEWIDTH, IMAGEHEIGHT, IMAGEWIDTH_CAP, IMAGEHEIGHT_CAP);
	vcap->preBegin();//预览模式开启

	cvNamedWindow("one",CV_WINDOW_AUTOSIZE);//创建显示窗口

	pthread_t id;
	printf("Main thread id is %d \n",pthread_self());
	if(!pthread_create(&id,NULL,thread,NULL))
	{
	printf("succeed!\n");
	}
	else
	{printf("Fail to Create Thread");
	return -1;
	}

	while(1){
		//如果flag为1，则抓取一张照片
		if(flag == 1){
			vcap->pre2cap();                      //预览模式切换至拍照模式
			//这里多获取几次图像队列，以便得到更高的图像质量（刚打开设备时图像模糊）
			vcap->getFrame((void **) &frame, &frameSize);vcap->backFrame();
			vcap->getFrame((void **) &frame, &frameSize);vcap->backFrame();
			vcap->getFrame((void **) &frame, &frameSize);
			cvmat_cap = cvMat(IMAGEHEIGHT_CAP,IMAGEWIDTH_CAP,CV_8UC3,frame);//CV_8UC3
			img_cap = cvDecodeImage(&cvmat_cap,1);//OpenCV图像解码

			if(!img_cap)    printf("No img_cap\n");
			cvSaveImage("cap.jpg",img_cap);       //保存图片
			cvReleaseImage(&img_cap);             //释放img_cap
			vcap->backFrame();                    //返回队列

			vcap->cap2pre();                      //拍照模式切换至预览模式

			flag = 0;
		}
		t = (double)cvGetTickCount();
		vcap->getFrame((void **) &frame, &frameSize);
		cvmat = cvMat(IMAGEHEIGHT,IMAGEWIDTH,CV_8UC3,frame);//CV_8UC3
		img = cvDecodeImage(&cvmat,1);           //OpenCV图像解码
		if(!img)    printf("No img\n");
		cvShowImage("one",img);                  //显示图片
		cvReleaseImage(&img);                    //释放img
		vcap->backFrame();                       //返回队列
        cvWaitKey(1);                            //没有这句话图像无法显示
		t=(double)cvGetTickCount()-t;
		printf("used time is %gms\n",(t/(cvGetTickFrequency()*1000)));
	}

	pthread_exit(0);                            //退出键盘检测线程

	vcap->preEnd();                             //预览模式结束

	return 0;
}

```


**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**