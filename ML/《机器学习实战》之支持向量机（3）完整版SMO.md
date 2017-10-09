# 《机器学习实战》之支持向量机（3）完整版SMO

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/ML/tree/master/Ch06**
- **操作系统：WINDOWS 10**
- **软件版本：python-3.6.2-amd64**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

&emsp;&emsp;在小规模数据集上，上一篇文章中的简化版SMO是没有问题的，但是在更大的数据集上，运行速度就会变慢。

&emsp;&emsp;完整版SMO和简化版SMO，实现alpha的更改个代数运算的优化环节一模一样。在优化过程中，唯一的不同就是选择alpha的方式。完整版的SMO算法应用了一些能够提速的启发方法。

&emsp;&emsp;Platt SMO算法通过一个外循环来选择第一个alpha，并且其选择过程会在两种方式之间进行切换：一种是在所有数据集上进行单遍扫描，另一种则是在非边界alpha（不等于边界0或C的alpha值）中实现单遍扫描。对整个数据集的扫描很容易，前面已经实现了，而实现非边界alpha值的扫描时，需要建立这些alpha值得列表，然后再对这个表进行遍历。同时，该步骤会跳过那些已知的不会改变的alpha值。

&emsp;&emsp;在选择第一个alpha值之后，算法会通过一个内循环来选择第二个alpha。在优化过程中，会通过最大化步长的方式来获得第二个alpha值。在简化版SMO算法中，我们会在选择j之后计算错误率Ej。但在这里，我们会建立一个全局的缓存用于保存误差值，并从中选择使得步长或者Ei-Ej最大的alpha值。

## 支持函数

&emsp;&emsp;和简化版一样，完整版也需要一些支持函数。

- 首要的事情就是建立一个数据结构来保存所有的重要值，而这个过程可以通过一个对象来完成；
- 对于给定的alpha值，第一个辅助函数calcEk()能够计算E值并返回（因为调用频繁，所以必须要单独拎出来）；
- selectJ()用于选择第二个alpha或者说内循环的alpha值，选择合适的值以保证在每次优化中采用最大步长；
- updateEk()用于计算误差值并将其存入缓存中。

```python
'''#######********************************
Non-Kernel VErsions below
'''#######********************************

class optStruct:
	"""
	Function：	存放运算中重要的值

	Input：		dataMatIn：数据集
				classLabels：类别标签
				C：常数C
				toler：容错率

	Output：	X：数据集
				labelMat：类别标签
				C：常数C
				tol：容错率
				m：数据集行数
				b：常数项
				alphas：alphas矩阵
				eCache：误差缓存
	"""	
	def __init__(self, dataMatIn, classLabels, C, toler):
		self.X = dataMatIn
		self.labelMat = classLabels
		self.C = C
		self.tol = toler
		self.m = shape(dataMatIn)[0]
		self.alphas = mat(zeros((self.m, 1)))
		self.b = 0
		self.eCache = mat(zeros((self.m, 2)))

def calcEk(oS, k):
	"""
	Function：	计算误差值E

	Input：		oS：数据结构
				k：下标

	Output：	Ek：计算的E值
	"""	
	#计算fXk，整个对应输出公式f(x)=w`x + b
	fXk = float(multiply(oS.alphas, oS.labelMat).T * (oS.X * oS.X[k,:].T)) + oS.b	
	#计算E值
	Ek = fXk - float(oS.labelMat[k])
	#返回计算的误差值E
	return Ek

def selectJ(i, oS, Ei):
	"""
	Function：	选择第二个alpha的值

	Input：		i：第一个alpha的下标
				oS：数据结构
				Ei：计算出的第一个alpha的误差值

	Output：	j：第二个alpha的下标
				Ej：计算出的第二个alpha的误差值
	"""	
	#初始化参数值
	maxK = -1; maxDeltaE = 0; Ej = 0
	#构建误差缓存
	oS.eCache[i] = [1, Ei]
	#构建一个非零列表，返回值是第一个非零E所对应的alpha值，而不是E本身
	validEcacheList = nonzero(oS.eCache[:, 0].A)[0]
	#如果列表长度大于1，说明不是第一次循环
	if (len(validEcacheList)) > 1:
		#遍历列表中所有元素
		for k in validEcacheList:
			#如果是第一个alpha的下标，就跳出本次循环
			if k == i: continue
			#计算k下标对应的误差值
			Ek = calcEk(oS, k)
			#取两个alpha误差值的差值的绝对值
			deltaE = abs(Ei - Ek)
			#最大值更新
			if (deltaE > maxDeltaE):
				maxK = k; maxDeltaE = deltaE; Ej = Ek
		#返回最大差值的下标maxK和误差值Ej
		return maxK, Ej
	#如果是第一次循环，则随机选择alpha，然后计算误差
	else:
		j = selectJrand(i, oS.m)
		Ej = calcEk(oS, j)
	#返回下标j和其对应的误差Ej
	return j, Ej

