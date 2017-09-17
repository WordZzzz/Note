# 《机器学习实战》之Logistic回归（2）最佳回归系数确定

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/ML/tree/master/Ch05**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言：

&emsp;&emsp;Sigmoid函数的输入记为z，有下面公式得出：

`!$z = w_0x_0 + w_1x_1 + w_2x_2 +···+ w_nx_n$`

&emsp;&emsp;如果采用向量的写法，上述公式可以写成`!$z = w^Tx$`，它表示将这两个数值向量对应元素相乘然后全部加起来即得到z值。其中的向量x是分类器的输入数据，向量w也就是我们要找到的最佳参数系数，从而使得分类器尽可能的精确。为了寻找最佳参数，我们需要用到最优化理论的一些知识。

&emsp;&emsp;下面首先介绍梯度上升这一优化算法，我们将学习到如何使用该方法求得数据集的最佳参数。接下来，展示如何绘制梯度上升法产生的决策变截图，该图能将梯度上升法的分类效果可视化地呈现出来。最后，我们将学习随机梯度上升算法，以及如何对其进行修改以获得更好的效果。

## 梯度上升算法

&emsp;&emsp;梯度上升算法基于的思想是：要找到某函数的最大值，最好的办法就是沿着该函数的梯度方向探寻。如果梯度记为`!$\nabla$`，则函数f(x,y)的梯度由下式表示：

`!$\nabla f(x,y) = \begin{pmatrix} \frac {\partial f(x,y)} {\partial x} \\ \frac {\partial f(x,y)} {\partial y} \\ \end{pmatrix}$`

&emsp;&emsp;这是机器学习中最容易造成混淆的一个地方，但在数学上并不难，需要做的只是牢记这些符号的含义。这个梯度意味着要沿x的方向移动`!$\frac {\partial f(x,y)} {\partial x}$`，沿y的方向移动`!$\frac {\partial f(x,y)} {\partial y}$`。其中，函数f(x,y)必须要在待计算的点上有定义并且可微。一个具体的函数例子见下图。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195011708?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;图中的梯度上升算法沿梯度方向移动了一步。乐意看到，梯度算子总是指向函数值增长最快的方向。这里说到移动方向，而未提及移动量的大小。该量值称为步长，记作α。用向量来表示的话，梯度上升算法的迭代公式如下：

`!$w: = w + \alpha \nabla_w f(w)$`

&emsp;&emsp;该公式将一直被迭代执行，直到达到某个停止条件为止，比如设定的迭代次数或者达到某个允许的误差范围。

&emsp;&emsp;如果我没记错的话，Andrew在course的machine learning第三周的课程中使用的是梯度下降算法，它的公式为：

`!$w: = w - \alpha \nabla_w f(w)$`

&emsp;&emsp;我们可以看到，两个算法其实是一样的，只是公式中的加减法不同而已。梯度上升算法用来求函数的最大值，而梯度下降算法用来求函数的最小值。

## 训练算法：使用梯度上升找到最佳参数

&emsp;&emsp;基于上面的内容，我们来看一个Logistic回归分类器的应用例子，从下图可以看到我们采集的数据集。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195117794?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;上图中有100个样本点，每个点包含两个数值型特征：X1和X2。在此数据集上，我们将通过使用梯度上升法找到最佳回归系数，也就是拟合出Logistic回归模型的最佳参数。

&emsp;&emsp;梯度上升法的伪代码如下：

```
每个回归系数初始化为1
重复R次：
    计算整个数据集的梯度
    使用alpha 下gradient 更新回归系数的向量
返回回归系数
```

&emsp;&emsp;下面的代码是梯度上升算法的具体实现。

```python
# -*- coding: UTF-8 -*- #
'''
Created on Sep 15, 2017
Logistic Regression Working Module
@author: WordZzzz
'''
from numpy import *

def loadDataSet():
	"""
	Function：	加载数据集

	Input：		testSet.txt：数据集

	Output：	dataMat：数据矩阵100*3
				labelMat：类别标签矩阵1*100
	"""	
	#初始化数据列表和标签列表
	dataMat = []; labelMat = []
	#打开数据集
	fr = open('testSet.txt')
	#遍历每一行
	for line in fr.readlines():
		#删除空白符之后进行切分
		lineArr = line.strip().split()
		#数据加入数据列表
		dataMat.append([1.0, float(lineArr[0]), float(lineArr[1])])
		#标签加入数据列表
		labelMat.append(int(lineArr[2]))
	#返回数据列表和标签列表
	return dataMat, labelMat

def sigmoid(inX):
	"""
	Function：	sigmoid函数

	Input：		inX：矩阵运算结果100*1

	Output：	1.0/(1+exp(-inX))：计算结果
	"""	
	#返回计算结果
	return 1.0/(1+exp(-inX))

def gradAscent(dataMatIn, classLabels):
	"""
	Function：	logistic回归梯度上升函数

	Input：		dataMatIn：数据列表100*3
				classLabels：标签列表1*100

	Output：	weights：权重参数矩阵
	"""	
	#转换为numpy矩阵dataMatrix：100*3
	dataMatrix = mat(dataMatIn)
	#转换为numpy矩阵并转置为labelMat：100*1
	labelMat = mat(classLabels).transpose()
	#获得矩阵行列数
	m,n = shape(dataMatrix)
	#初始化移动步长
	alpha = 0.001
	#初始化地带次数
	maxCycles = 500
	#初始化权重参数矩阵，初始值都为1
	weights = ones((n,1))
	#开始迭代计算参数
	for k in range(maxCycles):
		#100*3 * 3*1 => 100*1
		h = sigmoid(dataMatrix * weights)
		#计算误差100*1
		error = (labelMat - h)
		#更新参数值
		weights = weights + alpha * dataMatrix.transpose() * error
	#返回权重参数矩阵
	return weights
```

