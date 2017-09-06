**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之五：CAFFE安装与NVIDIA多媒体例程测试</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="black" size=5 face="仿宋">写在前面的前面：</font>**
之前就已经在实验室的深度学习服务器上安装过CAFFE/SSD，由于当时深度学习服务器管理不佳，多人混用造成好多依赖环境删删减减，经常会出现今天装的CAFFE/SSD明天就不能用的情况，所以难免多折腾几次。因此，博主对他们的安装还是颇有研究的。
大家用NVIDIA Jetson TX1，无非就是看上了它的计算能力，能跑深度学习框架。由于NVIDIA Jetson TX1容量有限，所以建议大家需要哪个就安装哪个。偏向于学习，就安装纯版本的CAFFE，偏向于应用测试，就安装各个基于CAFFE的升级版，比如SSD。

**<font color="red" size=5 face="仿宋">写在前面：</font>**
本博文原打算以CAFFE/SSD为例，介绍如何在NVIDIA Jetson TX1上安装CAFFE/SSD，但是最近自己又安装了一遍，发现本博文的步骤不全面，导致python大部分依赖环境都没装上，这样的结果就是jupyter notebook这种工具用不了。所以本篇博文在此只介绍CAFFE安装和基于CAFFE的NVIDIA多媒体例程测试。

**<font color="black" size=5 face="仿宋">安装过程：</font>**

1.用以下命令安装依赖包:
```
$ sudo add-apt-repository universe
$ sudo add-apt-repository multiverse
$ sudo apt-get update
$ sudo apt-get install libboost-all-dev libprotobuf-dev libleveldb-dev libsnappy-dev
$ sudo apt-get install libhdf5-serial-dev protobuf-compiler libgflags-dev libgoogle-glog-dev
$ sudo apt-get install liblmdb-dev libblas-dev libatlas-base-dev
```

2.下载CAFFE源码安装包从如下网站：

CAFFE：https://github.com/BVLC/caffe.git
```
$ git clone https://github.com/BVLC/caffe.git
```

3设置路径并解压：
a.如果在步骤2中选择自己从网页手动下载zip文件，则进行如下操作：
CAFFE:
```
$ mkdir -pv $HOME/Work/caffe
$ cp caffe-master.zip $HOME/Work/caffe/
$ cd $HOME/Work/caffe/ && unzip caffe-master.zip
```
b.如果在步骤2中直接git得到caffe文件，则进行如下操作：
```
$ mv caffe $HOME/Work/caffe/caffe-master
```

4.编译CAFFE源码:
CAFFE:
```
$ cd $HOME/Work/caffe/caffe-master
```

```
$ cp Makefile.config.example Makefile.config
$ vi Makefile.config
```

去掉下面该行代码的注释：
```
USE_CUDNN := 1
```
重点来了，在Makefile.config中找到下面这几行：
```
CUDA_ARCH := -gencode arch=compute_30,code=sm_30 \
        -gencode arch=compute_35,code=sm_35 \
        -gencode arch=compute_50,code=sm_50 \
        -gencode arch=compute_52,code=sm_52 \
        -gencode arch=compute_60,code=sm_60 \
        -gencode arch=compute_61,code=sm_61 \
        -gencode arch=compute_61,code=compute_61
```
更改为：
```
CUDA_ARCH := -gencode arch=compute_53,code=sm_53
```
这里的后缀数字53是TX1的计算能力，在其他平台上编译CAFFE也是同样的道理，要把计算能力改成对应的值，否则有可能会报错。关于计算能力如何确定，CUDA例程里面有测试程序，跑一下就可以输出GPU性能指标。

声明下面这两行路径，保存后退出：
```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/aarch64-linux-gnu/hdf5/serial
```
```
$ make -j4
```
完成后在build/lib目录下会出现库文件libcaffe.so。
```
$ make all -j4
$ make runtest -j4
$ make pycaffe -j4
```

5.编译opencv用户库
这个库是CAFFE所必需的。而且只能在目标板上编译。
```
$ cd ~/tegra_multimedia_api/samples/11_camera_object_identification/opencv_consumer_lib
```
检查并确保正确设置makefile以下变量：
```
CUDA_DIR:=/usr/local/cuda
CAFFE_DIR:=$HOME/Work/caffe/caffe-master
```
编译：
```
$ make
```
完成后当前目录下会出现库文件 libopencv_consumer.so

6. 通过下面的命令下载CAFFE模型二进制文件：
```
$ sudo apt-get install python-pip
$ sudo pip install pyyaml
$ cd $HOME/Work/caffe/caffe-master
$ ./scripts/download_model_binary.py models/bvlc_reference_caffenet/
```
用下面的命令获得ImageNet标签文件：
```
$ ./data/ilsvrc12/get_ilsvrc_aux.sh
```
7. 使用下列命令生成和运行示例：
```
$ cd ~/tegra_multimedia_api/samples/11_camera_object_identification
$ export TEGRA_ARMABI=aarch64-linux-gnu
$ export DISPLAY=:0
$ make
$ export LD_LIBRARY_PATH=$HOME/Work/caffe/caffe-master/build/lib:/usr/local/cuda/lib64
$ ./camera_caffe -width 1920 -height 1080 \
        -lib opencv_consumer_lib/libopencv_consumer.so \
        -model $HOME/Work/caffe/caffe-master/models/bvlc_reference_caffenet/deploy.prototxt \
        -trained $HOME/Work/caffe/caffe-master/models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel \
        -mean $HOME/Work/caffe/caffe-master/data/ilsvrc12/imagenet_mean.binaryproto \
        -label $HOME/Work/caffe/caffe-master/data/ilsvrc12/synset_words.txt
```
8、环境变量设置
1.在终端执行如下指令：
```
sudo vim ~/.bashrc
```
2.在最后一行添加caffe的python路径
```
export PYTHONPATH=$HOME/Work/caffe/caffe-master/python:$PYTHONPATH
```
然后加上之前声明的环境变量，这样就不用每次make或者运行的时候再次声明环境变量了。
```
export TEGRA_ARMABI=aarch64-linux-gnu
export DISPLAY=:0
export LD_LIBRARY_PATH=$HOME/Work/caffe/caffe-master/build/lib:/usr/local/cuda/lib64
```
3.source环境变量，在终端执行如下命令：
```
source ~/.bashrc
```
**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**