# 《机器学习实战》之朴素贝叶斯（2）使用Python进行文本分类

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/ML/tree/master/Ch04**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言：

&emsp;&emsp;要从文本中获取特征，需要先拆分文本。这里的特征是来自文本的词条（token），一个词条是字符的任意组合。可以把词条想象为单词，也可以使用非单词词条，如URL、IP地址或者任意其他字符串。然后将一个文本片段表示为一个词向量，其中值为1表示词条出现，0表示词条未出现。

&emsp;&emsp;以在线社区的留言板为例，为了不影响社区的发展，我们要屏蔽侮辱性的言论，所以要构建一个快速过滤器，如果某条留言使用了负面或者侮辱性的言语，那么就将该留言表示为内容不当。过滤这类内容是一个很常见的需求。对此问题建立两个类别：侮辱类和非侮辱类，使用1和0分别表示。

&emsp;&emsp;本文主要利用Python实现文本分类。

## 准备数据：从文本中构建词向量

&emsp;&emsp;我们将把文本看成单词向量或者词条向量，也就是说将句子转换为向量。打开文本编辑器，创建一个叫bayes.py的新文件，用如下代码实现构建词向量。

代码实现：

```python
# -*- coding: UTF-8 -*-
"""
Created on Sep 08, 2017
Naive Bayes
@author: wordzzzz
"""

from numpy import *

def loadDataSet():
	"""
	Function：	创建实验样本

	Args：		无

	Returns：	postingList：词条切分后的文档集合
				classVec：类别标签的集合
	"""	
	#词条切分后的文档集合
	postingList=[['my', 'dog', 'has', 'flea', 'problems', 'help', 'please'],
				['maybe', 'not', 'take', 'him', 'to', 'dog', 'park', 'stupid'],
				['my', 'dalmation', 'is', 'so', 'cute', 'I', 'love', 'him'],
				['stop', 'posting', 'stupid', 'worthless', 'garbage'],
				['mr', 'licks', 'ate', 'my', 'steak', 'how', 'to', 'stop', 'him'],
				['quit', 'buying', 'worthless', 'dog', 'food', 'stupid']]
	#类别标签的集合
	classVec = [0,1,0,1,0,1]    #1 is abusive, 0 not
	#词条切分后的文档集合和类别标签结合
	return postingList,classVec

def createVocabList(dataSet):
	"""
	Function：	创建一个包含所有文档中出现的不重复词的列表

	Args：		dataSet：数据集

	Returns：	list(vocabSet)：返回一个包含所有文档中出现的不重复词的列表
	"""
	#创建一个空集
	vocabSet = set([])
	#将新词集合添加到创建的集合中
	for document in  dataSet:
		#操作符 | 用于求两个集合的并集
		vocabSet = vocabSet | set(document)
	#返回一个包含所有文档中出现的不重复词的列表
	return list(vocabSet)

def setOfWords2Vec(vocabList, inputSet):
	"""
	Function：	词表到向量的转换

	Args：		vocabList：词汇表
				inputSet：某个文档

	Returns：	returnVec：文档向量
	"""
	#创建一个所含元素都为0的向量
	returnVec = [0]*len(vocabList)
	#遍历文档中词汇
	for word in inputSet:
		#如果文档中的单词在词汇表中，则相应向量位置置1
		if word in vocabList:
			returnVec[vocabList.index(word)] = 1
		#否则输出打印信息
		else: print("the word: %s is not in my Vocablary!" % word)
	#向量的每一个元素为1或0，表示词汇表中的单词在文档中是否出现
	return returnVec
```

&emsp;&emsp;首先命令行生成词汇表，程序中巧妙地运用了Python的set数据类型，通过并集运算，可以生成一个包含所有文档中出现的不重复词的列表。

输出结果：

