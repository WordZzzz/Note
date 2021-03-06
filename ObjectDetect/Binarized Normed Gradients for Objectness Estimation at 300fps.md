**<div align=center><font color="black" size=6 face="仿宋">BING: Binarized Normed Gradients for Objectness Estimation at 300fps</font></div>**
**<div align=center><font color="black" size=6 face="仿宋">基于二值化赋范梯度特征的一般对象估计</font></div>**

**<font color="black" size=5 face="仿宋">摘要：</font>**

&emsp;&emsp;训练通用对象度量来生成一小组候选对象窗口，已经显示出加速了经典的滑动窗口对象检测范式。我们观察到，通过观察梯度的标准，可以通过将其对应的图像窗口适当地调整到小的固定尺寸来区分具有良好定义的闭合边界的通用对象。基于这个观察和计算复杂度，我们建议将窗口大小调整为8*8并使用赋范梯度作为简单的64D特征来描述，用于明确地训练通用的对象度量。

&emsp;&emsp;我们进一步展示了这种特征的二值化版本，即二值化赋范梯度（BING），它可以用于高效的对象估计，这只需要少量的原子操作（例如加法，按位移动等）。对有挑战性的PASCAL VOC 2007数据集的实验表明，我们的方法有效生成一小组类别无关的高质量对象窗口（单个笔记本电脑CPU上的300fps），通过使用1,000个建议窗口，产生96.2％的对象检测率（DR）。通过增加建议窗口和颜色空间的数量来计算BING特征，我们的性能可以进一步提高到99.5％的DR。

&emsp;&emsp;作为计算机视觉中最重要的领域之一，近年来，对象检测取得了长足的进步。然而，大多数最先进的检测器仍然需要每个类别的特定分类器以滑动窗口方式评估许多图像窗口[17,25]。为了减少每个分类器需要考虑的窗口数量，训练类别通用的对象度量最近变得流行[2,3,21,22,48,49,57]。对象通常表示为反映图像窗口覆盖任何类别的对象的可能性的值[3]。通用对象度量在预过滤过程中具有很大的潜力，可以显着改善：i）通过减少搜索空间来提高计算效率，以及ii）通过允许在测试期间使用强分类器来提高检测精度。然而，设计好的通用对象度量方法是困难的，需要：

- 具备很好的检测率，找到所有前景对象；
- 提出少量建议，用于减少对象检测的计算时间；
- 达到很高的计算效率，很容易拓展到其他实时以及大规模的应用程序中；
- 具备很好的通用性，方便用于各个类别的检测器中，这样可以减少计算量

&emsp;&emsp;据我们所知，暂时还没有任何方法可以同时满足以上全部要求。

&emsp;&emsp;认知心理学[47,54]和神经生物学[20,38]的研究表明，人们在识别物体之前具有很强的感知对象能力。 基于观察到的人类反应时间和估计的生物信号传播时间，人类注意理论假设人类视觉系统仅处理图像的一部分，而对图像的其余部分几乎视而不见。这进一步表明，在识别对象之前，在人类视觉系统中有简单的机制来选择可能的对象位置。

&emsp;&emsp;在本文中，我们提出了一个非常简单而又强大的特征“BING”，通过使用对象状态分数来帮助搜索对象。我们的工作动机是基于物体是具有明确的封闭边界和中心的独立事物[3，26，32]的事实。我们观察到，当查看赋范梯度（见图1和图3）时，将其相应的图像窗口大小调整为小的固定大小（例如8 * 8）后，具有明确定义的闭合边界的通用对象具有惊人的强相关性）。因此，为了有效地量化图像窗口的目标，我们将其大小调整为8 * 8，并使用梯度的范数作为一个简单的64D特征，用于在级联SVM框架中学习通用对象度量。我们进一步展示了NG功能的二值化版本，即二值化规范梯度（BING）功能，可以用于图像窗口的有效对象估计，只需要几个原子CPU操作（即加法，按位移动等） ）。 大部分现存的先进方法，一般采用复杂的分类特征，而且需要采用加速方法以至于计算时间是可控的，相对于此，BING特征是简单朴素的。

