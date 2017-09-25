# NVIDIA Jetson TX1 系列开发教程之十四：YOLO安装与优化加速

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **编者: WordZzzz**

----------

[toc]

----------

**<font color="red" size=3 face="仿宋">&emsp;&emsp;系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

----------
## 前言

&emsp;&emsp;下周甲方要来查看项目进行，我着急忙慌的在刚刷完机的板子上编译YOLO，然而，webcom不好用······记性真是差的不行，赶紧打开markdown，把先前的笔记都整理出来。

&emsp;&emsp;本篇文章分为两个部分，第一部分是YOLO安装，第二部分是YOLO的优化加速。

## YOLO安装

&emsp;&emsp;YOLO官网：https://pjreddie.com/darknet/yolo/

&emsp;&emsp;不说废话，就是干。

1.获取源码：

```
git clone https://github.com/pjreddie/darknet
cd darknet
```

2.修改Makefile

&emsp;&emsp;我们打开MakeFile文件：

```
vim Makefile
```

&emsp;&emsp;Makefile内容如下：

```
GPU=0
CUDNN=0
OPENCV=0
OPENMP=0
DEBUG=0

ARCH= -gencode arch=compute_20,code=[sm_20,sm_21] \
      -gencode arch=compute_30,code=sm_30 \
      -gencode arch=compute_35,code=sm_35 \
      -gencode arch=compute_50,code=[sm_50,compute_50] \
      -gencode arch=compute_52,code=[sm_52,compute_52]

# This is what I use, uncomment if you know your arch and want to specify
# ARCH= -gencode arch=compute_52,code=compute_52

VPATH=./src/:./examples
SLIB=libdarknet.so
ALIB=libdarknet.a
EXEC=darknet
OBJDIR=./obj/

CC=gcc
NVCC=nvcc 
AR=ar
ARFLAGS=rcs
OPTS=-Ofast
LDFLAGS= -lm -pthread 
COMMON= -Iinclude/ -Isrc/
CFLAGS=-Wall -Wno-unknown-pragmas -Wfatal-errors -fPIC

ifeq ($(OPENMP), 1) 
CFLAGS+= -fopenmp
endif

ifeq ($(DEBUG), 1) 
OPTS=-O0 -g
endif

CFLAGS+=$(OPTS)

ifeq ($(OPENCV), 1) 
COMMON+= -DOPENCV
CFLAGS+= -DOPENCV
LDFLAGS+= `pkg-config --libs opencv` 
COMMON+= `pkg-config --cflags opencv` 
endif

ifeq ($(GPU), 1) 
COMMON+= -DGPU -I/usr/local/cuda/include/
CFLAGS+= -DGPU
LDFLAGS+= -L/usr/local/cuda/lib64 -lcuda -lcudart -lcublas -lcurand
endif

ifeq ($(CUDNN), 1) 
COMMON+= -DCUDNN 
CFLAGS+= -DCUDNN
LDFLAGS+= -lcudnn
endif

OBJ=gemm.o utils.o cuda.o deconvolutional_layer.o convolutional_layer.o list.o image.o activations.o im2col.o col2im.o blas.o crop_layer.o dropout_layer.o maxpool_layer.o softmax_layer.o data.o matrix.o network.o connected_layer.o cost_layer.o parser.o option_list.o detection_layer.o route_layer.o box.o normalization_layer.o avgpool_layer.o layer.o local_layer.o shortcut_layer.o activation_layer.o rnn_layer.o gru_layer.o crnn_layer.o demo.o batchnorm_layer.o region_layer.o reorg_layer.o tree.o  lstm_layer.o
EXECOBJA=captcha.o lsd.o super.o voxel.o art.o tag.o cifar.o go.o rnn.o rnn_vid.o compare.o segmenter.o regressor.o classifier.o coco.o dice.o yolo.o detector.o  writing.o nightmare.o swag.o darknet.o 
ifeq ($(GPU), 1) 
LDFLAGS+= -lstdc++ 
OBJ+=convolutional_kernels.o deconvolutional_kernels.o activation_kernels.o im2col_kernels.o col2im_kernels.o blas_kernels.o crop_layer_kernels.o dropout_layer_kernels.o maxpool_layer_kernels.o network_kernels.o avgpool_layer_kernels.o
endif

EXECOBJ = $(addprefix $(OBJDIR), $(EXECOBJA))
OBJS = $(addprefix $(OBJDIR), $(OBJ))
DEPS = $(wildcard src/*.h) Makefile include/darknet.h

#all: obj backup results $(SLIB) $(ALIB) $(EXEC)
all: obj  results $(SLIB) $(ALIB) $(EXEC)


$(EXEC): $(EXECOBJ) $(ALIB)
	$(CC) $(COMMON) $(CFLAGS) $^ -o $@ $(LDFLAGS) $(ALIB)

$(ALIB): $(OBJS)
	$(AR) $(ARFLAGS) $@ $^

$(SLIB): $(OBJS)
	$(CC) $(CFLAGS) -shared $^ -o $@ $(LDFLAGS)

$(OBJDIR)%.o: %.c $(DEPS)
	$(CC) $(COMMON) $(CFLAGS) -c $< -o $@

$(OBJDIR)%.o: %.cu $(DEPS)
	$(NVCC) $(ARCH) $(COMMON) --compiler-options "$(CFLAGS)" -c $< -o $@

obj:
	mkdir -p obj
backup:
	mkdir -p backup
results:
	mkdir -p results

.PHONY: clean

clean:
	rm -rf $(OBJS) $(SLIB) $(ALIB) $(EXEC) $(EXECOBJ)

```

