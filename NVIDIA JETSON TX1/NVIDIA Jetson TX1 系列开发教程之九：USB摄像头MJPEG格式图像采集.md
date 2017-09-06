**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之九：USB摄像头MJPEG格式图像采集</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">写在前面：</font>**
NVIDIA Jetson TX1开发套件包括一个CSI接口的500W摄像头，其官方多媒体例程基于此摄像头详细介绍了图像采集与编解码的种种操作。整个多媒体例程是基于gstreamer框架来写的，如果大家只是想把摄像头用起来，大可不必研究gstreamer框架，直接看接口就行。

由于项目需求，考虑到接线问题，我们放弃了CSI接口的摄像头，转而采用USB摄像头。而且最新的官方例程已经给出了基于V4L2的USB摄像头图像采集与编码例程。（12_camera_v4l2_cuda_video_encode）。

本博客将介绍三种在TX1上实现USB摄像头图像采集与显示的方法，分别基于gstreamer、ffmpeg和V4L2来实现USB摄像头的图像实时采集与显示。博主在项目中实现拍照和预览模式切换，最终采用的是V4L2+OpenCV2.4.13的方案，因为博主对V4L2比较熟悉，而对于gstreamer和ffmpeg只接触到了皮毛，对其理解也不是很到位。

注意：博主所用的USB摄像头输出图像格式为mjpeg，所以所有的程序中都是以jpeg为例进行编写的，如果想改成YUYV格式的，只需要改一下format设置，并把相应的解码程序去掉即可。

**<font color="black" size=5 face="仿宋">一、Gstreamer：</font>**

博主只是直接把测试好的gstreamer命令行写入到程序中，并不能对数据流做更多的操作，在多次尝试无果后放弃······

```
/*
 * main.cpp
 *
 *  Created on: May 31, 2017
 *      Author: wordzzzz
 */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <iostream>
#include <errno.h>

using namespace std;

int main(int argc, char **argv)
{
	//花括号内输入测试号的命令行即可
	char cmd_cap[] = {"gst-launch-1.0 v4l2src device= /dev/video1 ! 'image/jpeg, width=640, height=480' ! jpegdec ! xvimagesink -e &"};

	char *cmdstring = cmd_cap;

	if(*argv[1] == '1')
		cmdstring = cmd_cap;

	int status;
	if(NULL == cmdstring) //如果cmdstring为空趁早闪退吧，尽管system()函数也能处理空指针
	{
		return 0;
	}
	status = system(cmdstring);
	if(status < 0)
	{
		printf("cmd: %s\t error: %s", cmdstring, strerror(errno)); // 这里务必要把errno信息输出或
		return 0;
	}

	if(WIFEXITED(status))
	{
		printf("normal termination, exit status = %d\n", WEXITSTATUS(status)); //取得cmdstring执行结果
	}
		else if(WIFSIGNALED(status))
	{
		printf("abnormal termination,signal number =%d\n", WTERMSIG(status)); //如果cmdstring被信号中断，取得信号值
	}
	else if(WIFSTOPPED(status))
	{
		printf("process stopped, signal number =%d\n", WSTOPSIG(status)); //如果cmdstring被信号暂停执行，取得信号值
	}
}

```


**<font color="black" size=5 face="仿宋">二、Ffmpeg：</font>**
ffmpeg程序参考雷神博客，并在其基础上进行修改，ffmpeg编解码+SDL多线程定时刷新显示，程序详解还是看雷神的吧，打开传送门：http://blog.csdn.net/leixiaohua1020/article/details/38868499