&emsp;&emsp;我们已经在PASCAL VOC2007数据集上，广义的评价了我们的方法。实验结果表明，我们的方法有效（在单个笔记本电脑CPU上为300fps）生成一小组数据驱动，类别无关，高品质的对象窗口，通过使用1000个窗口（约为整个滑动窗口的0.2%），产生96.2％的检测率（DR）。将目标窗口数量增加到5000个，估计3个不同颜色空间的对象，我们的方法可以达到99.5％的DR。在[3,22,48]之后，我们也验证了我们的方法的泛化能力。当我们对6个物体类别的对象度量进行训练时，对其他14个不可见的类别进行测试，我们观察到与标准设置类似的高性能（见图3）。与大多数流行的替代方案相比[3,22,48]，BING功能使我们能够使用较小的提议来实现更好的DR，而且能够预测不可见的类别，更简单，更快1000倍。这满足了良好的目标检测器的上述要求。我们的源代码将与论文一起发布。

**<font color="black" size=5 face="仿宋">2.相关工作</font>**

&emsp;&emsp;能够在识别物体之前感知物体与自下而上的视觉注意（显着性）密切相关。根据显著性定义，我们将相关研究大致分为三类：固定点预测、显著性对象检测，对象提案生成。

&emsp;&emsp;固定点预测：该模型旨在预测人眼运动的显着性[4,37]。 灵感来自神经生物学研究关于早期灵长类动物视觉系统，Itti等人[36]提出了显着检测的第一个计算模型之一，其估计了多尺度图像特征中的中心包围的差异。Ma和Zhang[42]提出了一种模糊增长模型来分析局部对比度显着性。Harel等人[29]提出了将中心包围的特征图归一化以突出显眼部分。虽然固定点预测模型已经取得了显着的发展，但预测结果倾向于突出边缘和角落而不是整个对象。因此，这些模型不适合生成用于检测目的的对象提案。

&emsp;&emsp;显著性对象检测：该模型尝试检测场景中最引人注意的对象，然后对该对象的整个范围进行细分[5,40]。Liu等人[41]将CRF框架中的局部，区域和全局显着性检测结合起来。 Achanta等人[1]使用频率调节方法的局部显着区域。 Cheng等[11,14]提出了一种基于区域对比分析和迭代图分割的显著性对象检测和分割方法。最近的研究还试图在基于滤波的框架[46]中生成高质量的显着性图，使用有效的数据表示[12]或考虑分层结构[55]。对于简单图像的这种显著性物体分割在图像场景分析[15,58]，内容感知图像编辑[13,56,60]中取得了巨大的成功，并且可以用作处理大量互联网图像或构建的便宜工具通过自动选择好的结果[10,11]，可以很好的应用[7,8,16,31,34,35]。然而，这些方法不太可能用于呈现许多对象的复杂图像，并且它们很少占主导地位（例如VOC[23]）。

&emsp;&emsp;对象提案生成：该方法通过提出少量（例如1,000个）类别无关的提案，并不作出决定，将覆盖图像中的所有对象[3,22,48]。生成粗略分割[6,21]作为对象提案已被证明是减少针对特定分类器的搜索空间的有效方法，同时允许使用强分类器来提高准确性。然而，这两种方法在计算上是昂贵的，每个图像需要2-7分钟。 Alexe等人[3]提出了一种提示集成方法，以更有效地获得更好的预测性能。张等人[57]提出了一种具有定向梯度特征的级联排序SVM方法，用于有效的提案生成。 Uijlings等人[48]提出了一种选择性搜索方法来获得更高的预测性能。我们提出一种简单而直观的方法，通常可以比其他方法更好地实现检测性能，并且比最流行的替代品快了1000倍[3,22,48]（见第4节）。

&emsp;&emsp;另外，对于一个有效的滑动窗口对象检测方法，保证计算量可控是非常重要的[43,51]。Lampert等人[39]提出了一个优雅的分支定界方法用于检测。但是，这些方法只能用于加速分类器，而且是用户已经提供了一个好的边框。一些其他有效的分类器[17]和近似核方法[43,51]也已经被提出。这些方法旨在减小估计单个窗口的计算量，自然也能结合对象性建议进而减小损失。