```python
>>> import bayes
>>> listOPosts, listClasses = bayes.loadDataSet()
>>> myVocabList = bayes.createVocabList(listOPosts)
>>> myVocabList
['mr', 'cute', 'please', 'to', 'steak', 'worthless', 'not', 'how', 'so', 'I', 'stop', 'ate', 'buying', 'help', 'has', 'maybe', 'dog', 'him', 'flea', 'posting', 'stupid', 'is', 'food', 'garbage', 'take', 'park', 'my', 'quit', 'licks', 'dalmation', 'love', 'problems']
```

&emsp;&emsp;检查上述词表，就会发现这里不会出现重复的单词。目前该词表并没有进行排序，需要的话稍后可以对其排序。
然后看一下setOfWords2Vec的运行效果：

```python
>>> bayes.setOfWords2Vec(myVocabList, listOPosts[0])
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1]
>>> bayes.setOfWords2Vec(myVocabList, listOPosts[3])
[0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0]
```

&emsp;&emsp;该函数使用词汇表或者想要检查的所有单词作为输入，然后为其中每一个单词构建一个特征。我们来看对listOPosts[0]（['my', 'dog', 'has', 'flea', 'problems', 'help', 'please']）进行词条转换输出的结果，可以看到第三个元素值为1，即词汇表中对应的please这个单词在listOPosts[0]中，事实也确实如此。

## 训练算法：从词向量计算概率

&emsp;&emsp;前面介绍了如何将一组单词转换为一组数字，现在我们就开始使用这些数字计算概率。我们把上篇文章的贝叶斯准则再掏出来，讲之前的x、y替换为w。w表示这是一个向量，即它由多个数值组成。在这个例子中，数值个数与词汇表中的词个数相同。

`!$ P(c_i | w) = \frac {P(w | c_i)P(c_i)} {P(w)} $`

&emsp;&emsp;使用上述公式，对每个类计算该值，然后比较这两个概率值的大小。首先通过类别i（侮辱性留言或者非侮辱性留言）中文档数除以总的文档数来计算概率`!$ P(c_i) $`。接下来计算`!$ P(w | c_i) $`，这里就用到了贝叶斯假设。如果将w展开为一个个独立特征，那么就可以将上述概率写作`!$P(w_0, w_1, w_2, ···w_N|c_i)$`。这里假设所有词都互相独立，就是我们之前提到的naive的条件独立性假设，这意味着我们可以使用`!$P(w_0|c_i)P(w_1|c_i)P(w_2|c_i)...P(w_N|c_i)$`来计算上述概率，就算起来so easy!

&emsp;&emsp;写个伪代码大概就是这么个意思：

```
计算每个类别中的文档数目
对每篇训练文档：
    对每个类别：
        如果词条出现在文档中，则增加该词条的计数
        增加所有词条的计数值
对每个类别：
    对每个词条：
        将该词条的数目除以总词条数目得到条件概率
返回每个类别的条件概率
```

&emsp;&emsp;下面的代码使用了NumPy的一些函数，故应确保将from numpy import *语句添加到bayes.py文件的最前面。

代码实现：

```python
def trainNB0(trainMatrix, trainCategory):
	"""
	Function：	朴素贝叶斯分类器训练函数

	Args：		trainMatrix：文档矩阵
				trainCategory：类别标签向量

	Returns：	p0Vect：非侮辱性词汇概率向量
				p1Vect：侮辱性词汇概率向量
				pAbusive：侮辱性文档概率
	"""
	#获得训练集中文档个数
	numTrainDocs = len(trainMatrix)
	#获得训练集中单词个数
	numWords = len(trainMatrix[0])
	#计算文档属于侮辱性文档的概率
	pAbusive = sum(trainCategory)/float(numTrainDocs)
	#初始化概率的分子变量
	p0Num = zeros(numWords); p1Num = zeros(numWords)
	#初始化概率的分母变量
	p0Denom = 0.0; p1Denom = 0.0
	#遍历训练集trainMatrix中所有文档
	for i in range(numTrainDocs):
		#如果侮辱性词汇出现，则侮辱词汇计数加一，且文档的总词数加一
		if trainCategory[i] ==1:
			p1Num += trainMatrix[i]
			p1Denom += sum(trainMatrix[i])
		#如果非侮辱性词汇出现，则非侮辱词汇计数加一，且文档的总词数加一
		else:
			p0Num += trainMatrix[i]
			p0Denom += sum(trainMatrix[i])
	#对每个元素做除法求概率
	p1Vect = p1Num/p1Denom
	p0Vect = p0Num/p0Denom
	#返回两个类别概率向量和一个概率
	return p0Vect, p1Vect, pAbusive
```