```
/*
 * ffm_cap.cpp
 *
 *  Created on: Jul 19, 2017
 *      Author: wordzzzz
 */

#include <stdio.h>

#define __STDC_CONSTANT_MACROS

//Linux...
#ifdef __cplusplus
extern "C"
{
#endif
#include <libavcodec/avcodec.h>
#include <libavformat/avformat.h>
#include <libswscale/swscale.h>
#include <libavdevice/avdevice.h>
#include <SDL/SDL.h>
#ifdef __cplusplus
};
#endif

//Output YUV420P
#define OUTPUT_YUV420P 0
//'1' Use Dshow
//'0' Use VFW
#define USE_DSHOW 0


//Refresh Event
#define SFM_REFRESH_EVENT  (SDL_USEREVENT + 1)

#define SFM_BREAK_EVENT  (SDL_USEREVENT + 2)

int thread_exit=0;

//SDL线程40ms刷新一次
int sfp_refresh_thread(void *opaque)
{
	thread_exit=0;
	while (!thread_exit) {
		SDL_Event event;
		event.type = SFM_REFRESH_EVENT;
		SDL_PushEvent(&event);
		SDL_Delay(40);
	}
	thread_exit=0;
	//Break
	SDL_Event event;
	event.type = SFM_BREAK_EVENT;
	SDL_PushEvent(&event);

	return 0;
}

int main(int argc, char* argv[])
{

	AVFormatContext	*pFormatCtx;
	int				i, videoindex;
	AVCodecContext	*pCodecCtx;
	AVCodec			*pCodec;

	av_register_all();
	avformat_network_init();
	pFormatCtx = avformat_alloc_context();

	//Open File
	//char filepath[]="src01_480x272_22.h265";
	//avformat_open_input(&pFormatCtx,filepath,NULL,NULL)

	//Register Device
	avdevice_register_all();

    //Linux：参数设置
	AVDictionary* options = NULL;
	av_dict_set(&options, "input_format", "mjpeg", 0); //输入格式
	av_dict_set(&options, "framerate", "30", 0);       //帧率
	av_dict_set(&options, "video_size", "1280x720", 0);//图像像素
	AVInputFormat *ifmt=av_find_input_format("video4linux2");
	if(avformat_open_input(&pFormatCtx,"/dev/video1",ifmt,&options)!=0){
		printf("Couldn't open input stream.\n");
		return -1;
	}
	av_dict_free(&options);

	if(avformat_find_stream_info(pFormatCtx,NULL)<0)
	{
		printf("Couldn't find stream information.\n");
		return -1;
	}
	videoindex=-1;
	for(i=0; i<pFormatCtx->nb_streams; i++)
		if(pFormatCtx->streams[i]->codec->codec_type==AVMEDIA_TYPE_VIDEO)
		{
			videoindex=i;
			break;
		}
	if(videoindex==-1)
	{
		printf("Couldn't find a video stream.\n");
		return -1;
	}
	pCodecCtx=pFormatCtx->streams[videoindex]->codec;
	pCodec=avcodec_find_decoder(pCodecCtx->codec_id);
	if(pCodec==NULL)
	{
		printf("Codec not found.\n");
		return -1;
	}
	if(avcodec_open2(pCodecCtx, pCodec,NULL)<0)
	{
		printf("Could not open codec.\n");
		return -1;
	}
	printf("pCodecCtx->codec_id %d\n", pCodecCtx->codec_id);
	printf("pCodecCtx->width %d\n", pCodecCtx->width);
	printf("pCodecCtx->height %d\n", pCodecCtx->height);
	printf("pCodecCtx->pix_fmt %d\n", pCodecCtx->pix_fmt);
	AVFrame	*pFrame,*pFrameYUV;
	pFrame=av_frame_alloc();
	pFrameYUV=av_frame_alloc();
	//unsigned char *out_buffer=(unsigned char *)av_malloc(avpicture_get_size(AV_PIX_FMT_YUV420P, pCodecCtx->width, pCodecCtx->height));
	//avpicture_fill((AVPicture *)pFrameYUV, out_buffer, AV_PIX_FMT_YUV420P, pCodecCtx->width, pCodecCtx->height);
	//SDL----------------------------
	if(SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO | SDL_INIT_TIMER)) {
		printf( "Could not initialize SDL - %s\n", SDL_GetError());
		return -1;
	}
	int screen_w=0,screen_h=0;
	SDL_Surface *screen;
	screen_w = pCodecCtx->width;
	screen_h = pCodecCtx->height;
	screen = SDL_SetVideoMode(screen_w, screen_h, 0,0);

	if(!screen) {
		printf("SDL: could not set video mode - exiting:%s\n",SDL_GetError());
		return -1;
	}
	SDL_Overlay *bmp;
	bmp = SDL_CreateYUVOverlay(pCodecCtx->width, pCodecCtx->height,SDL_YV12_OVERLAY, screen);
	SDL_Rect rect;
	rect.x = 0;
	rect.y = 0;
	rect.w = screen_w;
	rect.h = screen_h;
	//SDL End------------------------
	int ret, got_picture;

	AVPacket *packet=(AVPacket *)av_malloc(sizeof(AVPacket));

#if OUTPUT_YUV420P
    FILE *fp_yuv=fopen("output.yuv","wb+");
#endif

	struct SwsContext *img_convert_ctx;
	img_convert_ctx = sws_getContext(pCodecCtx->width, pCodecCtx->height, pCodecCtx->pix_fmt, pCodecCtx->width, pCodecCtx->height, AV_PIX_FMT_YUVJ420P, SWS_BICUBIC, NULL, NULL, NULL);
	//------------------------------
	SDL_Thread *video_tid = SDL_CreateThread(sfp_refresh_thread,NULL);
	//
	SDL_WM_SetCaption("Simplest FFmpeg Read Camera",NULL);
	//Event Loop
	SDL_Event event;

	for (;;) {
		//Wait
		SDL_WaitEvent(&event);
		if(event.type==SFM_REFRESH_EVENT){
			//------------------------------
			if(av_read_frame(pFormatCtx, packet)>=0){
				printf("packet->stream_index = %d\n", packet->stream_index);
				if(packet->stream_index==videoindex){
					printf("videoindex = %d\n", videoindex);
					ret = avcodec_decode_video2(pCodecCtx, pFrame, &got_picture, packet);
					if(ret < 0){
						printf("Decode Error.\n");
						return -1;
					}
					if(got_picture){
						SDL_LockYUVOverlay(bmp);
						pFrameYUV->data[0]=bmp->pixels[0];
						pFrameYUV->data[1]=bmp->pixels[2];
						pFrameYUV->data[2]=bmp->pixels[1];
						pFrameYUV->linesize[0]=bmp->pitches[0];
						pFrameYUV->linesize[1]=bmp->pitches[2];
						pFrameYUV->linesize[2]=bmp->pitches[1];
						sws_scale(img_convert_ctx, (const unsigned char* const*)pFrame->data, pFrame->linesize, 0, pCodecCtx->height, pFrameYUV->data, pFrameYUV->linesize);

#if OUTPUT_YUV420P
						int y_size=pCodecCtx->width*pCodecCtx->height;
						fwrite(pFrameYUV->data[0],1,y_size,fp_yuv);    //Y
						fwrite(pFrameYUV->data[1],1,y_size/4,fp_yuv);  //U
						fwrite(pFrameYUV->data[2],1,y_size/4,fp_yuv);  //V
#endif

						SDL_UnlockYUVOverlay(bmp);

						SDL_DisplayYUVOverlay(bmp, &rect);

					}
				}
				av_free_packet(packet);
			}else{
				//Exit Thread
				thread_exit=1;
			}
		}else if(event.type==SDL_QUIT){
			thread_exit=1;
		}else if(event.type==SFM_BREAK_EVENT){
			break;
		}

	}


	sws_freeContext(img_convert_ctx);

#if OUTPUT_YUV420P
    fclose(fp_yuv);
#endif

	SDL_Quit();

	//av_free(out_buffer);
	av_free(pFrameYUV);
	avcodec_close(pCodecCtx);
	avformat_close_input(&pFormatCtx);

	return 0;
}
```



