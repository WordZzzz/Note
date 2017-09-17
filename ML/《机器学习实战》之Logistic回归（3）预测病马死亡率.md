# 《机器学习实战》之Logistic回归（3）预测病马死亡率

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/ML/tree/master/Ch05**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言：

&emsp;&emsp;本节将使用Logistic会归来预测还有疝病的马的存货问题。智力的数据包含368个样本和28个特征。疝病逝描述马胃肠痛的术语，然而这种病不一定来源于马的肠胃问题，其他问题也可能引发该病。

&emsp;&emsp;示例：使用Logistic回归估计马疝病的死亡率

- 收集数据：给定数据文件。
- 准备数据：用Python解析文本文件并填充缺失值。
- 分析数据：可视化并观察数据。
- 训练算法：使用优化算法，找到最佳的系数。
- 测试算法：为了量化回归效果，需要观察错误率，根据错误率决定是否回退到训练阶段，通过改变迭代的次数和步长等参数来得到更好的回归系数。
- 使用算法：实现一个简单的命令行程序来收集马的症状并输出预测结果并非难事，这可以作为留给读者的一道习题。

&emsp;&emsp;另外需要说明的是，除了部分指标主观和难以测量外，该数据还存在一个问题，数据集中有30%的值是缺失的。下面将首先介绍如何处理数据集中的数据缺失问题，然后利用Logistic回归和随机梯度上升算法来预测病马的生死。

## 准备数据；处理数据中的缺失值

&emsp;&emsp;数据中的缺失值是个非常棘手的问题，有很多文献都致力于解决这个问题。下面给出一些可选的做法：

- [ ] 使用可用特征的均值来填补缺失值；
- [ ] 使用特殊值来填补缺失值，如-1；
- [ ] 忽略所有缺失的样本；
- [ ] 使用相似样本的均值添补缺失值；
- [ ] 使用另外的机器学习算法预测缺失值。

&emsp;&emsp;现在，我们对后面用到的数据集进行预处理，使其可以顺利地使用分类算法。在预处理阶段需要做两件事：第一，所有的缺失值必须用一个实数值来替换，因为我们使用的Numpy数组不允许包含缺失值。这里选择实数0来替换所有缺失值，恰好能适用于Lodistic回归。这样做的原因在于，我们需要一个在更新时不会影响系数的值。回归系数的更新公式如下：

`!$weights = weights + alpha * error *dataMatrix[ranIndex]$`

&emsp;&emsp;很明显，如果dataMatrix的某特征对应值为0，那么该特征的系数将不做更新，即`!$weights = weights$`。另外，由于sigmoid(0)=0.5对结果的预测不具有任何倾向性，因为上述做法也不会对误差项造成任何影响，完美！

&emsp;&emsp;预处理的第二件事是，如果发现一条数据的类别标签已经缺失，纳闷我们的简单做法就是将该条数据丢弃。这是因为类别标签和特征不同，很难确定采用某个合适的值来替换。采用Logistic回归进行分类时，这种做法是合理的，而如果采用类似kNN的方法就可能不太可行。

&emsp;&emsp;原始数据经过预处理之后保存成两个文件：horseColicTest.txt和horseColicTraining.txt。如果相对原始数据和预处理数据进行比较，可以在http://archive.ics.uci.edu/ml/dataset/Horse+Colic浏览这些数据。

## 预测算法：用Logistic回归进行分类

&emsp;&emsp;使用Logistic回归方法进行分类并不需要做很多工作，所需要做的只是把测试集上每个特征向量乘以最优化方法得来的回归系数，再将该乘积结果求和，最后输入到Sigmoid函数即可。实现代码如下：

```python
def classifyVector(inX, weights):
	"""
	Function：	分类函数

	Input：		inX：计算得出的矩阵100*1
				weights：权重参数矩阵

	Output：	分类结果
	"""	
	#计算sigmoid值
	prob = sigmoid(sum(inX*weights))
	#返回分类结果
	if prob > 0.5:
		return 1.0
	else:
		return 0.0

def colicTest():
	"""
	Function：	训练和测试函数

	Input：		训练集和测试集文本文档

	Output：	分类错误率
	"""	
	#打开训练集
	frTrain = open('horseColicTraining.txt')
	#打开测试集
	frTest = open('horseColicTest.txt')
	#初始化训练集数据列表
	trainingSet = []
	#初始化训练集标签列表
	trainingLabels = []
	#遍历训练集数据
	for line in frTrain.readlines():
		#切分数据集
		currLine = line.strip().split('\t')
		#初始化临时列表
		lineArr = []
		#遍历21项数据重新生成列表，因为后面格式要求，这里必须重新生成一下。
		for i in range(21):
			lineArr.append(float(currLine[i]))
		#添加数据列表
		trainingSet.append(lineArr)
		#添加分类标签
		trainingLabels.append(float(currLine[21]))
	#获得权重参数矩阵
	trainWeights = stocGradAscent1(array(trainingSet), trainingLabels, 500)
	#初始化错误分类计数
	errorCount = 0

	numTestVec = 0.0
	#遍历测试集数据
	for line in frTest.readlines():
		#
		numTestVec += 1.0
		#切分数据集
		currLine =line.strip().split('\t')
		#初始化临时列表
		lineArr = []
		#遍历21项数据重新生成列表，因为后面格式要求，这里必须重新生成一下。
		for i in range(21):
			lineArr.append(float(currLine[i]))
		#如果分类结果和分类标签不符，则错误计数+1
		if int(classifyVector(array(lineArr), trainWeights)) != int(currLine[21]):
			errorCount += 1
	#计算分类错误率
	errorRate = (float(errorCount)/numTestVec)
	#打印分类错误率
	print("the error rate of this test is: %f" % errorRate)
	#返回分类错误率
	return errorRate

def multiTest():
	"""
	Function：	求均值函数

	Input：		无

	Output：	十次分类结果的平均值
	"""	
	#迭代次数
	numTests = 10
	#初始错误率和
	errorSum = 0.0
	#调用十次colicTest()，累加错误率
	for k in range(numTests):
		errorSum += colicTest()
	#打印平均分类结果
	print("after %d iterations the average error rate is: %f" % (numTests, errorSum/float(numTests)))
```

&emsp;&emsp;打开终端，输入命令行进行测试：

```python
>>> reload(logRegres)
<module 'logRegres' from 'E:\\机器学习实战\\mycode\\Ch05\\logRegres.py'>
>>> logRegres.multiTest()
the error rate of this test is: 0.358209
the error rate of this test is: 0.402985
the error rate of this test is: 0.388060
the error rate of this test is: 0.328358
the error rate of this test is: 0.388060
the error rate of this test is: 0.313433
the error rate of this test is: 0.283582
the error rate of this test is: 0.313433
the error rate of this test is: 0.298507
the error rate of this test is: 0.343284
after 10 iterations the average error rate is: 0.341791
```

&emsp;&emsp;从上面的结果可以看出，10次迭代之后的平均错误率为35%左右。事实上，这个结果并不差，因为哦我们有30%的数据缺失。当然，如果调整colicTest中迭代次数和stocGradAscent1中的步长，平均错误率科一降到20%左右。

&emsp;&emsp;对于这一章的内容，还计划写一篇博文《机器学习实战》之Logistic回归（4）Logisitc回归与最大熵模型，主要是公式推导，预计两周后更新。《机器学习实战》的Python3代码实现还是每周日进行更新，相应章节对应的公式推导和拓展预计有两周延迟。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**