&emsp;&emsp;我们只需要关注前几行：

```
GPU=0
CUDNN=0
OPENCV=0
OPENMP=0
DEBUG=0

ARCH= -gencode arch=compute_20,code=[sm_20,sm_21] \
      -gencode arch=compute_30,code=sm_30 \
      -gencode arch=compute_35,code=sm_35 \
      -gencode arch=compute_50,code=[sm_50,compute_50] \
      -gencode arch=compute_52,code=[sm_52,compute_52]
```

&emsp;&emsp;前五行的这些类似于宏定义，在make的时候会导致某些程序功能的开启与关闭，0代表关闭，1代表开启。我直接把前四个都打开了。想用GPU，那么前两行就得打开；想用webcom等OpenCV实现的程序，那么就需要打开OPENCV；至于OPENCMP，我只知道它是用来实现多线程优化加速的，所以我干脆也打开了。

- TX1计算能力是53，对应的配置：

```
GPU=1
CUDNN=1
OPENCV=1
OPENMP=1
DEBUG=0

ARCH= -gencode arch=compute_53,code=[sm_53,sm_53]
```

- TX1计算能力是53，对应的配置：

```
GPU=1
CUDNN=1
OPENCV=1
OPENMP=1
DEBUG=0

ARCH= -gencode arch=compute_62,code=[sm_62,sm_62]
```

3.保存Makefile之后，开始编译：

```
make
```

4.下载预训练好的模型：

```
wget https://pjreddie.com/media/files/yolo.weights
wget https://pjreddie.com/media/files/tiny-yolo-voc.weights
wget https://pjreddie.com/media/files/tiny-yolo.weights
```

5.测试：

&emsp;&emsp;我直接用webcom来测试。注意，必须使用支持V4L2的摄像头，板载的摄像头是不支持的，当然，如果你在Makefile里面没打开Opencv，这里是会报错的。

- COCO数据集训练的YOLO（干跑3帧左右）：

```
./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights
```

- COCO数据集训练的TINY-YOLO（干跑15帧左右）：

```
./darknet detector demo cfg/coco.data cfg/tiny-yolo.cfg tiny-yolo.weights
```

- COCO数据集训练的TINY-YOLO（干跑15帧左右）：

```
./darknet detector demo cfg/voc.data cfg/tiny-yolo-voc.cfg tiny-yolo-voc.weights
```

注：大家如果想看看不开GPU跑成什么样子，可以加入参数-nogpu。举个栗子：

- COCO数据集训练的TINY-YOLO（关掉GPU）：

```
./darknet -nogpu detector demo cfg/voc.data cfg/tiny-yolo-voc.cfg tiny-yolo-voc.weights
```

- 查看GPU使用情况：

```
$ sudo ~/tegrastats
```

## YOLO的优化加速

&emsp;&emsp;大家是不是不满足上面的帧率，别着急，WordZzzz带着你搞优化啊。正经说来，也不算优化，就是调调参数，让代码跑的快点。

