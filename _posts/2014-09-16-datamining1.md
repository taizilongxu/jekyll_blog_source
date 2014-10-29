---
layout: post
title:  "数据挖掘笔记"
category: datamining
tags: datamining 
---

##Chanpter 2 Data Preprocessing(数据预处理)

定期更新中~October 11 2014 10:36 AM

###1 Why preprocess the data?(数据预处理的必要性）

要进行数据挖掘需要把数据进行预处理,最简单的原因就是如果提供了完整干净的数据集,那么在数据挖掘的过程中就不用考虑这些东西了,所以很有必要.

* 不完整(缺少属性值)
* 含噪音(错误的值或者偏离太大的值)
* 不一致(比如字符串和整数相混)
* 重复的(一个人对应几个名字)

把大象装冰箱总共分四部:

1. 数据清理(Data cleaning): 可以去掉数据中的噪声，纠正不一致
2. 数据集成(Data integration): 将多个数据源合并成一致的数据存储，构成一个完整的数据集，如数据仓库或数据立方体
3. 数据变换(Data transformation): 将一种格式的数据转换为另一格式的数据(如规范化)
4. 数据归约(Data reduction): 可以通过聚集、删除冗余特性或聚类等方法来压缩数据

如下图所示:

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-17 16:52:40 的屏幕截图.png)

###2 Descriptive Date Summarization(描述性数据汇总)

这些东西可以更加直观的观测数据,更好的理解数据

####Measuring the Central Tendency（度量数据的中心趋势）

1. mean(几何平均数)
2. median(中值)
3. mode(众数): 出现最多的一个数

这里有一个经验公式:

$$ mean - mode = 3 * (mean - median) $$

条件是单峰的情况下,并且是asymmetrical类型(倾斜的数据)

####Measuring the Dispersion of Data(度量数据的离散程度)

1. Quartiles, outliers and boxplots（四分位数、离散点和盒图）
	* Inter-quartile range: IQR = Q3 – Q1;(Q1(25%),Q3(75%))
	* Five number summary: min, Q1, M, Q3, max
	* Boxplot(盒图):招不开了,见下
	* Outlier: usually, a value higher/lower than 1.5 x IQR
2. Variance and standard deviation (sample: s, population: σ)（方差和标准差）
3. Properties of Normal Distribution Curve(正态分布特性)

####Graphic Displays of Basic Statistical Descriptions(描述数据的基本图形)

Boxplot(盒图)

* Data is represented with a box(一个盒子来表示)
* The ends of the box are at the first and third quartiles, i.e., the height of the box is IRQ(上下边界分别是四分位点,即Q1和Q3,高度就是IRQ的高度了)
* The median is marked by a line within the box(中值在盒子里用一个线表示)
* Whiskers: two lines outside the box extend to Minimum and Maximum(盒子外两条线延伸到最大最小值)
* whiskers(上面两条线)长度不会大于1.5 * IRQ

来感受下:

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/Cpb7Fx.png)

Histogram Analysis(直方图)

Quantile Plot(分位数图)

Quantile-Quantile (Q-Q) Plot()(Q-Q图,横纵坐标都为分位数)

Scatter plot（散点图）

Loess (local regression) curve(回归曲线?): add a smooth curve to a scatter plot to provide better perception of the pattern of dependence

###3 Data Cleaning

数据清理分以下几步:

1. 填充空缺的值
2. 识别孤立点
3. 消除噪声
4. 纠正数据中的不一致

####Missing Values(空缺数据)

1. 忽略该元组(最常用的)
2. 人工填写空缺值: 量太大!
3. 使用一个全局常量填充空缺值: 将空缺的属性值用同一个常数(如“Unknown”或)替换。如果空缺值都用“Unknown”替换，当空缺值较多时。挖掘程序可能误以为它们形成了一个有趣的概念，因为它们都具有相同的值——“Unknown”。因此，尽管该方法简单，我们并不推荐它。
4. 使用属性的平均值填充空缺值
5. 利用同类别均值填补遗漏数据: 例如，如果将顾客按credit risk分类， 则用具有相同信用度的顾客的平均收入替换income中的缺值
6. 使用最可能的值填充空缺值:可以利用回归、贝叶斯计算公式或判定树归纳确定，推断出该条记录特定属性最大可能的取值。例如，利用数据集中其他顾客的属性，可以构造一棵判定树，来预测income的空缺值。

####Noisy Data