**<font color="black" size=5 face="仿宋">3.方法</font>**

&emsp;&emsp;灵感来自人类视觉系统的能力，在识别物体之前有效地感知对象[20,38,47,54]，我们引入了梯度（NG）特征（第3.1节）的简单64D范数，以及其二进制近似 ，即二值化赋范梯度（BING）特征（第3.3节），以有效捕获图像窗口的对象性。

&emsp;&emsp;为了找到图像中的一般对象，我们扫描一个定义好的量化窗口（依据尺度或者是纵横比）。每一个窗口通过一个线性模型  `!$w ∈ R64$ `获得得分

```mathjax!
\begin{equation}
s_l = < w,g_l >(1)
\end{equation}
```
   

```mathjax!
\begin{equation}
l = < i,x,y >(2)
\end{equation}
```
   

&emsp;&emsp;`!$s_l$` 代表过滤器得分，  `!$g_l$` 代表NG特征，  `!$l$` 表示坐标，  `!$i$` 表示尺度，  `!$(x,y)$` 表示窗口位置。运用非最大抑制(NMS)，我们为每个尺度提供一些建议窗口。相对于其他窗口（例如：100 * 100），一些尺度（例如:10 * 500）的窗口包含对象的可能性是很小的。因此我们定义对象状态得分（校准过滤器得分）：

```mathjax!
\begin{equation}
o_l = v_i · s_l + t_i  (3)
\end{equation}
```
   

&emsp;&emsp;其中  `!$v_i，t_i∈ R$ `，针对不同尺度  `!$i$` 的窗口，得到不同的独立学习系数。使用校准函数（3）是非常快的，通常只需要在最终的建议窗口重排之后进行。

3.1 梯度幅值（NG）和对象状态

&emsp;&emsp;对象是具有明确界限的边界和中心的独立事物[3,26,32]。当将与现实世界对象相对应的窗口大小调整为小的固定大小（例如8 * 8，由于计算原因而选择，将在第3.3节中进行说明）时，相应图像梯度的范数（即幅度）将成为良好的辨别特征，因为封闭边界在这种抽象视图中可能存在的小变化。如图1所示，虽然游轮和人在颜色，形状，质地，照明等方面有很大的差异，但它们在赋范梯度空间中具有明显的相关性。为了有效地预测对象实例的存在，利用这一观察，我们首先将输入图像的大小调整为不同的量化尺寸，并计算出每个调整大小的图像的规范梯度。这些调整大小的规范梯度图的8 * 8个区域的值被定义为其对应窗口的64D规范梯度（NG）特征。

&emsp;&emsp;我们的NG特征，作为图像窗口的密集和紧凑的对象特征，具有以下几个优点。 首先，无论物体的位置，尺度和纵横比如何变化，由于该特征的归一化支持区域，其对应的NG特征将保持大致不变。 换句话说，NG特征对于位置，比例和纵横比的变化不敏感，这对于检测任意类别的对象非常有用。而这些不敏感的属性就是一个很好的对象建议生成方法。 其次，NG特征的密集紧凑表示使得计算和验证非常有效，因此具有很大的实际应用潜力。
 
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823181219294?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="700" /></div>
<p></p>
Figure 1 尽管对象（红色）和背景（绿色），在图像空间（a）呈现出了很大的不同，通过一个适当的尺度和纵横比，我们将其分别重置为固定大小(b)，他们对应的NG特征(c)表现出很大的共性,基于NG特征，我们学习了一个简单的64D线性模型（d），用来筛选对象窗口。"
&emsp;&emsp;将这种优势引入NG特征的成本是丧失描述能力。 幸运的是，所产生的误报将由后续的类别特定检测器处理。 在第4节，我们显示我们的方法产生了一小组高质量的提案，涵盖了具有挑战性的VOC2007数据集中的96.2％的真实对象窗口。