### 修改网络模型输入图像尺寸大小

&emsp;&emsp;YOLOv2做了很多优化，其中就有为了提高小物体检测准确率而增加的多尺度训练（这里说的不够专业，后面有时间了专门写篇讲解YOLO的文章）。

&emsp;&emsp;原来的YOLO网络使用固定的448 * 448的图片作为输入，现在加入anchor boxes后，输入变成了416 * 416。目前的网络只用到了卷积层和池化层，那么就可以进行动态调整（意思是可检测任意大小图片）。作者希望YOLOv2具有不同尺寸图片的鲁棒性，因此在训练的时候也考虑了这一点。

&emsp;&emsp;不同于固定输入网络的图片尺寸的方法，作者在几次迭代后就会微调网络。没经过10次训练（10 epoch），就会随机选择新的图片尺寸。YOLO网络使用的降采样参数为32，那么就使用32的倍数进行尺度池化{320,352，…，608}。最终最小的尺寸为320 * 320，最大的尺寸为608 * 608。接着按照输入尺寸调整网络进行训练。

&emsp;&emsp;这种机制使得网络可以更好地预测不同尺寸的图片，意味着同一个网络可以进行不同分辨率的检测任务，在小尺寸图片上YOLOv2运行更快，在速度和精度上达到了平衡。

&emsp;&emsp;所以，我们可以修改输入尺寸大小，来提高YOLO运行速度。

&emsp;&emsp;随便打开一个cfg下的cfg文件，如“tiny-yolo.cfg”,内容如下：

```
[net]
# Training
# batch=64
# subdivisions=2
# Testing
batch=1
subdivisions=1
width=416
height=416
channels=3
momentum=0.9
decay=0.0005
angle=0
saturation = 1.5
exposure = 1.5
hue=.1

learning_rate=0.001
burn_in=1000
max_batches = 500200
policy=steps
steps=400000,450000
scales=.1,.1

[convolutional]
batch_normalize=1
filters=16
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=32
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=64
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=128
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=256
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=1

[convolutional]
batch_normalize=1
filters=1024
size=3
stride=1
pad=1
activation=leaky

###########

[convolutional]
batch_normalize=1
size=3
stride=1
pad=1
filters=512
activation=leaky

[convolutional]
size=1
stride=1
pad=1
filters=425
activation=linear

[region]
anchors =  0.57273, 0.677385, 1.87446, 2.06253, 3.33843, 5.47434, 7.88282, 3.52778, 9.77052, 9.16828
bias_match=1
classes=80
coords=4
num=5
softmax=1
jitter=.2
rescore=0

object_scale=5
noobject_scale=1
class_scale=1
coord_scale=1

absolute=1
thresh = .6
random=1

```

&emsp;&emsp;同样的，我们只需要看前面几行就行，把width和height修改成最小尺度：

```
width=288
height=288
```

&emsp;&emsp;此时再次使用COCO数据集训练的TINY-YOLO进行测试，帧率可以提高到20左右：

```
./darknet detector demo cfg/coco.data cfg/tiny-yolo.cfg tiny-yolo.weights
```

### 修改预览分辨率

&emsp;&emsp;可以直接X掉预览窗口，这样，预览窗口减小后，速度也会提升，只不过有时候效果不明显。

&emsp;&emsp;当然，大家也可以修改源码然后重新编译。源码中预览窗口大小的代码在src/demo.c中的第279行。我把预览分辨率改成了1280 * 720，当然你还可以改的更小。改完之后重新编译。

```
    if(!prefix){
        cvNamedWindow("Demo", CV_WINDOW_NORMAL);
        if(fullscreen){
            cvSetWindowProperty("Demo", CV_WND_PROP_FULLSCREEN, CV_WINDOW_FULLSCREEN);
        } else {
            cvMoveWindow("Demo", 0, 0);
            cvResizeWindow("Demo", 1280, 720);
        }
    }
```

### 修改摄像头分辨率

&emsp;&emsp;一开始就让摄像头采集到的分辨率低点，也是有效果的。

```
./darknet detector demo cfg/coco.data cfg/tiny-yolo.cfg tiny-yolo.weights -w 640 -h 480
```

&emsp;&emsp;经过上述三步的优化之后，我用COCO数据集，帧率峰值都能到30（TX2，TX1慢5帧左右）。

----------

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**

----------