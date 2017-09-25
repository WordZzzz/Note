# NVIDIA Jetson TX1 系列开发教程之十二：libcurl、RapidJSON安装

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **嵌入式平台：NVIDIA Jetson TX1**
- **嵌入式系统：Ubuntu16.04**
- **编者: WordZzzz**

----------

[toc]

----------

**<font color="red" size=4 face="仿宋">&emsp;&emsp;各位看客注意了，本章教程无关硬件平台，在ubuntu下具有通用性，别被标题赶跑了哦。我踩过的坑，您就不要再踩了，枪在手，跟我走。有问题欢迎评论区留言~</font>**

**<font color="red" size=4 face="仿宋">&emsp;&emsp;系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

----------

## 前言

&emsp;&emsp;由于项目需要，WordZzzz最近尝试在TX1上解析服务器端传回json信息。先用Python3进行了测试，Python作为一门脚本语言，用来做测试简直棒极了。但是，我们最终还是要把代码移植到linux C上来进行实现。

&emsp;&emsp;虽说大家能看到这篇文章一般都是想直接解决问题的，但是我还是在此啰嗦几句，做个小小的科普。

&emsp;&emsp;JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。 易于人阅读和编写。同时也易于机器解析和生成。 它基于JavaScript Programming Language, Standard ECMA-262 3rd Edition - December 1999的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。 这些特性使JSON成为理想的数据交换语言。

JSON建构于两种结构：

- “名称/值”对的集合（A collection of name/value pairs）。不同的语言中，它被理解为对象（object），纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。
- 值的有序列表（An ordered list of values）。在大部分语言中，它被理解为数组（array）。
这些都是常见的数据结构。事实上大部分现代计算机语言都以某种形式支持它们。这使得一种数据格式在同样基于这些结构的编程语言之间交换成为可能。
- 再详细点的话，就请进入传送门吧：http://www.json.org/json-zh.html。

&emsp;&emsp;用c在linux下实现json解析，需要两个工具，一个是可以实现http通信的工具，这里WordZzzz用的是libcurl（当然也可以用curl或者wget来模拟http的post和get）；另一个是可以解析json信息的工具，这里WordZzzz用的是腾讯开源的RapidJSON（当然大家也可以尝试json-c等）。

## libcurl

### 简介

- cURL是一个利用URL语法在命令行下工作的文件传输工具，1997年首次发行。它支持文件上传和下载，所以是综合传输工具，但按传统，习惯称cURL为下载工具。cURL还包含了用于程序开发的libcurl。
- cURL支持的通信协议有FTP、FTPS、HTTP、HTTPS、TFTP、SFTP、Gopher、SCP、Telnet、DICT、FILE、LDAP、LDAPS、IMAP、POP3、SMTP和RTSP。
- libcurl支持的平台有Solaris、NetBSD、FreeBSD、OpenBSD、Darwin、HP-UX、IRIX、AIX、Tru64、Linux、UnixWare、HURD、Windows、Symbian、Amiga、OS/2、BeOS、Mac OS X、Ultrix、QNX、BlackBerry Tablet OS、OpenVMS、RISC OS、Novell NetWare、DOS等。
- 想要了解更多信息，官网传送门：https://curl.haxx.se/



### 安装

1.下载libcurl源码：

```
$ git clone https://github.com/curl/curl.git
```
2.配置：

```
$ ./buidconf
$ ./configure --enable-debug
```

&emsp;&emsp;第一步用于生成configure配置文件，第二步进行配置。可以./configure --help查看其他可选参数。我是默认安装openssl的，所以没有出现找不到openssl库的问题。如果遇到了就装一个，选择默认安装省事，自己指定安装目录比较麻烦。具体查看工程目录下的README。

3.编译&安装：

&emsp;&emsp;默认库文件安装在/usr/local/lib 头文件安装在/usr/local/include  --->安装要root权限

```
$ make
$ sudo make install
```

