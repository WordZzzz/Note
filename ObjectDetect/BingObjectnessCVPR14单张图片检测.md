---
title: "BingObjectnessCVPR14单张图片检测"
comments: true
mathjax: true
categories:
  - BING
tags:
  - BING
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **操作系统：WINDOWS 10**
- **软件版本：VS2013+OpenCV2.4.13**
- **编者: WordZzzz**

----------

&emsp;&emsp;应邀，本人将自己加入单张图片检测的BING代码上传至GITHUB，源码编译及详解请跳转至我的CSDN博客或者私人博客。

源码编译：http://blog.csdn.net/u011475210/article/details/77822669
源码详解：http://blog.csdn.net/u011475210/article/details/77916097

## 主要工作：

&emsp;&emsp;在原有代码的基础上加入单张图片检测框提取代码（Objectness.cpp），大家可以直接在主函数中找到我定义的函数并直接跳转至定义查看即可。

### 检测框提取：

```cpp
/*
name：getObjBndBoxesForSingleImage
function：get boxes from single image
edit by wordzzzz
*/
void Objectness::getObjBndBoxesForSingleImage(Mat img, ValStructVec<float, Vec4i> &finalBoxes, int numDetPerSize)
{
	ValStructVec<float, Vec4i> boxes;
	finalBoxes.reserve(10000);

	int scales[3] = { 1, 3, 5 };
	for (int clr = MAXBGR; clr <= G; clr++)
	{
		setColorSpace(clr);
		loadTrainedModel();
		CmTimer tm("Predict");
		tm.Start();

		getObjBndBoxes(img, boxes, numDetPerSize);
		finalBoxes.append(boxes, scales[clr]);

		tm.Stop();
		printf("Average time for predicting an image (%s) is %gs\n", _clrName[_Clr], tm.TimeInSeconds());
	}

	//Write on file the total number and the list of rectangles returned by objectess, one for each row.

	CStr fName = _bbResDir + "bb";
	FILE *f = fopen(_S(fName + ".txt"), "w");
	fprintf(f, "%d\n", boxes.size());
	printf("boxes.size() is %d \n", boxes.size());
	for (size_t k = 0; k < boxes.size(); k++)
		fprintf(f, "%g, %s\n", boxes(k), _S(strVec4i(boxes[k])));
	fclose(f);

	for (int j = 0; j < boxes.size(); j++)
		finalBoxes[j] = boxes[j];

}
```

### 检测框显示：

```cpp
/*
name：myilluTestReults
function：draw the result
edit by wordzzzz
*/
void Objectness::myilluTestReults(CStr imgPath, const ValStructVec<float, Vec4i> &boxesTests)
{
	CStr resDir = _voc.localDir + "ResIlu2/";
	CmFile::MkDir(resDir);

		const ValStructVec<float, Vec4i> &boxes = boxesTests;
		Mat img = imread(imgPath);//如果想将所有检测框画到一张图片上，请取消该行注释

		for (int k = 0; k < boxes.size(); k++){
			//			Mat img = imread(imgPath);//如果想将所有检测框画到一张图片上，请注释掉该行
			const Vec4i &bb = boxes[k];
			rectangle(img, Point(bb[0], bb[1]), Point(bb[2], bb[3]), Scalar(0), 3);
			rectangle(img, Point(bb[0], bb[1]), Point(bb[2], bb[3]), Scalar(255, 255, 255), 2);
			rectangle(img, Point(bb[0], bb[1]), Point(bb[2], bb[3]), Scalar(0, 0, 255), 1);
			CStr resNameNE = CmFile::GetNameNE(imgPath) + to_string(k);
			//			imwrite(resDir + resNameNE +"_Match.jpg", img);//如果想将所有检测框画到一张图片上，请注释掉该行
		}
		imwrite(resDir + "_Match.jpg", img);//如果想将所有检测框画到一张图片上，请取消该行注释
	
}
```

**<font color="red" size=3 face="仿宋">本教程到此结束，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**