3.2 objectness度量

&emsp;&emsp;为了学习图像窗口的对象度量，我们遵循两级SVM的总体思路[57]。

- 使用线性SVM学习式（1）中的单个模型w [24]。 真实对象窗口和随机采样背景窗口的NG特征分别用作正和负训练样本。

- 在（3）中使用线性SVM学习  `!$v_i$`和  `!$t_i$` [24]，我们评估（1）在尺寸  `!$i$` 上训练图像，并将所选择的（NMS）提案作为训练样本，将它们的过滤分数作为1D特征，并使用训练图像注释检查其标签（有关评估标准，请参阅第4节）。

- 讨论：如图1d所示，学习的线性模型w（参见实验设置的第4节）看起来类似于假设为灵长类动物的生物似然结构的多尺寸中心包围模式[27,38,54]。 沿着边界的大的权重有利于将物体（中心）与其背景（包围）分开的边界。 与手动设计的中心环绕模式[36]相比，我们学到的w捕获更复杂更自然的前景。 比如，低层面的对象相对于高层面的部分要更加阻塞。也就表示模型w中会给予低层次的对象更低的置信度。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823184337349?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>


3.3 二值化梯度幅值(BING)

&emsp;&emsp;为了能够利用二值化近似模型[28,59]中的优点，我们提出了一个NG特征的加速版，二值化赋范梯度，加速特征提取和测试过程。我们学习的线性模型w∈R64可以通过算法1近似表示为一系列基向量的组合  `!$ w ≈ \sum_{j=1}^{N_w} \beta_j \alpha_j$ `。

&emsp;&emsp;其中`!$N_w$`表示基向量的个数，`!$\alpha_j∈\{ -1,1 \}^{64}$ `表示基向量，`!$\beta_j∈R$ `表示校准系数。 `!$\alpha_j$` 可以进一步表示为二值向量和它的补： `!$\alpha_j = \alpha_j^+ - \overline{\alpha_j^+}$` , 其中 `!$\alpha_j^+∈\{ 0,1 \}^{64}$ `,由  `!$α$ `二值化之后得到的  `!$b$ `可以被直接用于测试，而且只需要按位与和字节统计操作[28]

```mathjax!
\begin{equation}
< w,b > ≈ \sum_{j=1}^{N_w}\beta_j(2< \alpha_j^+,b>-|b|)
\end{equation}
```
 
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823184540642?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>

&emsp;&emsp;关键过程就是如何二值化以及有效的计算NG特征。我们近似采用梯度幅值（以及转化为01字节）的前Ng位来作二值化。因此，64维 NG特征 `!$g_l$` 值可以通过前Ng位二值化梯度幅值(BING)近似表示为：

```mathjax!
\begin{equation}
g_l≈ \sum_{k=1}^{N_g}2^{8-k}b_{k,l}
\end{equation}
```
 
<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823184656137?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="400" /></div>
<p></p>

Figure 2 变量说明：BING特征`!$b_{x,y}$`,它的最后一行是`!$r_{x,y}$`，最后一个元素`!$b_{x,y}$`。注意出现在式(2)和式(5)中的下标  `!$i, x, y, l, k,$` 这些是定位整个向量而不是向量元素的索引。我们可以用一个简单的原子变量(INT64和BYTE)表示BING特征和它的最后一行，这样能够更有效的进行特征计算。

&emsp;&emsp;注意：这些BING特征拥有不同的权重，依据它原本不同的字节位。获取8 * 8的BING特征一般需要遍历64位，依据8 * 8 BING特征的两个特征，我们提出了一个快速的特征计算方法，能够只使用一些简单的原子操作（按位或和按位移动）避免循环计算。

&emsp;&emsp;第一，BING特征`!$b_{x,y}$` 以及它的最后一行  `!$r_{x,y}$` 可以存储在一个简单的INT64和一个BYTE变量中；第二，相邻的BING特征以及他们的行之间拥有一个简单的累积关系。如图2，将  `!$r_{x-1,y}$` 按位移动1位，这1位将自动进位到  `!$r_{x,y}$` ，插入  `!$b_{x,y}$` 的过程可以用按位或来实现。同样，将  `!$b_{x-1,y}$` 按位移动8位，这8位将自动进位到  `!$b_{x,y}$` ，自动插入  `!$r_{x,y}$` 。

