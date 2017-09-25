# NVIDIA Jetson TX1 系列开发教程之十三：使用OpenCV在图像上添加汉字

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

&emsp;&emsp;同样的，由于项目需要，WordZzzz最近尝试在TX1上实现使用Opencv将解析json后得到的信息添加到图片上，其中包括汉字。然而，事情并没我想象的那么简单，主要还是自己对编码格式这块不太熟悉。下面将分两部分进行介绍。

## 安装freetype

&emsp;&emsp;首先我们要检查一下自己的系统下面有没有freetype这个软件包：

```
$ pkg-config --cflags --libs freetype2
-I/usr/include/freetype2 -lfreetype
```
&emsp;&emsp;如果有上述第二行的打印信息，那么这部分可以跳过，如果没有，就需要按照下面的步骤进行安装了。

```
#打开终端，添加PPA源:

$ sudo add-apt-repository ppa:no1wantdthisname/ppa
$ freetype-ppa

#安装freetype有两种方式：

$ upgrade freetype

#或者：

$ sudo apt update && sudo apt install libfreetype6

#如果你想卸载，科一进行如下操作：

$ sudo apt install ppa-purge && sudo ppa-purge ppa:no1wantdthisname/ppa

```

## UTF-8与GBK的转换

ubuntu下系统默认的各种编码格式都是UTF-8，我们想要系统支持GBK，想让程序支持GBK，需要进行如下操作，我采用的方案二。

### 方案一：iconv函数族

&emsp;&emsp;iconv函数族的头文件是iconv.h,使用前需包含。

```
#include <iconv.h>
```

&emsp;&emsp;iconv函数族有三个函数,原型如下:
- (1) iconv_t iconv_open(const char *tocode, const char *fromcode);
此函数说明将要进行哪两种编码的转换,tocode是目标编码,fromcode是原编码,该函数返回一个转换句柄,供以下两个函数使用。
- (2) size_t iconv(iconv_t cd,char **inbuf,size_t *inbytesleft,char **outbuf,size_t *outbytesleft);
此函数从inbuf中读取字符,转换后输出到outbuf中,inbytesleft用以记录还未转换的字符数,outbytesleft用以记录输出缓冲的剩余空间。
- (3) int iconv_close(iconv_t cd);
此函数用于关闭转换句柄,释放资源。

&emsp;&emsp;实现代码如下：

```
#include <iconv.h>  
#include <stdlib.h>  
#include <stdio.h>  
#include <unistd.h>  
#include <fcntl.h>  
#include <string.h>  
#include <sys/stat.h>  
  
int code_convert(char *from_charset, char *to_charset, char *inbuf, size_t inlen,  
        char *outbuf, size_t outlen) {  
    iconv_t cd;  
    char **pin = &inbuf;  
    char **pout = &outbuf;  
  
    cd = iconv_open(to_charset, from_charset);  
    if (cd == 0)  
        return -1;  
    memset(outbuf, 0, outlen);  
    if (iconv(cd, pin, &inlen, pout, &outlen) == -1)  
        return -1;  
    iconv_close(cd);  
    *pout = '\0';  
  
    return 0;  
}  
  
int u2g(char *inbuf, size_t inlen, char *outbuf, size_t outlen) {  
    return code_convert("utf-8", "gb2312", inbuf, inlen, outbuf, outlen);  
}  
  
int g2u(char *inbuf, size_t inlen, char *outbuf, size_t outlen) {  
    return code_convert("gb2312", "utf-8", inbuf, inlen, outbuf, outlen);  
}  
  
int main(void) {  
    char *s = "中国";  
    int fd = open("test.txt", O_RDWR|O_CREAT, S_IRUSR | S_IWUSR);  
    char buf[10];  
    u2g(s, strlen(s), buf, sizeof(buf));  
    write(fd, buf, strlen(buf));  
    close(fd);  
  
    fd = open("test.txt2", O_RDWR|O_CREAT, S_IRUSR | S_IWUSR);  
    char buf2[10];  
    g2u(buf, strlen(buf), buf2, sizeof(buf2));  
    write(fd, buf2, strlen(buf2));  
    close(fd);  
    return 1;  
}  
```

