**<font color="black" size=6 face="仿宋">NVIDIA Jetson TX1 系列开发教程之六：SSD安装与测试</font>**

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **虚拟机系统：Ubuntu14.04**
- **编者: WordZzzz**

----------

**<font color="red" size=5 face="仿宋">写在前面：</font>**
本博文原打算以SSD为例，介绍如何在NVIDIA Jetson TX1上安装SSD，并进行图片检测。

**<font color="black" size=5 face="仿宋">安装过程：</font>**

1.用以下命令安装依赖包:
```
$ sudo add-apt-repository universe
$ sudo add-apt-repository multiverse
$ sudo apt-get update
$ sudo apt-get install libboost-all-dev libprotobuf-dev libleveldb-dev libsnappy-dev
$ sudo apt-get install libhdf5-serial-dev protobuf-compiler libgflags-dev libgoogle-glog-dev
$ sudo apt-get install liblmdb-dev libblas-dev libatlas-base-dev libopenblas-dev
```

2.下载SSD源码安装包从如下网站：
SSD：https://github.com/weiliu89/caffe.git
```
$ git clone https://github.com/weiliu89/caffe.git
```
3.设置路径并解压：
a.如果是从官网下载的zip压缩包，则进行如下操作：
```
$ mkdir -pv $HOME/Work/caffe
$ cp caffe-ssd.zip $HOME/Work/caffe/
$ cd $HOME/Work/caffe/ && unzip caffe-ssd.zip
```
b.如果是git获得的，则进行如下操作：
```
$ mv caffe $HOME/Work/caffe/caffe-ssd
```
无论进行a操作还是b操作，最好都进行一下版本切换：
```
$ cd ~/Work/caffe/caffe-ssd
$ git checkout ssd
```
4.安装python依赖：
```
$ cd ../python
$ sudo apt-get install python-pip python-numpy
$ sudo pip install --upgrade pip
$ for req in $(cat requirements.txt); do pip install $req; done
```

requirements.txt内容如下：
```
Cython>=0.19.2
numpy>=1.7.1
scipy>=0.13.2
scikit-image>=0.9.3
matplotlib>=1.3.1
ipython>=3.0.0
h5py>=2.2.0
leveldb>=0.191
networkx>=1.8.1
nose>=1.3.0
pandas>=0.12.0
python-dateutil>=2.6.0
protobuf>=2.5.0
python-gflags>=2.0
pyyaml>=3.10
Pillow>=2.3.0
six>=1.1.0
```
5.编译SSD源码:

```
$ cd $HOME/Work/caffe/caffe-ssd
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
$ make runtest -j4(此部分报错，可以不跑)
$ make pycaffe -j4
```

**<font color="black" size=5 face="仿宋">环境变量配置：</font>**
1.在终端执行如下指令：
```
$ sudo vim ~/.bashrc
```
2.在最后一行添加caffe的python路径
```
export PYTHONPATH=$HOME/Work/caffe/caffe-ssd/python:$PYTHONPATH
```
然后加上之前声明的环境变量，这样就不用每次make或者运行的时候再次声明环境变量了。
```
export TEGRA_ARMABI=aarch64-linux-gnu
export DISPLAY=:0
export LD_LIBRARY_PATH=$HOME/Work/caffe/caffe-ssd/build/lib:/usr/local/cuda/lib64
```
3.source环境变量，在终端执行如下命令：
```
$ source ~/.bashrc
```

**<font color="black" size=5 face="仿宋">测试：</font>**
1.使用jupyter或者ipython打开notebook：
```
$ jupyter notebook
```
2.修改caffe路径，需要在下图的标记位置写上自己的caffe路径：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170715163635152?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="800" /></div>
<p></p>

3.下载VGG训练模型，直接从官网下载即可，然后解压到models下。并确认下图中标记框中路径是否正确：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170715163725622?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="800" /></div>
<p></p>

4.置信度，通过调节置信度阈值，来控制在检测结果中需要显示的显示框：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170715163753223?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="800" /></div>
<p></p>

5.测试结果：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170715163821537?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="600" /></div>
<p></p>

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**