def updateEk(oS, k):
	"""
	Function：	更新误差缓存

	Input：		oS：数据结构
				j：alpha的下标

	Output：	无
	"""	
	#计算下表为k的参数的误差
	Ek = calcEk(oS, k)
	#将误差放入缓存
	oS.eCache[k] = [1, Ek]
```

## 优化例程

&emsp;&emsp;接下来简单介绍一下用于寻找决策边界的优化例程。

&emsp;&emsp;大部分代码和之前的smoSimple()是一样的，区别在于：

- 使用了自己的数据结构，该结构在oS中传递；
- 使用selectJ()而不是selectJrand()来选择第二个alpha的值；
- 在alpha值改变时更新Ecache。

```python
def innerL(i, oS):
	"""
	Function：	完整SMO算法中的优化例程

	Input：		oS：数据结构
				i：alpha的下标

	Output：	无
	"""	
	#计算误差
	Ei = calcEk(oS, i)
	#如果标签与误差相乘之后在容错范围之外，且超过各自对应的常数值，则进行优化
	if ((oS.labelMat[i]*Ei < -oS.tol) and (oS.alphas[i] < oS.C)) or ((oS.labelMat[i]*Ei > oS.tol) and (oS.alphas[i] > 0)):
		#启发式选择第二个alpha值
		j, Ej = selectJ(i, oS, Ei)
		#利用copy存储刚才的计算值，便于后期比较
		alphaIold = oS.alphas[i].copy(); alpahJold = oS.alphas[j].copy();
		#保证alpha在0和C之间
		if (oS.labelMat[i] != oS.labelMat[j]):
			L = max(0, oS.alphas[j] - oS. alphas[i])
			H = min(oS.C, oS.C + oS.alphas[j] - oS.alphas[i])
		else:
			L = max(0, oS.alphas[j] + oS.alphas[i] - oS.C)
			H = min(oS.C, oS.alphas[j] + oS.alphas[i])
		#如果界限值相同，则不做处理直接跳出本次循环
		if L == H: print("L==H"); return 0
		#最优修改量，求两个向量的内积（核函数）
		eta = 2.0 * oS.X[i, :]*oS.X[j, :].T - oS.X[i, :]*oS.X[i, :].T - oS.X[j, :]*oS.X[j, :].T
		#如果最优修改量大于0，则不做处理直接跳出本次循环，这里对真实SMO做了简化处理
		if eta >= 0: print("eta>=0"); return 0
		#计算新的alphas[j]的值
		oS.alphas[j] -= oS.labelMat[j]*(Ei - Ej)/eta
		#对新的alphas[j]进行阈值处理
		oS.alphas[j] = clipAlpha(oS.alphas[j], H, L)
		#更新误差缓存
		updateEk(oS, j)
		#如果新旧值差很小，则不做处理跳出本次循环
		if (abs(oS.alphas[j] - alpahJold) < 0.00001): print("j not moving enough"); return 0
		#对i进行修改，修改量相同，但是方向相反
		oS.alphas[i] += oS.labelMat[j] * oS.labelMat[i] * (alpahJold - oS.alphas[j])
		#更新误差缓存
		updateEk(oS, i)
		#更新常数项
		b1 = oS.b - Ei - oS.labelMat[i] * (oS.alphas[i] - alphaIold) * oS.X[i, :]*oS.X[i, :].T - oS.labelMat[j] * (oS.alphas[j] - alpahJold) * oS.X[i, :]*oS.X[j, :].T
		b2 = oS.b - Ej - oS.labelMat[i] * (oS.alphas[i] - alphaIold) * oS.X[i, :]*oS.X[j, :].T - oS.labelMat[j] * (oS.alphas[j] - alpahJold) * oS.X[j, :]*oS.X[j, :].T
		#谁在0到C之间，就听谁的，否则就取平均值
		if (0 < oS.alphas[i]) and (oS.C > oS.alphas[i]): oS.b = b1
		elif (0 < oS.alphas[j]) and (oS.C > oS.alphas[i]): oS.b = b2
		else: oS.b = (b1 + b2) / 2.0
		#成功返回1
		return 1
	#失败返回0
	else: return 0