输出结果：

```python
>>> reload(bayes)
<module 'bayes' from 'E:\\机器学习实战\\mycode\\Ch04\\bayes.py'>
>>> listOPosts, listClasses = bayes.loadDataSet()       #从预先加载值中调入数据
>>> myVocabList = bayes.createVocabList(listOPosts)     #构建一个包含所有值得词汇表
>>> trainMat = []                                       #利用for循环来填充trainMat列表
>>> for postinDoc in listOPosts:
...     trainMat.append(bayes.setOfWords2Vec(myVocabList, postinDoc))
...
>>> p0V, p1V, pAb = bayes.trainNB0(trainMat, listClasses)
>>> pAb                                                 #任意侮辱性文档的概率
0.5
>>> p0V
array([ 0.04166667,  0.04166667,  0.04166667,  0.04166667,  0.04166667,
        0.        ,  0.        ,  0.04166667,  0.04166667,  0.04166667,
        0.04166667,  0.04166667,  0.        ,  0.04166667,  0.04166667,
        0.        ,  0.04166667,  0.08333333,  0.04166667,  0.        ,
        0.        ,  0.04166667,  0.        ,  0.        ,  0.        ,
        0.        ,  0.125     ,  0.        ,  0.04166667,  0.04166667,
        0.04166667,  0.04166667])
>>> p1V
array([ 0.        ,  0.        ,  0.        ,  0.05263158,  0.        ,
        0.10526316,  0.05263158,  0.        ,  0.        ,  0.        ,
        0.05263158,  0.        ,  0.05263158,  0.        ,  0.        ,
        0.05263158,  0.10526316,  0.05263158,  0.        ,  0.05263158,
        0.15789474,  0.        ,  0.05263158,  0.05263158,  0.05263158,
        0.05263158,  0.        ,  0.05263158,  0.        ,  0.        ,
        0.        ,  0.        ])
```

&emsp;&emsp;首先，我们发现文档属于侮辱类的概率pAb为0.5，该值是正确定。词汇表中的第一个词是cute，其在类别0中出现一次，在类别1中从未出现，对应的条件概率分别为0.44166667和0.0。该计算是正确的。

## 测试算法：根据现实情况修改分类器

&emsp;&emsp;利用贝叶斯分类器对文档进行分类时，要计算过个概率的乘积以获得文档属于某个类别的概率，如果其中一个概率值为0，那么最后的乘积也为0。为降低这种影响，可以将所有词的出现数初始化为1，并将分母初始化为2。所以需要修改一下trainNB()中的分母分子初始化代码。

```
	#初始化概率的分子变量
	p0Num = ones(numWords); p1Num = ones(numWords)
	#初始化概率的分母变量
	p0Denom = 2.0; p1Denom = 2.0
```

&emsp;&emsp;另一个遇到的问题就是下溢出，太多的很小数相乘，导致程序向下溢出或者得不到正确的答案（比如四舍五入后乘积为0）。一种解决办法就是对乘积取自然对数。即`!$ln(a*b) = ln(a) + ln(b)$`，于是通过求对数可以避免下溢出或者浮点数舍入导致的错误。我们可以放心的是，采用自然对数处理不会有任何损失。下图给出了函数f(x)和ln(f(x))的曲线。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170910153516173?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600" height="400" /></div>
<p></p>

&emsp;&emsp;检查这两条曲线，就会发现他们在相同区域内同时增加或者减少，并且在相同点上取到极值。取值虽然不同，但是不影响最终结果。所以需要修改一下trainNB()中的求概率代码。