&emsp;&emsp;需要强调的是，gradAscent函数中的for循环部分中的运算是矩阵运算。变量h不是一个数而是一个列向量，列向量的元素个数等于样本个数，这里是100.对应的运算dataMatrix * weights代表不止一次乘积计算，实际上该运算包含了300次的乘积。

&emsp;&emsp;接下来我们看看实际效果，打开终端，输入命令行：

```python
>>> import logRegres
>>> dataArr, labelMat = logRegres.loadDataSet()
>>> logRegres.gradAscent(dataArr, labelMat)
matrix([[ 4.12414349],
        [ 0.48007329],
        [-0.6168482 ]])
```

## 分析数据：画出决策边界

&emsp;&emsp;分析数据，当然离不开可视化模块matplotlib，代码实现如下：

```python
def plotBestFit(weights):
	"""
	Function：	画出数据集和最佳拟合直线

	Input：		weights：权重参数矩阵

	Output：	包含数据集和拟合直线的图像
	"""	
	#加载matplotlib中的pyplot模块
	import matplotlib.pyplot as plt
	#导入数据
	dataMat, labelMat = loadDataSet()
	#创建数组
	dataArr =array(dataMat)
	#获取数组行数
	n = shape(dataArr)[0]
	#初始化坐标
	xcord1 = []; ycord1 = []
	xcord2 = []; ycord2 = []
	#遍历每一行数据
	for i in range(n):
		#如果对应的类别标签对应数值1，就添加到xcord1，ycord1中
		if int(labelMat[i]) == 1:
			xcord1.append(dataArr[i,1]); ycord1.append(dataArr[i,2])
		#如果对应的类别标签对应数值0，就添加到xcord2，ycord2中
		else:
			xcord2.append(dataArr[i,1]); ycord2.append(dataArr[i,2])
	#创建空图
	fig = plt.figure()
	#添加subplot，三种数据都画在一张图上
	ax = fig.add_subplot(111)
	#1类用红色标识，marker='s'形状为正方形
	ax.scatter(xcord1, ycord1, s=30, c='red', marker='s')
	#0类用绿色标识，弄认marker='o'为圆形
	ax.scatter(xcord2, ycord2, s=30, c='green')
	#设置x取值，arange支持浮点型
	x = arange(-3.0, 3.0, 0.1)
	#配计算y的值
	y = (-weights[0]-weights[1]*x)/weights[2]
	#画拟合直线
	ax.plot(x, y)
	#贴坐标表头
	plt.xlabel('X1'); plt.ylabel('X2')
	#显示结果
	plt.show()
```

&emsp;&emsp;打开终端，输入命令行进行测试：

```python
>>> from imp import reload
>>> reload(logRegres)
<module 'logRegres' from 'logRegres.py'>
>>> weights = logRegres.gradAscent(dataArr, labelMat)
>>> logRegres.plotBestFit(weights.getA())
```

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195213466?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;这个分类效果相当不错，从图上看之分错了两到四个点。但是，尽管例子简单并且数据集很小，这个方法却很需要大量的计算（300次乘积）。下面我们将对该算法进行改进，从而使它可以用到真实数据上。

## 训练算法：随机梯度上升

&emsp;&emsp;梯度上升算法在每次更新回归系数时都需要遍历整个数据集，计算复杂度太高了。一种改进方法就是一次仅用一个样本点来更新回归系数，该方法称为随机梯度上升算法。由于可以在新样本到来时对分类器进行增量式更新，因而随机梯度上升算法是一个在线学习方法。与“在线学习”相对应的，一次处理所有数据被称为是“批处理”。

&emsp;&emsp;随机梯度上升算法伪代码如下：

```
所有回归系数初始化为1
对数据集中每个样本
    计算该样本的梯度
    使用alpha x gradient 更新回归系数值
返回回归系数值
```

&emsp;&emsp;代码实现如下：