1. **binning(分箱)**: 分箱方法通过考察“邻居”(即周围的值)来平滑存储数据的值。存储的值被分布到一些“桶”或箱中。由于分箱方法参考相邻的值，因此它进行局部平滑。这里平滑方法又分了3种
	* 按箱平均值平滑
	* 按箱中值平滑
	* 按箱边界平滑
2. **clustering(聚类)**: 通过聚类分析可以检测孤立点，聚类将类似的值组织成群或“聚类”。直观地看，落在聚类集合之外的值被视为孤立点,这个方法在机器学习里有过了解.
3. **regression(回归)**: 以利用拟合函数(如回归函数)来平滑数据。

###4 Data Integration(数据集成)

它需要统一原始数据中的所有矛盾之处，如字段的:同名异义、异名同义、单位不统一,字长不一致，从而把原始数据在最低层上加以转换，提炼和集成。

1. **模式集成**: 通常，数据库和数据仓库有元数据——关于数据的数据。这种元数据可以帮助避免模式集成中的错误。
2. **冗余问题**: 可以用概率的方法算出两个数据是相关还是独立,这种分析可以度量一个属性能在多大程度上蕴含另一个.
3. **数据值冲突的检测与处理**:
	* 表示不同导致数据冲突
    * 语义不同导致数据冲突

对于现实世界的同一实体，来自不同数据源的属性值可能不同。这可能是因为表示、比例或编码不同。例如，重量属性可能在一个系统中以公制单位存放，而在另一个系统中以英制单位存放。不同旅馆的价格不仅可能涉及不同的货币，而且可能涉及不同的服务(如免费早餐)和税。数据这种语义上的异种性，是数据集成的巨大挑战。

将多个数据源中的数据集成起来，能够减少或避免结果数据集中数据的冗余和不一致性。这有助于提高其后挖掘的精度和速度。另外，在数据集成中还应考虑数据类型的选择问题，如在值域范围内应尽量用tinyint代替int, 可大大减少字节数，对于大规模数据集来说将会大大减少系统开销。

###5 Data Transformation(数据变换)

数据变换将数据转换成适合于挖掘的形式。主要是找到数据的特征表示，对数据进行规格化处理。用维变换或转换方式减少有效变量的数目或找到数据的不变式.

1. **smoothing(平滑)**: 去掉数据中的噪声。这种技术包括分箱（Bin)、聚类和回归。
2. **Aggregation(聚集)**:对数据进行汇总和聚集例如，可以聚集日销售数据，计算月和年销售额。这一步用来为多粒度数据分析构造数据立方体。
3. **Generalization(概化)**:使用概念分层，用高层次概念替换低层次“原始”数据。例如，分类的属性，如street，可以概化为较高层的概念，如city或country.
4. **Normalization(规范化)**: 将属性数据按比例缩放，使之落入一个小的特定区间，如-1．0到1．0或0.0到1.0。规格化的目的是将一个属性取值范围影射到一个特定范围之内，以消除数值性属性因大小不一而造成挖掘结果的偏差.
5. **Attribute construction属性构造或特征构造**: 可以利用已知的属性构造新的属性并添加到属性集中，以帮助挖掘过程。(由长，宽求面积)

###7 Data Reduction(数据归约)

主要以下方法:

1. Data cube aggregation(数据方聚集):聚集操作用于数据方中的数据。主要用来构建数据立方.
![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-10-11 10:28:48 的屏幕截图.png)
2. Atrribute subset selection(维归约):可以检测并删除不相关、弱相关或冗余的属性或维。(去除多余的属性值)
3. Dimensionality reduction(数据压缩):使用编码机制压缩数据集。
	* discrete wavelet transform小波变换DWT
    * principal components analysis 主要成分分析
4. Numerosity reduction(数值压缩):用替代的、较小的数据表示替换或估计数据,如参数模型(只需要存放模型参数,而不是实际数据)或非参数方法,如**聚类**、**选样**和使用**直方图**。
5. Discretization and concept hierarchy generation(离散化和概念分层产生):属性的原始值用区间值或较高层的概念替换。概念分层允许挖掘多个抽象层上的数据,是数据挖掘的一种强有力的工具。
五种数值概念分层产生方法:
	* 分箱
    * 直方图分析
    * 聚类分析
    * 基于熵的离散化
    * 通过“自然划分”的数据分段 :  **3-4-5 规则**

##Chapter 3 Data warehouse and OLAP technology:an overview(数据仓库和数据挖掘的 OLAP 技术)