### 方案二：使用mbstowcs和wcstombs

&emsp;&emsp;mbstowcs将多字节编码转换为宽字节编码；wcstombs将宽字节编码转换为多字节编码
注意，需要系统编码的支持，可以通过locale -a 查看系统支持的。若不支持zh_CN.gbk, 需要安装，在ubuntu上的安装步骤如下：

&emsp;&emsp;编辑配置文件：

```
$sudo vim /var/lib/locales/supported.d/zh-hans
```

&emsp;&emsp;加入下面几行代码：

```
zh_CN.UTF-8 UTF-8
zh_SG.UTF-8 UTF-8
zh_CN.GBK GBK
zh_CN.GB18030 GB18030
```
 
&emsp;&emsp;保存后更新：

```
$ sudo locale-gen
```

&emsp;&emsp;查看:

```
$ locale -a
C
POSIX
···
zh_CN.gb18030
zh_CN.gbk
zh_CN.utf8
zh_SG.utf8
```

&emsp;&emsp;测试代码：

```
#include <stdlib.h>  
#include <stdio.h>  
#include <string.h>  
#include <unistd.h>  
#include <fcntl.h>  
#include <sys/stat.h>  
#include <locale.h>  
  
/** 
 * DESCRIPTION: 实现由utf8编码到gbk编码的转换 
 * 
 * Input: gbkStr,转换后的字符串;  srcStr,待转换的字符串; maxGbkStrlen, gbkStr的最 
 大长度 
 * Output: gbkStr 
 * Returns: -1,fail;>0,success 
 * 
 */  
int utf82gbk(char *gbkStr, const char *srcStr, int maxGbkStrlen) {  
    if (NULL == srcStr) {  
        printf("Bad Parameter：srcStr\n");  
        return -1;  
    }  
  
    //首先先将utf8编码转换为unicode编码  
    if (NULL == setlocale(LC_ALL, "zh_CN.utf8")) //设置转换为unicode前的码,当前为utf8编码  
            {  
        printf("Bad Parameter：zh_CN.utf8\n");  
        return -1;  
    }  
  
    int unicodeLen = mbstowcs(NULL, srcStr, 0); //计算转换后的长度  
    if (unicodeLen <= 0) {  
        printf("Can not Transfer!!!\n");  
        return -1;  
    }  
    wchar_t *unicodeStr = (wchar_t *) calloc(sizeof(wchar_t), unicodeLen + 1);  
    mbstowcs(unicodeStr, srcStr, strlen(srcStr)); //将utf8转换为unicode  
  
    //将unicode编码转换为gbk编码  
    if (NULL == setlocale(LC_ALL, "zh_CN.gbk")) //设置unicode转换后的码,当前为gbk  
            {  
        printf("Bad Parameter：zh_CN.gbk\n");  
        return -1;  
    }  
    int gbkLen = wcstombs(NULL, unicodeStr, 0); //计算转换后的长度  
    if (gbkLen <= 0) {  
        printf("Can not Transfer!!!\n");  
        return -1;  
    } else if (gbkLen >= maxGbkStrlen) //判断空间是否足够  
            {  
        printf("Dst Str memory not enough\n");  
        return -1;  
    }  
    wcstombs(gbkStr, unicodeStr, gbkLen);  
    gbkStr[gbkLen] = 0; //添加结束符  
    free(unicodeStr);  
    return gbkLen;  
}  
  
int main(void) {  
    char *s = "我爱WordZzzz";  
    int fd = open("test.txt", O_RDWR | O_CREAT, S_IRUSR | S_IWUSR);  
    char buf[10];  
    utf82gbk(buf, s, sizeof(buf));  
    write(fd, buf, strlen(buf));  
    close(fd);  
  
    return 1;  
}  
```

## OpenCV的中文支持

&emsp;&emsp;OpenCV想要支持中文显示，需要添加额外的文件。网上参考的都是这两个文件，2007年的，历史悠久。