```python
def stocGradAscent0(dataMatrix, classLabels):
	"""
	Function：	随机梯度上升算法

	Input：		dataMatIn：数据列表100*3
				classLabels：标签列表1*100

	Output：	weights：权重参数矩阵
	"""	
	#获取数据列表大小
	m,n = shape(dataMatrix)
	#步长设置为0.01
	alpha = 0.01
	#初始化权重参数矩阵，初始值都为1
	weights = ones(n)
	#遍历每一行数据
	for i in range(m):
		#1*3 * 3*1
		h = sigmoid(sum(dataMatrix[i]*weights))
		#计算误差
		error = classLabels[i] - h
		#更新权重值
		weights = weights + alpha * error * dataMatrix[i]
	#返回权重参数矩阵
	return weights
```

&emsp;&emsp;可以看到，随机梯度上升算法与梯度上升算法在代码上很相似，但也有一些区别：第一，后者的变量h和误差error都是向量，而前者则全是数值；第二，前者没有矩阵的转换过程，所有变量的数据类型都是Numpy数组。

```python
>>> from numpy import *
>>> reload(logRegres)
<module 'logRegres' from 'logRegres.py'>
>>> dataArr, labelMat = logRegres.loadDataSet()
>>> weights=logRegres.stocGradAscent0(array(dataArr), labelMat)
>>> logRegres.plotBestFit(weights)
```

&emsp;&emsp;执行完毕之后，效果如下图所示，该图与上一张图有一些相似之处。可以看到，拟合出来的直线比上次差了点，但是你要知道，这次的随机梯度上升算法，我们只是在整个数据集上迭代了一次而已（算成100次也是和500次差很多）。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195237593?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;一个判断优化算法优劣的可靠算法是看它是否收敛，也就是说参数是否达到了稳定值。随机梯度上升算法迭代200次，三个回归系数的变化如下图所示。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195307000?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;从上图中我们可以看到，X2迭代50次左右就稳定了，但是其他两个则需要过多次的迭代。同时我们发现在大的波动停止之后，还会有一些小的周期性波动，产生这种现象的原因是因为存在一些不能正确分类的样本点（数据集并非线性可分），在每次迭代时会引发系数的剧烈改变。我们期望算法能避免来回波动，并且收敛也要足够快。

## 改进的随机梯度上升算法

&emsp;&emsp;先上代码：

```python
def stocGradAscent1(dataMatrix, classLabels, numIter=150):
	"""
	Function：	改进的随机梯度上升算法

	Input：		dataMatIn：数据列表100*3
				classLabels：标签列表1*100
				numIter：迭代次数

	Output：	weights：权重参数矩阵
	"""	
	#获取数据列表大小
	m, n = shape(dataMatrix)
	#初始化权重参数矩阵，初始值都为1
	weights = ones(n)
	#开始迭代，迭代次数为numIter
	for j in range(numIter):
		#初始化index列表，这里要注意将range输出转换成list
		dataIndex = list(range(m))
		#遍历每一行数据，这里要注意将range输出转换成list
		for i in list(range(m)):
			#更新alpha值，缓解数据高频波动
			alpha = 4/(1.0+j+i)+0.0001
			#随机生成序列号，从而减少随机性的波动
			randIndex = int(random.uniform(0, len(dataIndex)))
			#序列号对应的元素与权重矩阵相乘，求和后再求sigmoid
			h = sigmoid(sum(dataMatrix[randIndex]*weights))
			#求误差，和之前一样的操作
			error = classLabels[randIndex] - h
			#更新权重矩阵
			weights = weights + alpha * error * dataMatrix[randIndex]
			#删除这次计算的数据
			del(dataIndex[randIndex])
	#返回权重参数矩阵
	return weights
```

&emsp;&emsp;改进：

- 一方面，alpha在每次迭代的时候都会调整，这会缓解上一张图中的数据高频波动。另外，虽然alpha会随着迭代次数不断减小，但永远不会减小到0，这是因为alpha更新公式中存在一个常数项，必须这样做的原因是为了保证在多次迭代之后新数据仍然具有一定得影响。如果要处理的问题是动态变化的，那么可以适当加大上述常数项，来确保新的值获得更大的回归系数。另一点值得注意的是，在降低alpha的函数中，alpha每次减少i/(j+i)时，alpha就不是严格下降的。便面参数的严格下降也常见于模拟退火算法等其他优化算法中。
- 另一方面，通过随机选取样本来更新回归系数，可以减少周期性的波动。

&emsp;&emsp;同样的，下图反映了stocGradAscent1函数中，每次迭代时各个回归系数的变化情况。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195327393?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;我们可以看到，除了周期性波动的减少，上图的水平轴也少了很多（迭代了40次），这是因为该函数可以收敛的更快。

```python
>>> reload(logRegres)
<module 'logRegres' from 'logRegres.py'>
>>> dataArr, labelMat = logRegres.loadDataSet()
>>> weights=logRegres.stocGradAscent1(array(dataArr), labelMat)
>>> logRegres.plotBestFit(weights)
```

&emsp;&emsp;分类效果如图所示：

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170917195345825?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

&emsp;&emsp;迄今为止我们分析了回归系数的变化情况，但还没有达到本章的最终目标，即完成具体的分类任务。下一届将使用随机梯度上升算法来解决病马的生死预测问题。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**