```
	#对每个元素做除法求概率
	p1Vect = log(p1Num/p1Denom)
	p0Vect = log(p0Num/p0Denom)
```

&emsp;&emsp;下面构建朴素贝叶斯分类函数和测试函数：

代码实现：

```python
def classifyNB(vec2Classify, p0Vec, p1Vec, pClass1):
	"""
	Function：	朴素贝叶斯分类函数

	Args：		vec2Classify：文档矩阵
				p0Vec：非侮辱性词汇概率向量
				p1Vec：侮辱性词汇概率向量
				pClass1：侮辱性文档概率

	Returns：	1：侮辱性文档
				0：非侮辱性文档
	"""
	#向量元素相乘后求和再加到类别的对数概率上，等价于概率相乘
	p1 = sum(vec2Classify * p1Vec) + log(pClass1)
	p0 = sum(vec2Classify * p0Vec) + log(1.0 - pClass1)
	#分类结果
	if p1 > p0:
		return 1
	else:
		return 0

def testingNB():
	"""
	Function：	朴素贝叶斯分类器测试函数

	Args：		无

	Returns：	testEntry：测试词汇列表
				classifyNB(thisDoc, p0V, p1V, pAb)：分类结果
	"""
	#从预先加载中调入数据
	listOPosts, listClasses = loadDataSet()
	#构建一个包含所有词的列表
	myVocabList = createVocabList(listOPosts)
	#初始化训练数据列表
	trainMat = []
	#填充训练数据列表
	for postinDoc in listOPosts:
		trainMat.append(setOfWords2Vec(myVocabList, postinDoc))
	#训练
	p0V, p1V, pAb = trainNB0(trainMat, listClasses)
	#测试
	testEntry = ['love', 'my', 'dalmation']
	thisDoc = array(setOfWords2Vec(myVocabList, testEntry))
	print(testEntry,'classified as: ', classifyNB(thisDoc, p0V, p1V, pAb))
	#测试
	testEntry = ['stupid', 'garbage']
	thisDoc = array(setOfWords2Vec(myVocabList, testEntry))
	print(testEntry,'classified as: ', classifyNB(thisDoc, p0V, p1V, pAb))
```

输出结果：

```python
>>> reload(bayes)
<module 'bayes' from 'E:\\机器学习实战\\mycode\\Ch04\\bayes.py'>
>>> bayes.testingNB()
['love', 'my', 'dalmation'] classified as:  0
['stupid', 'garbage'] classified as:  1
```

&emsp;&emsp;大家可以对输入文本做一下修改，看看分类器会输出什么结果。这个例子非常简单，但是它展示了朴素贝叶斯分类器的工作原理。接下来我们可以对代码做一些修改，使分类器工作得更好。

## 准备数据：文档词袋模型

&emsp;&emsp;我们将每个词的出现与否作为一个特征，可以被描述为词集模型（set-of-words model）。如果一个词在文档中出现不止一次，这可能意味着包含该词是否出现在文档中所不能表达的某些信息，这种方法被称为词袋模型（bag-of-words model）。词袋中的单词可以出现多次，而在词集中，每个单词只能出现一次。下面的程序给出了基于词袋模型的朴素贝叶斯代码，。它与函数setOfWords2Vec()几乎完全相同，唯一不同的是每当遇到一个单词时，它会增加词向量中的对应值，而不只是将对应的数值设为1。

代码实现：

```python
def bagOfWords2VecMN(vocabList, inputSet):
	"""
	Function：	词袋到向量的转换

	Args：		vocabList：词袋
				inputSet：某个文档

	Returns：	returnVec：文档向量
	"""
	#创建一个所含元素都为0的向量
	returnVec = [0]*len(vocabList)
	#将新词集合添加到创建的集合中
	for word in inputSet:
		#如果文档中的单词在词汇表中，则相应向量位置加1
		if word in vocabList:
			returnVec[vocabList.index(word)] += 1
	#返回一个包含所有文档中出现的词的列表
	return returnVec
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**