&emsp;&emsp;头文件CvxText.h：

```
#ifndef OPENCV_CVX_TEXT_2007_08_31_H
#define OPENCV_CVX_TEXT_2007_08_31_H

/**
* \file CvxText.h
* \brief OpenCV汉字输出接口
*
* 实现了汉字输出功能。
*/

#include <ft2build.h>
#include FT_FREETYPE_H

#include<opencv2/opencv.hpp>

/**
* \class CvxText
* \brief OpenCV中输出汉字
*
* OpenCV中输出汉字。字库提取采用了开源的FreeFype库。由于FreeFype是
* GPL版权发布的库，和OpenCV版权并不一致，因此目前还没有合并到OpenCV
* 扩展库中。
*
* 显示汉字的时候需要一个汉字字库文件，字库文件系统一般都自带了。
* 这里采用的是一个开源的字库：“文泉驿正黑体”。
*
* 关于"OpenCV扩展库"的细节请访问
* http://code.google.com/p/opencv-extension-library/
*
* 关于FreeType的细节请访问
* http://www.freetype.org/
*/


class CvxText
{
	// 禁止copy

	CvxText& operator=(const CvxText&);

	//================================================================
	//================================================================

public:

	/**
	* 装载字库文件
	*/

	CvxText(const char *freeType);
	virtual ~CvxText();

	//================================================================
	//================================================================

	/**
	* 获取字体。目前有些参数尚不支持。
	*
	* \param font        字体类型, 目前不支持
	* \param size        字体大小/空白比例/间隔比例/旋转角度
	* \param underline   下画线
	* \param diaphaneity 透明度
	*
	* \sa setFont, restoreFont
	*/

	void getFont(int *type,
		CvScalar *size = NULL, bool *underline = NULL, float *diaphaneity = NULL);

	/**
	* 设置字体。目前有些参数尚不支持。
	*
	* \param font        字体类型, 目前不支持
	* \param size        字体大小/空白比例/间隔比例/旋转角度
	* \param underline   下画线
	* \param diaphaneity 透明度
	*
	* \sa getFont, restoreFont
	*/

	void setFont(int *type,
		CvScalar *size = NULL, bool *underline = NULL, float *diaphaneity = NULL);

	/**
	* 恢复原始的字体设置。
	*
	* \sa getFont, setFont
	*/

	void restoreFont();

	//================================================================
	//================================================================

	/**
	* 输出汉字(颜色默认为黑色)。遇到不能输出的字符将停止。
	*
	* \param img  输出的影象
	* \param text 文本内容
	* \param pos  文本位置
	*
	* \return 返回成功输出的字符长度，失败返回-1。
	*/

	int putText(IplImage *img, const char    *text, CvPoint pos);

	/**
	* 输出汉字(颜色默认为黑色)。遇到不能输出的字符将停止。
	*
	* \param img  输出的影象
	* \param text 文本内容
	* \param pos  文本位置
	*
	* \return 返回成功输出的字符长度，失败返回-1。
	*/

	int putText(IplImage *img, const wchar_t *text, CvPoint pos);

	/**
	* 输出汉字。遇到不能输出的字符将停止。
	*
	* \param img   输出的影象
	* \param text  文本内容
	* \param pos   文本位置
	* \param color 文本颜色
	*
	* \return 返回成功输出的字符长度，失败返回-1。
	*/

	int putText(IplImage *img, const char    *text, CvPoint pos, CvScalar color);

	/**
	* 输出汉字。遇到不能输出的字符将停止。
	*
	* \param img   输出的影象
	* \param text  文本内容
	* \param pos   文本位置
	* \param color 文本颜色
	*
	* \return 返回成功输出的字符长度，失败返回-1。
	*/
	int putText(IplImage *img, const wchar_t *text, CvPoint pos, CvScalar color);

	//================================================================
	//================================================================

private:

	// 输出当前字符, 更新m_pos位置

	void putWChar(IplImage *img, wchar_t wc, CvPoint &pos, CvScalar color);

	//================================================================
	//================================================================

private:

	FT_Library   m_library;   // 字库
	FT_Face      m_face;      // 字体

	//================================================================
	//================================================================

	// 默认的字体输出参数

	int         m_fontType;
	CvScalar   m_fontSize;
	bool      m_fontUnderline;
	float      m_fontDiaphaneity;

	//================================================================
	//================================================================
};

#endif // OPENCV_CVX_TEXT_2007_08_31_H
```