4.测试代码：
&emsp;&emsp;接下来写个测试代码来使用libcurl库(此测试代码下载指定URL的页面)。
&emsp;&emsp;测试代码摘自网络，如下：
```
// 采用CURLOPT_WRITEFUNCTION 实现网页下载保存功能  

#include <stdio.h>;  
#include <stdlib.h>;  
#include <unistd.h>;  
   
#include <curl/curl.h>;  
#include <curl/types.h>;  
#include <curl/easy.h>;  
   
FILE *fp;  //定义FILE类型指针  
//这个函数是为了符合CURLOPT_WRITEFUNCTION而构造的  
//完成数据保存功能  
size_t write_data(void *ptr, size_t size, size_t nmemb, void *stream)    
{  
    int written = fwrite(ptr, size, nmemb, (FILE *)fp);  
    return written;  
}  
   
int main(int argc, char *argv[])  
{  
    CURL *curl;  
    if (argc != 3)  
    {  
        fprintf(stderr, "usage: %s url filename\n", argv[0]);  
        exit(-1);  
    }  
    curl_global_init(CURL_GLOBAL_ALL);    
    curl = curl_easy_init();  
    curl_easy_setopt(curl, CURLOPT_URL, argv[1]);    
   
    if((fp = fopen(argv[2],"w")) == NULL)  
    {  
        curl_easy_cleanup(curl);  
        exit(1);  
    }  
  //CURLOPT_WRITEFUNCTION 将后继的动作交给write_data函数处理  
    curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_data);    
    curl_easy_perform(curl);  
    curl_easy_cleanup(curl);  
    exit(0);  
}  
```

5.查看库链接信息：

```
$ pkg-config --cflags --libs libcurl
-I/usr/local/include -L/usr/local/lib -lcurl
```

&emsp;&emsp;返回信息如上，其中-I后面的是头文件路径，-L后面的是链接库路径，链接库名称为curl。

6.编译测试代码：

&emsp;&emsp;可以直接用nsight进行配置（详情参考我之前的nsight基础教程：http://blog.csdn.net/u011475210/article/details/72853170、nsight进阶教程：http://blog.csdn.net/u011475210/article/details/72860277），也可以直接手写gcc命令进行编译。

```
$ gcc -L/usr/local/lib -o test test.c -lcurl 
```

7.编译完成，运行程序：

```
$ ./test
```

&emsp;&emsp;程序会将百度首页下载下来，存为index.html。

## RapidJSON

### 简介

&emsp;&emsp;RapidJSON 是一个 C++ 的 JSON 解析器及生成器。它的灵感来自 RapidXml。

- RapidJSON 小而全。它同时支持 SAX 和 DOM 风格的 API。SAX 解析器只有约 500 行代码。
- RapidJSON 快。它的性能可与 strlen() 相比。可支持 SSE2/SSE4.2 加速。
- RapidJSON 独立。它不依赖于 BOOST 等外部库。它甚至不依赖于 STL。
- RapidJSON 对内存友好。在大部分 32/64 位机器上，每个 JSON 值只占 16 字节（除字符串外）。它预设使用一个快速的内存分配器，令分析器可以紧凑地分配内存。
- RapidJSON 对 Unicode 友好。它支持 UTF-8、UTF-16、UTF-32 (大端序／小端序)，并内部支持这些编码的检测、校验及转码。例如，RapidJSON 可以在分析一个 UTF-8 文件至 DOM 时，把当中的 JSON 字符串转码至 UTF-16。它也支持代理对（surrogate pair）及 "\u0000"（空字符）。
- 想要了解更多信息，官网传送门：http://rapidjson.org/zh-cn/

### 安装

1.cmake：
&emsp;&emsp;采用cmake方式进行安装，所以cmake命令得有。

```
$ sudo apt-get install cmake
```

2.下载源码：

```
$ git clone https://github.com/Tencent/rapidjson.git
```

3.编译&安装：

```
$ cd rapidjson/
$ mkdir build
$ cd build
$ cmake ..
$ sudo make install
```

4.查看库链接信息：

```
$ pkg-config --cflags --libs RapidJSON
-I/usr/local/include
```

&emsp;&emsp;可以发现只有头文件信息，我们只需要调用头文件而不需要链接库，所以很方便。

5.官方例程：

```
// rapidjson/example/simpledom/simpledom.cpp`
#include "rapidjson/document.h"
#include "rapidjson/writer.h"
#include "rapidjson/stringbuffer.h"
#include <iostream>
using namespace rapidjson;
int main() {
    // 1. 把 JSON 解析至 DOM。
    const char* json = "{\"project\":\"rapidjson\",\"stars\":10}";
    Document d;
    d.Parse(json);
    // 2. 利用 DOM 作出修改。
    Value& s = d["stars"];
    s.SetInt(s.GetInt() + 1);
    // 3. 把 DOM 转换（stringify）成 JSON。
    StringBuffer buffer;
    Writer<StringBuffer> writer(buffer);
    d.Accept(writer);
    // Output {"project":"rapidjson","stars":11}
    std::cout << buffer.GetString() << std::endl;
    return 0;
}
```

&emsp;&emsp;注意此例子并没有处理潜在错误。

6.编译例程：

```
$ g++ -o simpledom simpledom.cpp
```

7.运行：

```
$ ./simpledom
```

&emsp;&emsp;图解过程：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170924171738668?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;输出结果：

```
{"project":"rapidjson","stars":11}
```

----------

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**

----------