&emsp;&emsp;我们的BING特征有效的利用了整体图像之间的累积性质[52]。与之前的方法在任意矩形范围内计算一些值不同的是，我们采用一些原子操作在一个固定8 * 8大小范围内计算一系列二进制模式。

&emsp;&emsp;一个图像窗口对应的BING特征  `!$b_{k,l}$` 的过滤器得分,见式(1),可以表示为：

```mathjax!
\begin{equation}
s_l≈ \sum_{j=1}^{N_w}\beta_j\sum_{k=1}^{N_g}C_{j,k}
\end{equation}
```
   

&emsp;&emsp;其中，  `!$C_{j,k} = 2^{8-k}(2< \alpha_j^+,b_{k,l}>-|b_{k,l}|)$` 可以通过一些快速的按位操作以及SSE指令操作计算得到。

&emsp;&emsp;实现细节：我们使用一维的标识[-1,0,1]，来定义水平方向和垂直方向上的图像梯度  `!$g_x$` 和  `!$g_y$` ，当计算梯度幅值是采用  `!$min(|g_x|+|g_y|,255)$` ,然后将其存入一个BYTE中。

**<font color="black" size=5 face="仿宋">4.实验评估</font>**

&emsp;&emsp;我们在VOC2007数据集上实验评估了我们的方法，使用的是DR和WIN的评估标准，与3个现存最先进的方法建议质量、通用性以及效率上做了对比。正如[3,48]，一系列高检测率的粗糙集对于有效对象检测是足够了的，，而且它允许使用复杂的特征和补充线索来得到比传统方法更好的质量和更高的效率。在对比试验中，我们采用的对应作者公布的实现方式和建议的参数设置。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823185011109?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="700" /></div>
<p></p>
Figure 3 不同方法关于#WIN和DR的权衡曲线。我们的方法使用1000建议窗口，达到了96.2%DR，使用5000个建议窗口可以达到99.5DR.其他三个方法我们采用了相同的评估标准，可以看出来优于一些其他的方法[6,21,25,30,50]，显著性测量[33，36]，特征点检测[44]以及HOG检测[17].
 
&emsp;&emsp;建议质量对比：参照[3,48,57]，我们在数据集VOC2007上采用DR-WIN评估测试集，该数据集包含4952张20个类别的带有边框注释图像。数量巨大，种类繁多，视角、尺度、位置、阻塞、光照等都有不同，这些特点非常符合我们识别所有对象的要求。图3展示了数据统计对比，对比的方法有：OBN[3]，SEL[48]，以及CSVM[57]。正如[48]，通过收集不同参数设置下的结果，可以增加建议窗口的离散度，也能提高检测率DR，当然也需要提高建议窗口的数量（#WIN）。SEL[48]组合了80个不同参数设置的结果，达到了99.1%的DR和使用了10000多个建议窗口。我们的方法达到99.5%的DR，但只需要5000个建议窗口，而且仅仅收集了3个颜色空间的结果（RGB，HSV，GRAY）。如同DR-#WIN数据分析展示的那样，我们简单的方法在总体上达到了更好的效果，速率上比最流行的的方法[3,22,48]提升了3个数量级（见表格1），我们举例阐述了一些不同复杂度下的结果，如图4.

&emsp;&emsp;通用性测试：参照[3]，为了证明我们的方法具有通用性，采用包含未训练类别的对象图像进行测试。我们采用6个类别的对象训练我们的方法，通过剩下的14个类别进行测试。图3中，训练和测试是通过BING和BING-generic表示的。正如我们看到了那样，两个曲线基本一致，证明了我们方法的通用性。