&emsp;&emsp;源文件cvText.cpp：

```
/*
 * cvText.cpp
 *
 *  Created on: Sep 20, 2017
 *      Author: wordzzzz
 */

#include <wchar.h>
#include <assert.h>
#include <locale.h>
#include <ctype.h>

#include "CvxText.h"

//====================================================================
//====================================================================

// 打开字库

CvxText::CvxText(const char *freeType)
{
	assert(freeType != NULL);

	// 打开字库文件, 创建一个字体

	if (FT_Init_FreeType(&m_library)) throw;
	if (FT_New_Face(m_library, freeType, 0, &m_face)) throw;

	// 设置字体输出参数

	restoreFont();

	// 设置C语言的字符集环境

	setlocale(LC_ALL, "");
}

// 释放FreeType资源

CvxText::~CvxText()
{
	FT_Done_Face(m_face);
	FT_Done_FreeType(m_library);
}

// 设置字体参数:
//
// font         - 字体类型, 目前不支持
// size         - 字体大小/空白比例/间隔比例/旋转角度
// underline   - 下画线
// diaphaneity   - 透明度

void CvxText::getFont(int *type, CvScalar *size, bool *underline, float *diaphaneity)
{
	if (type) *type = m_fontType;
	if (size) *size = m_fontSize;
	if (underline) *underline = m_fontUnderline;
	if (diaphaneity) *diaphaneity = m_fontDiaphaneity;
}

void CvxText::setFont(int *type, CvScalar *size, bool *underline, float *diaphaneity)
{
	// 参数合法性检查

	if (type)
	{
		if (type >= 0) m_fontType = *type;
	}
	if (size)
	{
		m_fontSize.val[0] = fabs(size->val[0]);
		m_fontSize.val[1] = fabs(size->val[1]);
		m_fontSize.val[2] = fabs(size->val[2]);
		m_fontSize.val[3] = fabs(size->val[3]);
	}
	if (underline)
	{
		m_fontUnderline = *underline;
	}
	if (diaphaneity)
	{
		m_fontDiaphaneity = *diaphaneity;
	}
	//FT_Set_Pixel_Sizes(m_face, (int)m_fontSize.val[0], 0);
}

// 恢复原始的字体设置

void CvxText::restoreFont()
{
	m_fontType = 0;            // 字体类型(不支持)

	m_fontSize.val[0] = 20;      // 字体大小
	m_fontSize.val[1] = 0.5;   // 空白字符大小比例
	m_fontSize.val[2] = 0.1;   // 间隔大小比例
	m_fontSize.val[3] = 0;      // 旋转角度(不支持)

	m_fontUnderline = false;   // 下画线(不支持)

	m_fontDiaphaneity = 1.0;   // 色彩比例(可产生透明效果)

	// 设置字符大小

	FT_Set_Pixel_Sizes(m_face, (int)m_fontSize.val[0], 0);
}

// 输出函数(颜色默认为黑色)

int CvxText::putText(IplImage *img, const char    *text, CvPoint pos)
{
	return putText(img, text, pos, CV_RGB(255, 255, 255));
}
int CvxText::putText(IplImage *img, const wchar_t *text, CvPoint pos)
{
	return putText(img, text, pos, CV_RGB(255, 255, 255));
}

//

int CvxText::putText(IplImage *img, const char    *text, CvPoint pos, CvScalar color)
{
	if (img == NULL) return -1;
	if (text == NULL) return -1;

	//

	int i;
	for (i = 0; text[i] != '\0'; ++i)
	{
		wchar_t wc = text[i];

		// 解析双字节符号

		if (!isascii(wc)) mbtowc(&wc, &text[i++], 2);

		// 输出当前的字符

		putWChar(img, wc, pos, color);
	}
	return i;
}
int CvxText::putText(IplImage *img, const wchar_t *text, CvPoint pos, CvScalar color)
{
	if (img == NULL) return -1;
	if (text == NULL) return -1;

	//

	int i;
	for (i = 0; text[i] != '\0'; ++i)
	{
		// 输出当前的字符

		putWChar(img, text[i], pos, color);
	}
	return i;
}

// 输出当前字符, 更新m_pos位置

void CvxText::putWChar(IplImage *img, wchar_t wc, CvPoint &pos, CvScalar color)
{
	// 根据unicode生成字体的二值位图

	FT_UInt glyph_index = FT_Get_Char_Index(m_face, wc);
	FT_Load_Glyph(m_face, glyph_index, FT_LOAD_DEFAULT);
	FT_Render_Glyph(m_face->glyph, FT_RENDER_MODE_MONO);

	//

	FT_GlyphSlot slot = m_face->glyph;

	// 行列数

	int rows = slot->bitmap.rows;
	int cols = slot->bitmap.width;

	//

	for (int i = 0; i < rows; ++i)
	{
		for (int j = 0; j < cols; ++j)
		{
			int off = ((img->origin == 0) ? i : (rows - 1 - i))
				* slot->bitmap.pitch + j / 8;

			if (slot->bitmap.buffer[off] & (0xC0 >> (j % 8)))
			{
				int r = (img->origin == 0) ? pos.y - (rows - 1 - i) : pos.y + i;;
				int c = pos.x + j;

				if (r >= 0 && r < img->height
					&& c >= 0 && c < img->width)
				{
					CvScalar scalar = cvGet2D(img, r, c);

					// 进行色彩融合

					float p = m_fontDiaphaneity;
					for (int k = 0; k < 4; ++k)
					{
						scalar.val[k] = scalar.val[k] * (1 - p) + color.val[k] * p;
					}

					cvSet2D(img, r, c, scalar);
				}
			}
		} // end for
	} // end for

	// 修改下一个字的输出位置

	double space = m_fontSize.val[0] * m_fontSize.val[1];
	double sep = m_fontSize.val[0] * m_fontSize.val[2];

	pos.x += (int)((cols ? cols : space) + sep);
}

```