**<font color="black" size=5 face="仿宋">三、V4L2+OpenCV2.4.13：</font>**

这里贴出自己的代码，其中V4L2各功能函数是按照标准的V4L2框架进行编写的。本篇博客只贴出摄像头图像采集程序代码，下一篇将针对拍照+预览程序进行代码讲解：
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
 * main.cpp
 *
 *  Created on: Jul 26, 2017
 *      Author: wordzzzz
 */

#include "include.h"
#include "v4l2cap.h"

#define IMAGEWIDTH 640
#define IMAGEHEIGHT 480

#define TRUE 1
#define FALSE 0

int main()
{
	IplImage* img,* img_cap;
	CvMat cvmat,cvmat_cap;
	double t;
	unsigned char *frame = NULL;
	unsigned long frameSize = 0;
	string videoDev="/dev/video1";

	V4L2Capture *vcap = new V4L2Capture(const_cast<char*>(videoDev.c_str()),
			IMAGEWIDTH, IMAGEHEIGHT, IMAGEWIDTH_CAP, IMAGEHEIGHT_CAP);
	vcap->preBegin();                                //预览模式开启

	cvNamedWindow("one",CV_WINDOW_AUTOSIZE);         //创建显示窗口

	while(1){
		t = (double)cvGetTickCount();
		vcap->getFrame((void **) &frame, &frameSize);//获取图像队列
		cvmat = cvMat(IMAGEHEIGHT,IMAGEWIDTH,CV_8UC3,frame);//CV_8UC3
		img = cvDecodeImage(&cvmat,1);               //OpenCV解码
		if(!img)    printf("No img\n");
		cvShowImage("one",img);                      //显示图像
		cvReleaseImage(&img);                        //循环外释放也可以
		vcap->backFrame();                           //返回图像队列
        cvWaitKey(1);                                //不写这句话将无法显示图像
		t=(double)cvGetTickCount()-t;
		printf("used time is %gms\n",(t/(cvGetTickFrequency()*1000)));
	}
	vcap->preEnd();                                  //预览结束

	return 0;
}

```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**