&emsp;&emsp;最近的工作[18]能够在20秒内检测100000对象类别，主要采用的是减低传统多类别计算复杂度从O(LC)到O(L),L表示推荐窗口的数量，C表示分类器的数量。我们的方法可以得到任意类别（训练过的以及未训练的）的高质量的推荐窗口，可以通过减小L来显著减少计算复杂度。

&emsp;&emsp;计算时间：见表1，我们的方法可以在300fps的视频中，提供高效率的提供高质量的对象窗口，其他的方法对一张图片都需要几秒。这些方法通常是作为现存最先进的算法，而且很难大幅度的提升速度。我们在2501张图片上的训练需要很少的时间（包括加载XML文件，总共20S），而现有的先进的方法[6,21]测试一张图片通常需要多于2分钟.

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823185136104?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="200" /></div>
<p></p>
Table 1 在VOC2007上的平均计算时间

&emsp;&emsp;如表2，通过采用二进制近似的方法学习线性过滤器和BING特征提取，计算每个图像窗口的得分只需要一些原子操作。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823185234675?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="300" /></div>
<p></p>
Table 2 用于计算不同阶段每个图像窗口的对象的平均原子操作数：计算规范梯度，提取BING特征，并获得对象分数。

&emsp;&emsp;在每个标准化的尺度和纵横比下，定位的数量相当于O（N），N代表图像中的像素数.因此，在所有尺寸和纵横比的图像中，计算得分的复杂度也是O(N)。在每一个潜在的位置上，提取BING特征和计算得分可以利用它邻近的2个位置(例如：左和上)。这意味着空间复杂度也是O(N)。我们在同一个Intel i7-3940XM CPU上，对比其他基准方法[3,2，，48,57]的运行时间。

&emsp;&emsp;如表3，我们可以进一步意识到，不同的近似对结果质量的影响，通过对比我们在其他试验中采用Nw=2，Ng=4.

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823185400065?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="250" /></div>
<p></p>
Table 3 在不同近似层下的，平均检测结果（DR，使用1000个建议窗口），控制（Nw和Ng），N/A表示没有近似。

<p></p>
<div align=center><img src="http://img.blog.csdn.net/20170823185822979?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTQ3NTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="800" height="700" /></div>
<p></p>
Figure 4 在VOC2007上的真实的测试样例。

**<font color="black" size=5 face="仿宋">5.结论以及将来的工作</font>**

&emsp;&emsp;我们呈现了一个非常简单，快速而且高质量的objectness方法，通过采用BING特征，计算任意尺度和纵横比的图像窗口中，我们仅仅需要一些原子操作（加，按位等）。通过最广泛的基准（VOC2007）和DR-#WIN评估标准进行结果评价，结果表明，与其他现存先进方法[3,22,48]相比，我们的方法不仅表现突出，而且速度上提升了3个数量级。

&emsp;&emsp;局限性： 与其他objectness方法[3,57]和滑动窗口[17,25]一样，我们都预测了一系列的对象矩形边框，因此，也有相似的局限性，对于一些类别的对象，一个矩形框并不能很好的集中实体，以便用来进行区域分割[6,21,33,45]，例如蛇。

&emsp;&emsp;进一步的工作：由于我们的方法具备高质量以及高效率的特性，所以它很适合实时多类别的对象检测和大规模图像收集应用程序（如：ImageNet[19]）。由于使用的简单的二进制操作，而且空间效率高，使得我们的方法可以在普通的设备上运行[28,59]。

&emsp;&emsp;我们的加速策略主要是减少窗口数量，这个可以通过其他的加速技术（通常旨在减少分类时间）来实现。将我们的方法和[18]的方法的进行组合将是很有趣的这样能够在一个机器上实时检测数以千计类别的对象。我们的方法解决了基于对象检测方法[53]的效率屏障，使得能够进行实时的高质量的对象检测。

&emsp;&emsp;通过使用简单的BING特征，我们能够使用一小部分（1000）的建议窗口得到涵盖几乎（96.2%）所有的对象区域。引入新的线索进一步降低建议窗口的数量，以便维持高效率的对象检测，以及在更多的应用程序[9]上使用BING特征，将是很值得研究的。