&emsp;&emsp;测试代码：

```
#include <opencv/highgui.h>  
#include <assert.h>  
#include "CvxText.h"  
using namespace cv;  
  
int main(int argc, char *argv[])  
{  
    // 打开一幅  
    IplImage *img = cvLoadImage("lena.jpg");  
    // 输出汉字  
    {  
        CvxText text("simhei.ttf"); // "zenhei.ttf"为黑体常规  
        const char *msg = "在OpenCV中输出汉字！";  
        float p = 0.5;  
        text.setFont(NULL, NULL, NULL, &p);   // 透明处理  
        text.putText(img, msg, cvPoint(100, 150), CV_RGB(255, 0, 0));  
    }  
    // 定义窗口，并显示影象  
    cvShowImage("test", img); cvWaitKey(-1);  
    cvReleaseImage(&img);  
    return 0;  
} 
```

&emsp;&emsp;代码里面的simhei.tff是字体名称，需要拷贝到你程序的执行目录下，当然也可以直接在代码中写字体的路径。window下一般都有这个字体，ubuntu的话，来windows下拷贝一下就好，windows的的字体目录：C:\Windows\Fonts。

## 敲重点

**<font color="red" size=4 face="仿宋">&emsp;&emsp;如果你的代码的编码格式本身就是GBK，那么你就不需要UTF-8转GBK这个过程，即第二个部分就可以略过；但是，如果你进行第三部分的时候，发现部分汉字能显示，部分不能显示，那就是编码问题，需要调用第二部分的UTF-8转GBK的函数。/font>**

----------

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**

----------