```

## 外循环代码

&emsp;&emsp;外循环代码的输入和函数smoSimple()完全一样。整个代码的主体是while循环，终止条件：当迭代次数超过指定的最大值，或者遍历整个集合都未对任意alpha对进行修改时，就退出循环。while循环内部与smoSimple()中有所不同，一开始的for循环在数据集上遍历任意可能的alpha。通过innerL()来选择第二个alpha，并在可能时对其进行优化处理。如果有任意一对alpha值发生改变，就会返回1.第二个for循环遍历所有的非边界alpha值，也就是不在边界0或C上的值。

```python
def smoP(dataMatIn, classLabels, C, toler, maxIter):
	"""
	Function：	完整SMO算法

	Input：		dataMatIn：数据集
				classLabels：类别标签
				C：常数C
				toler：容错率
				maxIter：最大的循环次数

	Output：	b：常数项
				alphas：数据向量
	"""	
	#新建数据结构对象
	oS = optStruct(mat(dataMatIn), mat(classLabels).transpose(), C, toler)
	#初始化迭代次数
	iter = 0
	#初始化标志位
	entireSet = True; alphaPairsChanged = 0
	#终止条件：迭代次数超限、遍历整个集合都未对alpha进行修改
	while (iter < maxIter) and ((alphaPairsChanged > 0) or (entireSet)):
		alphaPairsChanged = 0
		#根据标志位选择不同的遍历方式
		if entireSet:
			#遍历任意可能的alpha值
			for i in range(oS.m):
				#选择第二个alpha值，并在可能时对其进行优化处理
				alphaPairsChanged += innerL(i, oS)
				print("fullSet, iter: %d i: %d, pairs changed %d" % (iter, i, alphaPairsChanged))
			#迭代次数累加
			iter += 1
		else:
			#得出所有的非边界alpha值
			nonBoundIs = nonzero((oS.alphas.A > 0) * (oS.alphas.A < C))[0]
			#遍历所有的非边界alpha值
			for i in nonBoundIs:
				#选择第二个alpha值，并在可能时对其进行优化处理
				alphaPairsChanged += innerL(i, oS)
				print("non-bound, iter: %d i: %d, pairs changed %d" % (iter, i, alphaPairsChanged))
			#迭代次数累加
			iter += 1
		#在非边界循环和完整遍历之间进行切换
		if entireSet: entireSet = False
		elif (alphaPairsChanged == 0): entireSet =True
		print("iteration number: %d" % iter)
	#返回常数项和数据向量
	return oS.b, oS.alphas
```

&emsp;&emsp;测试代码，大家有兴趣的话可以多次运行计算一下运行时间的平均值，看看和简化版相比快了多少。

```python
>>> reload(svmMLiA)
<module 'svmMLiA' from 'E:\\机器学习实战\\mycode\\Ch06\\svmMLiA.py'>
>>> dataArr, labelArr = svmMLiA.loadDataSet('testSet.txt')
>>> b, alphas = svmMLiA.smoP(dataArr, labelArr, 0.6, 0.001, 40)
L==H
fullSet, iter: 0 i: 0, pairs changed 0
L==H
fullSet, iter: 0 i: 1, pairs changed 0
fullSet, iter: 0 i: 2, pairs changed 1
L==H
···
j not moving enough
fullSet, iter: 2 i: 97, pairs changed 0
fullSet, iter: 2 i: 98, pairs changed 0
fullSet, iter: 2 i: 99, pairs changed 0
iteration number: 3
```

&emsp;&emsp;常数C一方面要保障所有样例的间隔不小于1.0，另一方面又要使得分类间隔要尽可能大，并且要在这两方面之间平衡。如果C很大，那么分类器就会将力图通过分隔超平面对多有的样例都正确分类。这种优化结果如下图，很明显，支持向量变多了。如果数据集非线性可分，就会发现支持向量会在超平面附近聚集成团。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20171009160736671?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"/></div>
<p></p>

## 分类测试

好了，终于可以拿我们计算出来的alpha值进行分类了。首先必须基于alpha值得到超平面，这也包括了w的计算。

```python
def calcWs(alphas, dataArr, classLabels):
	"""
	Function：	计算W

	Input：		alphas：数据向量
				dataArr：数据集
				classLabels：类别标签

	Output：	w：w*x+b中的w
	"""	
	#初始化参数
	X = mat(dataArr); labelMat = mat(classLabels).transpose()
	#获取数据行列值
	m,n = shape(X)
	#初始化w
	w = zeros((n,1))
	#遍历alpha，更新w
	for i in range(m):
		w += multiply(alphas[i]*labelMat[i],X[i,:].T)
	#返回w值
	return w
```

&emsp;&emsp;上述代码中最重要的就是for循环，实现多个数的乘积。虽然for循环遍历了数据集中的所有数据，但是最终起作用的只有支持向量。

```python
>>> from numpy import *
>>> datMat = mat(dataArr)
>>> datMat[0]* mat(ws)+b
matrix([[-0.92555695]])
>>> labelArr[0]
-1.0
>>> datMat[2]* mat(ws)+b
matrix([[ 2.30436336]])
>>> labelArr[2]
1.0
>>> datMat[1]* mat(ws)+b
matrix([[-1.36706674]])
>>> labelArr[1]
-1.0
```

&emsp;&emsp;上面的测试中，计算值大于0属于1类，小于0属于-1类。

&emsp;&emsp;至此，线性分类器介绍完了，如果数据集非线性可分，那么我们就需要引入核函数的概念了，下一篇将进行介绍。

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**
