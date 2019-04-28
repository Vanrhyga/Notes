[TOC]

# ROC 曲线

分类器性能指标

## 定义

接受者操作特征，曲线上每个点反映着对同一信号刺激的感受性。

**横轴**：负正类率（false postive rate FPR）特异度。

**纵轴**：真正类率（true postive rate TPR）灵敏度，Sensitivity（正类覆盖率）

针对一个二分类问题，将实例分成正类（postive）或者负类（negative）。但是实际分类中会出现以下4种情况：

- 若一个实例是正类并且被预测为正类，即为真正类（true postive TP）
- 若一个实例是正类，但是被预测成为负类，即为假负类（false negative FN）
- 若一个实例是负类，但是被预测成为正类，即为假正类（false postive FP）
- 若一个实例是负类并且被预测为负类，即为真负类（true negative TN）

**TP**：正确的肯定数目

**FN**：漏报，没有找到正确匹配的数目

**FP**：误报

**TN**：正确拒绝的非匹配数目

Actual Postive（TP+FN）

Actual Negative（FP+TN）

- 真正类率 TPR：TP/(TP+FN)，代表分类器预测的正类中实际正实例占所有正实例的比例。
- 负正类率 FPR：FP/(FP+TN)，代表分类器预测的正类中实际负实例占所有负实例的比率，1 - Specificity
- 真负类率（True Negative Rate）TNR：TN/(FP+TN)，代表分类器预测的负类中实际负实例占所有负实例的比例，Specificity

假设采用逻辑回归分类器，给出针对每个实例为正类的概率。那么通过设定一个阈值，如0.6，概率大于等于 0.6 的为正类，小于 0.6 的为负类。对应的就可以算出一组（FPR,TPR），在平面中得到对应坐标点。随着阈值的逐渐减小，越来越多的正例被划分为正类，但是这些正类中同样也掺杂着真正的负实例，即 TPR 和 FPR 会同时增大。阈值最大时，对应坐标点为（0，0），阈值最小时，对应坐标点为（1，1）。

![image-20180731184712900](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180731184712900.png)

理想目标：TPR=1，FPR=0，即图中（0，1）点，故 ROC 曲线越靠拢（0，1）点，越偏离 45° 对角线越好，Sensitivity、Specificity 越大效果越好。



## 画法

假设已经得出一系列样本被划分为正类的概率，然后按照大小排序，下图是一个示例，图中共有20个测试样本，“Class”一栏表示每个测试样本真正的标签（p 表示正样本，n 表示负样本），“Score” 表示每个测试样本属于正样本的概率。

![image-20180731185712765](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180731185712765.png)

接下来，我们从高到低，依次将“Score”值作为阈值 threshold。当测试样本属于正样本的概率大于或等于这个 threshold 时，我们认为它为正样本，否则为负样本。举例来说，对于图中的第 4 个样本，其“Score”值为 0.6，那么样本 1，2，3，4 都被认为是正样本，因为它们的“Score”值都大于等于 0.6，而其他样本则都认为是负样本。每次选取一个不同的 threshold，我们就可以得到一组 FPR 和 TPR，即 ROC 曲线上的一点。这样一来，我们一共得到了 20 组 FPR 和 TPR 的值，将它们画在 ROC 曲线的结果如下图：

![image-20180731210622249](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180731210622249.png)

AUC（Area under Curve）：ROC 曲线下的面积，介于 0.1 和 1 之间。AUC 作为数值可以直观地评价分类器的好坏，值越大越好。

首先 AUC 是一个概率值，当你随机挑选一个正样本以及负样本，当前的分类算法根据计算得到的 Score 值将这个正样本排在负样本前面的概率就是 AUC 值，AUC 值越大，当前分类算法越有可能将正样本排在负样本前面，从而能够更好地分类。



## 为什么使用 ROC 和 AUC 评价分类器

ROC 曲线有个很好的特性：当测试集中的正负样本的分布变换的时候，ROC 曲线能够保持不变。在实际的数据集中，经常会出现样本类不平衡，即正负样本比例差距较大，而且测试数据中的正负样本也可能随着时间变化。下图是 ROC 曲线和 Pression-Recall 曲线的对比：

![image-20180731211740156](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180731211740156.png)

在上图中，（a）和（c）为 ROC 曲线，（b）和（d）为 Precision-Recall 曲线。

（a）和（b）展示的是分类器在原始测试集（正负样本分布平衡）的结果，（c）（d）是将测试集中负样本的数量增加到原来的 10 倍后，分类器的结果。可以明显地看出，ROC 曲线基本保持原貌，而 Precision-Recall 曲线变化较大。





# PR 曲线

在显著目标提取中，PR 曲线是用来评估模型性能的重要指标。

## 原理

PR 曲线中的 P（Precision）和 R（Recall）分别意为“查准率”和“查全率”。以“查准率”为纵坐标，“查全率”为横坐标所做的曲线即为 PR 曲线：

![image-20180801105041310](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801105041310.png)



## 计算方法

将显著性目标图谱 S 进行二值化得到 M，通过如下公式逐像素对比 M 与 Ground-truth 来计算 Precision 值与 Recall 值：

![image-20180801110228730](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801110228730.png)

如下图所示，模型输出的显著性图片与 Ground-truth 图在一般情况下都不会完全相同，即模型所提取出的显著性图除了正确被分类的目标（TP）与背景（TN）外，会将一部分本应该是背景的区域划到目标区域（FP），将一部分本应该是目标的区域化为背景区域（FN）：

![image-20180801110700283](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801110700283.png)

通过统计，获得TP、TN、FP、FN的数目，然后利用下式来计算 Precision 值与 Recall 值：

![image-20180801110844761](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801110844761.png)



## 画法

每取一个阈值，即可算得一组相应的 Precision 值与 Recall 值。以 Recall 为横坐标，Precision 为纵坐标绘制曲线图，即可得到 PR 曲线。





# SVM

支持向量机（Support Vector Machine）

## 了解 SVM

### 什么是支持向量机 SVM

分类作为数据挖掘领域中一项非常重要的任务，其目的是学会一个分类函数或分类器，而支持向量机本身便是一种监督式学习的方法，它广泛应用于统计分类及回归分析中。

SVM 是 90 年代中期发展起来的基于统计学习理论的一种机器学习方法，通过寻求结构化风险最小来提高学习机泛化能力，实现经验风险和置信范围的最小化，从而达到在统计样本量较少的情况下，亦能获得良好统计规律的目的。

通俗来讲，它是一种二分类模型，其基本模型定义为特征空间上的间隔最大的线性分类器，即支持向量机的学习策略便是间隔最大化，最终可转化为一个凸二次规划问题的求解。



### 线性分类

线性分类器（也叫做感知机，这里的机表示一种算法）

- 分类标准

考虑一个两类的分类问题，数据点用 x 来表示（这是一个 n 维向量），w^T 中的 T 代表转置，而类别用 y 来表示，可以取 1 或 -1，分别代表两个不同的类。

一个线性分类器的学习目标就是要在 n 维的数据空间中找到一个分类超平面，其方程可以表示为：

![image-20180801214006762](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801214006762.png)

上面给出了线性分类的定义描述，但为何用 y 取 1 或者 -1来表示两个不同的类别呢？其实，1 或者 -1 的分类标准起源于 logistic 回归。

- 1 或者 -1 分类标准的起源：logistic 回归

Logistic 回归目的是从特征学习出一个 0/1 分类模型，而这个模型是将特性的线性组合作为自变量，由于自变量的取值范围是负无穷到正无穷。因此，使用 logistic 函数（或称作 sigmoid 函数）将自变量映射到（0，1）上，映射后的值被认为是属于 y=1 的概率。

形式化表示即，假设函数：

![image-20180801214639519](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801214639519.png)

其中 x 是 n 维特征向量，函数 g 就是 logistic 函数。

而![image-20180801214735425](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801214735425.png)的图像是：

![image-20180801214849963](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801214849963.png)

可以看到，将无穷映射到了（0，1）。

而假设函数就是特征属于 y=1 的概率：

![image-20180801215000067](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801215000067.png)

![image-20180801215115682](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801215115682.png)

![image-20180801215213876](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801215213876.png)

- 形式化表示

这次使用的结果标签是 y=-1，y=1，替换在 logistic 回归中使用的 y=0

![image-20180801215644194](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801215644194.png)

与 logistic 回归的形式化表示没区别。

![image-20180801220116371](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801220116371.png)

于此，想必已经解释明白了为何线性分类的标准一般用 1 或者 -1 来表示。



### 线性分类的一个例子

一个二维平面（一个超平面，在二维空间中的例子就是一条直线），如下图所示，平面上有两种不同的点，分别用两种不同的颜色表示，一种为红颜色的点，另一种则为蓝颜色的点，红颜色的线表示一个可行的超平面。

![image-20180801220430566](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801220430566.png)

从上图可以看出，红颜色的线把红颜色的点和蓝颜色的点分开来。而这条红颜色的线就是所说的超平面，即所谓的超平面的的确确把这两种不同颜色的数据点分隔开来，在超平面一边的数据点所对应的 y 全是 -1，而在另一边全是 1。

接着，可以令分类函数：

![image-20180801220718834](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801220718834.png)

显然，若 f(x) = 0，那么 x 是位于超平面上的点。不妨要求对于所有满足 f(x) \< 0 的点，其对应的 y 等于 -1，而 f(x) > 0 则对应 y=1 的数据点。

有些时候，或者说大部分时候数据并不是线性可分的，此时，满足这样条件的超平面根本不存在。这里先从最简单的情形开始推导，假设数据都是线性可分的，即这样的超平面是存在的。

下面的篇幅将按下述 3 点走：

1.需要确定上述分类函数 f(x)=w.x+b（w.x 表示 w 与 x 的内积）中的两个参数 w 和 b。

2.如何确定 w 和 b 呢？答案是寻找两条边界端或极端划分直线中间的最大间隔（之所以要寻最大间隔是为了能更好地划分不同的点），从而确定最终的最大间隔分类超平面 hyper plane 和分类函数。

3.进而把寻求分类函数 f(x)=w.x+b 的问题转化为对 w，b 的最优化问题，最终转化为对偶因子的求解。

总结：从最大间隔出发，转化为求对变量 w 和 b 的凸二次规划问题。



### 函数间隔 Functional margin 与几何间隔 Geometrical margin

一般而言，一个点距离超平面的远近可以表示为分类预测的确信或准确程度。

在超平面 w*x+b=0 确定的情况下，|w\*x+b|能够相对地表示点 x 距离超平面的远近，而 w\**x+b 的符号与类标记 y 的符号是否一致表示分类是否正确。所以，可以用 y*(w\*x+b) 的正负性来判定或表示分类的正确性和确信度。

于此，便引出了定义样本到分类间隔距离的函数间隔的概念。

- 函数间隔 Functional margin

定义函数间隔 functional margin 为：

![image-20180802094105913](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802094105913.png)

定义超平面（w，b）关于训练数据集 T 的函数间隔为超平面（w，b）关于 T 中所有样本点（xi,yi）的函数间隔最小值。其中，x 是特征，y 是结果标签，i 表示第 i 个样本，有：

![image-20180802094301783](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802094301783.png)

与此同时，问题就出现了。上述定义的函数间隔虽然可以表示分类预测的正确性和确信度，但在选择分类超平面时，只有函数间隔还远远不够。因为若成比例地改变 w 和 b，如变为 2w 和 2b，虽然此时超平面没有改变，但函数间隔的值却变成了原来的 2 倍。

其实，可以对法向量 w 加些约束条件，使其表面上看起来规范化。如此，引出真正定义点到超平面的距离——几何间隔的概念。

- 点到超平面的距离定义：几何间隔 Geometrical margin 

![image-20180802094942947](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802094942947.png)

给出几何间隔的定义前，如上图所示，对于一个点 x，令其垂直投影到超平面，对应点为 x0，由于 w 是垂直于超平面的一个向量，有：

![image-20180802095359361](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802095359361.png)

又由于 x0 是超平面上的点，满足 f(x0)=0，代入超平面方程即可算出：

![image-20180802100046091](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802100046091.png)

不过这里的结果是带符号的，我们需要的只是它的绝对值。因此，类似地，也乘上对应的类别 y 即可。实际定义的几何间隔为：

![image-20180802100435797](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802100435797.png)

函数间隔 y*(w\*x+b)=y\**f(x) 实际上就是 |f(x)|，只是人为定义的一个间隔度量；而几何间隔 |f(x)|/||w|| 才是直观上的点到超平面的距离。



### 最大间隔分类器 Maximum Margin Classifer 的定义

于此，很明显地看出，函数间隔和几何间隔相差一个||w||的缩放因子。按照之前的分析，对一个数据点进行分类，当它的 margin 越大时，分类的 confidence 越大。对于一个包含 n 个点的数据集，可以很自然地定义它的 margin 为所有 n 个点中 margin 值最小的那个。于是，为了使得分类的 confidence 高，希望所选择的超平面的能够最大化这个 margin 值。

已知：

1.functional margin 明显是不太适合用来最大化的一个量。因为在 hyper plane 固定以后，可以等比例地缩放 w 的长度和 b 的值，使得 f(x) 的值任意大，亦即 functional margin 可以在 hyper plane 保持不变的情况下被取得任意大。

2.而geometrical margin 则没有这个问题，因为除了 ||w|| 这个分母，所以缩放 w 和 b 的时候，它的值是不会改变的，它只随着 hyper plane 的变动而变动。因此，这是更加适合的一个 margin。

这样一来，maximum margin classifier 的目标函数可以定义为：

![image-20180802101702608](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802101702608.png)

当然，还需要满足一些条件，根据 margin 的定义，有：

![image-20180802101831307](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802101831307.png)

处于方便推导和优化的目的，可以令：

![image-20180802102050239](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802102050239.png)

此时，目标函数转化为：

![image-20180802102139803](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802102139803.png)

其中，s.t. 即 subject to，导出约束条件。

通过求解这个问题，可以找到一个 margin 最大的 classifier，如下图所示，中间的红色线条是 Optimal Hyper Plane，另外两条线到红线的距离都是等于几何间隔的：

![image-20180802102427961](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802102427961.png)

通过最大化 margin，使得该分类器对数据进行分类时具有了最大的 confidence，从而设计决策最优分类超平面。



### 到底什么是 Support Vector

回忆一下下图：

![image-20180802102623011](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802102623011.png)

可以看到两个支撑着中间的 gap 的超平面，它们到中间的纯红线 separating hyper plane 的距离相等，即我们所能得到的最大的几何间隔，而“支撑”这两个超平面的必定会有一些点，而这些“支撑”的点便叫做支撑向量 Support Vector。

如下图，Support Vector 便是蓝色虚线和粉红色虚线上的点：

![image-20180802103014375](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802103014375.png)

很显然，由于这些 Supporting Vector 刚好在边界上，所以他们满足：

![image-20180802103101612](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802103101612.png)

，而对于所有不是支撑向量的点，也就是在“阵地后方”的点，则显然有：

![image-20180802103159883](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802103159883.png)





## 深入 SVM

### 从线性可分到线性不可分

- 从原始问题到对偶问题的求解

回忆一下得到的目标函数：

![image-20180802103403523](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802103403523.png)

上述问题等价于：

![image-20180802103517686](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802103517686.png)

1.转化到这个形式后，问题成为了一个凸优化问题。或者更具体地说，因为现在的目标函数是二次的，约束条件是线性的，所以它是一个凸二次规划问题。这个问题可以用任何现成的 QP 的优化包进行求解，归结为一句话即是：在一定的约束条件下，目标最优，损失最小。

2.虽然这个问题确实是一个标准的 QP 问题，但是它也有它的特殊结构，通过 Lagrange Duality 变换到对偶变量 dual variable 的优化问题之后，可以找到一种更加有效的方法来进行求解。而且，通常情况下，这种方法比直接使用通用的 QP 优化包进行优化要高效得多。

也就是说，除了用解决 QP 问题的常规方法之外，还可以通过求解对偶问题得到最优解，这就是线性可分条件下支持向量机的对偶算法。这样做的优点在于：一，对偶问题往往更容易求解；二，可以自然地引入核函数，进而推广到非线性分类问题。

至于上述提到，关于什么是 Lagrange duality？简单地说，通过给每一个约束条件加上一个 Lagrange multiplier（拉格朗日乘值），即引入拉格朗日乘子 α，可将约束条件融合到目标函数里去：

![image-20180802104452140](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802104452140.png)

然后令：

![image-20180802104535808](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802104535808.png)

容易验证，当某个约束条件不满足时，例如：

![image-20180802104647453](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802104647453.png)

显然有：

![image-20180802104727464](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802104727464.png)

而当所有约束条件都满足时，有：

![image-20180802104811674](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802104811674.png)

即我们最初要最小化的量。当然，这里也有约束条件，就是 αi>=0。现在的目标函数变成了：

![image-20180802105057369](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802105057369.png)

现在，把最小和最大的位置交换一下：

![image-20180802112605795](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802112605795.png)

当然，交换后的问题不再等价于原问题，并且有：

![image-20180802112711254](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802112711254.png)

总之，第二个问题的最优值在这里提供了一个第一个问题的最优值的下界。在满足某些条件的情况下，这两者相等，这时就可以通过求解第二个问题来间接地求

解第一个问题。

之所以从 minmax 的原始问题转化为 maxmin 的对偶问题，一是因为两者为近似解，二是转化为对偶问题后，更容易求解。

- KKT 条件

所谓的“满足某些条件”就是要满足 KKT 条件。那 KKT 条件的表现形式是什么呢？

一般地，一个最优化数学模型能够表示成下列标准形式：

![image-20180802113446894](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802113446894.png)

其中，f(x) 是需要最小化的函数，h(x) 是等式约束，g(x) 是不等式约束，p 和 q 分别为等式约束和不等式约束的数量。同时，需要明白以下内容：

![image-20180802113644298](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802113644298.png)

KKT 条件的意义：它是一个非线性规划问题能有最优化解法的必要和充分条件。

那到底什么是所谓 Karush-Kuhn-Tucker 条件呢？KKT 条件就是指上面最优化数学模型的标准形式中的最小点 x* 必须满足下面的条件：

![image-20180802114020529](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802114020529.png)

经过论证，这里的问题是满足 KKT 条件的，因此转化为求解第二个问题。而求解这个对偶学习问题，分为 3 个步骤。首先让 L(w,b,α) 关于 w 和 b 最小化，然后求对 α 的极大，最后利用 SMO 算法求解对偶因子。

- 对偶问题求解的 3 个步骤

1.首先固定 α，要让 L 关于 w 和 b 最小化，分别对 w，b 求偏导数：

![image-20180802114544205](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802114544205.png)

以上结果代回上述的 L：

![image-20180802114614380](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802114614380.png)

得到：

![image-20180802114737513](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802114737513.png)

具体推导过程如下：

![image-20180802115116676](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802115116676.png)

可以看出，此时的拉格朗日函数只包含了一个变量，那就是 αi。

2.求对 α 的极大，即是关于对偶问题的最优化问题，从上面的式子可知，通过：

![image-20180802115638085](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802115638085.png)

可求出 w，通过：

![image-20180802115706341](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802115706341.png)

可求出 b。

目标函数转化为：

![image-20180802120025431](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802120025431.png)

这个问题有更加高效的优化算法，即 SMO 算法。

- 序列最小最优化 SMO 算法

当：

![image-20180802165404214](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802165404214.png)

要解决的是在参数 α 上求最大值 W 的问题，至于 x 和 y 都是已知数。其中，C 是一个参数，用于控制目标函数中两项（“寻找 margin 最大的超平面”和“保证数据点偏差量最小”）之间的权重。

- 线性不可分的情况

关于 hyper plane，对一个数据点 x 进行分类，实际上是通过把 x 带入到：

![image-20180802165922208](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802165922208.png)

算出结果，然后根据其正负号来进行类别划分的。而前面的推导中得到：

![image-20180802170014613](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802170014613.png)

因此，分类函数为：

![image-20180802170048887](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802170048887.png)

这里的形式的有趣之处在于，对于新点 x 的预测，只需要计算它与训练数据点的内积即可。这一点至关重要，是之后使用 Kernel 进行非线性推广的基本前提。此外，所谓 Supporting Vector 也在这里显示出来——事实上，所有非 Supporting Vector 所对应的系数 α 都是等于 0 的，因此对于新点的内积计算实际上只要针对少量的“支持向量”而不是所有的训练数据即可。

为什么非支持向量对应的 α 等于 0 呢？直观上来理解的话，就是这些“后方”的点对超平面是没有影响的，由于分类完全由超平面决定，所以这些无关的点不会参与分类问题的计算，因而也就不会产生任何影响了。

回忆通过 Lagrange multiplier 得到的目标函数：

![image-20180802170816196](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802170816196.png)

注意到，如果 xi 是支持向量的话，上式中红颜色的部分是等于 0 的（因为支持向量的 functional margin 等于 1）。而对于非支持向量来说，functional margin 会大于 1，因此红颜色部分是大于 0 的，而 αi 又是非负的，为了满足最大化，αi 必须等于 0。这也就是这些非 Supporting Vector 的点的局限性。

综上，便得到了一个 maximum margin hyper plane classifier，这就是所谓的支持向量机。当然，到目前为止，我们的 SVM 还比较弱，只能处理线性的情况。不过，在得到了对偶 dual 形式之后，通过 Kernel 推广到非线性的情况就变成了一件非常容易的事情了。



### 核函数 Kernel

- 特征空间的隐式映射：核函数

已经了解到了 SVM 处理线性可分的情况，而对于非线性的情况，SVM 的处理方法是选择一个核函数，通过将数据映射到高维空间，来解决在原始空间中线性不可分的问题。由于核函数的优良品质，这样的非线性扩展在计算量上并没有比原来复杂多少，这一点是非常难得的。当然，这要归功于核方法——除了 SVM 外，任何将计算表示为数据点的内积的方法，都可以使用核方法进行非线性扩展。

MInsky 和 Papert 在 20 世纪 60 年代就已经明确指出线性学习器计算能力有限。为什么呢？因为总体上来讲，现实世界复杂的应用需要有比线性函数更富有表达能力的假设空间。也就是说，目标概念通常不能由给定属性的简单线性函数组合产生，而是应该寻找待研究数据的更为一般化的抽象特征。

而核函数则提供了此种问题的解决途径，它通过把数据映射到高维空间来增加线性学习器的能力，使得线性学习器对偶空间的表达方式让分类操作更具灵活性和可操作性。因为训练样例一般是不会独立出现的，它们总是以成对样例的内积形式出现，而用对偶形式表示学习器的优势为在该表示中可调参数的个数不依赖输入属性的个数，通过使用恰当的核函数来替代内积，可以隐式地将非线性的训练数据映射到高维空间，而不增加可调参数的个数。

简而言之：在线性不可分的情况下，支持向量机通过某种事先选择的非线性映射（核函数）将输入变量映射到一个高维特征空间，在这个空间中构造最优分类超平面。使用 SVM 进行数据集分类工作的过程首先是同预先选定的一些非线性映射将输入空间映射到高维特征空间：

![image-20180802172815367](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802172815367.png)

使得在高维属性空间中有可能就训练数据实现超平面的分割，避免了在原输入空间中进行非线性曲面分割计算。SVM 数据集形成的分类函数具有这样的性质：它是一组以支持向量为参数的非线性函数的线性组合，因此分类函数的表达式仅和支持向量的数量有关，而独立于空间的维度，在处理高维输入空间的分类时，这种方法尤其有效，其工作原理如下图所示：

![image-20180802173158249](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802173158249.png)

具体点说，在遇到核函数之前，如果用原始的方法，那么在用线性学习器学习一个非线性关系，需要选择一个非线性特征集，并且将数据写成新的表达形式，这等价于应用一个固定的非线性映射，将数据映射到特征空间，在特征空间中使用线性学习器。因此，考虑的假设集是如下类型的函数：

![image-20180802173430943](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802173430943.png)



这里，![image-20180802173520893](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802173520893.png)X->F 是从输入空间到某个特征空间的映射，这意味着建立非线性学习器分为两步：

1.首先使用一个非线性映射将数据变换到一个特征空间 F。

2.在特征空间使用线性学习器分类。

而对偶形式就是线性学习器的一个重要性质，这意味着假设可以表达为训练点的线性组合。因此，决策规则可以用测试点和训练点的内积来表示：

![image-20180802173840967](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802173840967.png)

如果有一种方式可以在特征空间中直接计算内积，就像在原始输入点的函数中一样，就有可能将两个步骤融合到一起建立一个非线性的学习器，这样直接计算的方法称为核函数方法。于是，核函数便横空出世了：

![image-20180802174040202](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802174040202.png)

- 核函数：如何处理非线性数据

介绍过线性情况下的支持向量机，它通过寻找一个线性的超平面来达到对数据进行分类的目的。不过，由于是线性方法，所以对非线性的数据就没有办法处理。举个例子，如下图所示的两类数据，分别分布为两个圆圈的形状，这样的数据本身就是线性不可分的，此时如何把这两类数据分开呢？

![image-20180802174453139](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802174453139.png)

事实上，上图所示的这个数据集，使用两个半径不同的圆圈加上了少量的噪音生成得到的。所以，一个理想的分界应该是一个“圆圈“，而不是一条线。如果用 X1 和 X2 来表示这个二维平面的一个坐标的话，一条二次曲线（圆圈是二次曲线的一种特殊情况）的方程可以写作这样的形式：

![image-20180802175007310](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802175007310.png)

如果构造另外一个五维的空间，其中 5 个坐标的值分别为 Z1、Z2、Z3、Z4、Z5，上面的方程在新的坐标系下可以写作：

![image-20180802175442324](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802175442324.png)

关于新的坐标 Z，这正是一个 hyper plane 的方程！也就是说，若做一个映射![image-20180802175531157](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802175531157.png)，将 X 按照上面的规则映射为 Z，那么在新的空间中原来的数据将变成线性可分的，从而使用之前推导的线性分类算法就可以进行处理，这正是 Kernel 方法处理非线性问题的基本思想。

假设原始的数据是非线性的，通过一个映射将其映射到一个高维空间中，数据变得线性可分了。这时，我们就可以使用原来的推导来进行计算，只是所有的推导是在新的空间，而不是原始空间中进行。当然，推导过程也并不是可以简单地直接类比的。例如，原本我们要求超平面的法向量 w，但是如果映射之后得到的新空间的维度是无穷维的（确实会出现这样的情况，比如后面会提到的高斯核 Gaussian Kernel），要表示一个无穷维的向量，描述起来会比较麻烦。不妨先忽略过这些细节，直接从最终的结论来分析：

![image-20180802180337436](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802180337436.png)

而其中的 α 也是通过求解如下 dual 问题而得到的：

![image-20180802180431552](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802180431552.png)

这样一来问题就解决了吗？似乎是的：拿到非线性数据，找到一个映射把原来的数据映射到新空间中，再做线性 SVM 即可。不过，事实上没有这么简单！在最初的例子里，对一个二维空间做映射，选择的新空间是原始空间的所有一阶和二阶的组合，得到了 5 个维度；如果原始空间是三维，会得到 19 个维的新空间，这个数目是呈爆炸性增长的，而且如果遇到无穷维的情况，就根本无从计算。所以，就需要 Kernel 出马了。

![image-20180802180843289](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802180843289.png)

又注意到：

![image-20180802181159549](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802181159549.png)

二者有很多相似的地方，实际上，只要把某几个维度线性缩放一下，然后再加上一个常数维度。具体来说，上面这个式子的计算结果实际上和映射：

![image-20180802182603882](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802182603882.png)

之后的内积的结果是相等的，那么区别在于什么地方呢？

一个是映射到高维空间中，然后再根据内积的公式进行计算；而另一个则直接在原来的低维空间中进行计算，而不需要显示地写出映射后的结果。

回忆之前提到的映射的维度爆炸，在前一种方法已经无法计算的情况下，后一种方法却依旧能从容处理，甚至是无穷维度的情况也没有问题。

把这里的计算两个向量在隐式映射的空间中的内积的函数叫做核函数（Kernel Function）。例如，在刚才的例子中，核函数为：

![image-20180802183048409](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802183048409.png)

核函数能简化映射空间中的内积运算——刚好”碰巧“的是，在 SVM 里需要计算的地方，数据向量总是以内积的形式出现。对比刚才写出来的式子，现在的分类函数为：

![image-20180802183208089](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802183208089.png)

其中 α 由如下 dual 问题计算而得：

![image-20180802183242444](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802183242444.png)

这样一来计算的问题就算解决了，避开了直接在高维空间中进行计算，而结果却是等价的！当然，这里的例子非常简单，所以可以手工构造出对应的核函数，如果对于任意一个映射，想要构造出对应的核函数就很困难了。

- 几个核函数

通常人们会从一些常用的核函数中选择（根据问题和数据的不同，选择不同的参数，实际上就是得到了不同的核函数），例如：

1.多项式核，空间的维度是![image-20180802183645172](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802183645172.png)，其中，m 是原始空间的维度。

2.高斯核：![image-20180802183724710](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802183724710.png)

这个核就是之前提到过的会将原始空间映射为无穷维空间的函数。不过，如果 α 选得很大，高次特征上的权重实际上衰减得非常快，所以近似相当于一个低维的子控件：反过来，如果 α 选得很小，则可以将任意的数据映射为线性可分——当然，这并不一定是好事，因为随之而来的可能是非常严重的过拟合问题。不过，总的来说，通过调控参数 α，高斯核实际上具有相当高的灵活性，也是使用最广泛的核函数之一。下图所示的例子，便是把低维线性不可分的数据通过高斯核函数映射到了高维空间：

![image-20180802184318316](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802184318316.png)

线性核实际上就是原始空间中的内积。这个核存在的主要目的是使得”映射后空间中的问题“和”映射前空间中的问题“两者在形式上统一起来了。意思是说，在写代码或者写公式时，只要写个模板或通用表达式，然后再代入不同的核，便可以了。于此，在形式上统一了起来，不用再分别写一个线性的和一个非线性的。

- 核函数的本质

以下三点：

1.实际中，会将经常遇到线性不可分的样例。此时，常用的做法是把样例特征映射到高维空间中去；

2.但进一步，如果凡是遇到线性不可分的样例，一律映射到高维空间，那么这个维度大小会是高到可怕的；

3.此时，核函数就隆重登场了，核函数的价值在于它虽然也是将特征进行从低维到高维的转换。但它事先在低维上进行计算，而将实质上的分类效果表现在了高维上，避免了直接在高维空间中的复杂计算。

已知，当把内积变成![image-20180802185048042](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185048042.png)后，求解将有两种方法：

1.先找到这种映射，然后将输入空间中的样本映射到新的空间中，最后在新空间中去求内积![image-20180802185145873](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185145873.png)。例如，多项式：

![image-20180802185257625](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185257625.png)

对其进行变换：

![image-20180802185334237](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185334237.png)

得到：

![image-20180802185349428](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185349428.png)

也就是说，通过把输入空间从二维向四维映射后，样本由线性不可分变成了线性可分，这种转化带来的直接问题是维度变高了。这意味着，首先可能导致后续计算变复杂，其次可能出现维度之咒，对于学习器而言就是：特征空间维数最终可能无法计算，而它的泛化能力（学习器对训练样本以外数据的适应性）会随着维度的增长而大大降低。最终可能会使得内积无法求出，于是失去了这种转化的优势。

2.或者是找到某种方法，它不需要显示地将输入空间中的样本映射到新的空间中，而能够在输入空间中直接计算出内积。它其实是对输入空间向高维空间的一种隐式映射，不需要显式地给出那个映射，再输入空间就可以计算![image-20180802185849775](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802185849775.png)，这就是核函数方法。

最后，引用一个例子举例说明核函数解决非线性问题的直观效果。

假设你是一个农场主，圈养了一批羊群，但是为预防狼群袭击，需要搭建一个篱笆把羊群围起来。但是篱笆应该建在哪里呢？可能需要依据羊群和狼群的位置建立一个”分类器“，比较下图这几种不同的分类器，可以看到 SVM 完成了一个很完美的解决方案：

![image-20180802190229942](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180802190229942.png)

这个例子从侧面简单说明了 SVM 使用非线性分类器的优势，而逻辑模式和决策树模式都是使用了直线方法。



### 使用松弛变量处理 outliers 

有时并不是因为数据本身是非线性结构的，而只是因为数据有噪音。对于这种偏离正常位置很远的数据点，称之为 outlier。在原来的 SVM 模型里，outlier 的存在有可能造成很大的影响，因为超平面本身就是只有少数几个 Support Vector 决定的，如果这些 Support Vector 里又存在 outlier 的话，其影响就很大了。如下图：

![image-20180803103253798](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803103253798.png)

用黑圈圈起来的蓝点是一个 outlier，它偏离了自己原本所应该在的那半个空间，如果直接忽略掉它，原来的分隔超平面还是挺好的，但是由于 outlier 的出现，导致分隔超平面不得不歪曲，同时 margin 也相应变小了。当然，更严重的情况是，如果这个 outlier 再往右上移动一些距离，将无法构造出能将数据分开的超平面。

为了处理这种情况，SVM 允许数据点在一定程度上偏离超平面。例如上图中，黑色实线所对应的距离，就是 outlier 偏离的距离。如果把它移动回来，就刚好落在原来的超平面上，从而不会使得超平面发生变形。

原来的约束条件为：

![image-20180803104212641](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104212641.png)

现在考虑到 outlier 问题，约束条件变成了：

![image-20180803104256577](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104256577.png)

其中：

![image-20180803104310738](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104310738.png)

称为松弛变量 slack variable，对应数据点 Xi 允许偏离的 functional margin 的量。当然，如果松弛变量任意大的话，那么任意的超平面都是符合条件的了。所以，在原来的目标函数后面加上一项，使得这些松弛变量的总和也要最小：

![image-20180803104517463](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104517463.png)

其中，C 是一个参数，用于控制目标函数中两项（”寻找 margin 最大的超平面“和”保证数据点偏差量最小“）之间的权重。注意，其中松弛变量是需要优化的变量之一，而 C 是一个事先确定好的常量。完整写出来，如下：

![image-20180803104721433](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104721433.png)

用之前的方法将限制或约束条件加入到目标函数中，得到新的拉格朗日函数，如下：

![image-20180803105003796](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803105003796.png)

分析方法和前面一样：

![image-20180803104858592](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803104858592.png)

代回 L 并化简，得到和原来一样的目标函数：

![image-20180803105128835](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803105128835.png)

由于有 ri>=0（作为 Lagrange multiplier 的条件），因此有 αi<=C，所以整个 dual 问题写作：

![image-20180803105346176](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803105346176.png)

前后结果唯一的区别就是现在 dual variable α 多了一个上限 C，而 Kernel 化的非线性形式也是一样的。





## 证明 SVM

### 线性学习器

- 感知机算法

目的：不断训练以期寻找一个合适的超平面。

![image-20180803110143345](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110143345.png)

如下图所示，凭直觉可以看出，图中的红线是最优超平面，最终，若蓝线能通过不断的训练移动到红线位置上，则代表训练成功。

![image-20180803110321869](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110321869.png)

既然需要通过不断的训练让蓝线最终成为最优分类超平面，那么，到底需要训练多少次呢？Novikoff 定理说明当间隔是正的时候，感知机算法会在有限次数的迭代中收敛：

![image-20180803110505909](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110505909.png)

![image-20180803110539117](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110539117.png)

根据误分次数公式可知，迭代次数与对应于扩充权重的训练集的间隔有关。

扩充间隔即为样本到分类间隔的距离，即从扩充间隔引出的最大分类间隔：

![image-20180803110739431](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110739431.png)

![image-20180803110824400](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803110824400.png)

同时有一点值得注意：感知机算法虽然可以通过简单迭代对线性可分数据生成正确分类的超平面，但不是最优效果。怎样才能得到最优效果呢？就是之前曾解释过的寻找最大分类间隔超平面。



### 非线性学习器

- Mercer 定理

![image-20180803111123387](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803111123387.png)

 

### 损失函数

曾有这样一段话”SVM 是 90 年代中期发展起来的基于统计学习理论的一种机器学习方法，通过寻求结构化风险最小来提高学习机泛化能力，实现经验风险和置信范围的最小化，从而达到在统计样本量较少的情况下，亦能获得良好统计规律的目的。“什么是结构化风险？什么又是经验风险？要了解这两个所谓的”风险“，还得从监督学习说起。

监督学习实际上就是一个经验风险或结构风险函数的最优化问题。风险函数度量平均意义下模型预测的好坏，模型每一次预测的好坏用损失函数来度量。它从假设空间 F 中选择模型 f 作为决策函数，对于给定的输入 X，由 f(X) 给出相应的输出 Y，这个输出的预测值 f(X) 与真实值 Y 可能一致也可能不一致，用一个损失函数来度量预测错误的程度。损失函数记为 L(Y,f(X))。

常用的损失函数有以下几种：

![image-20180803111955115](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803111955115.png)

给定一个训练数据集：

![image-20180803112018213](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803112018213.png)

模型 f(X) 关于训练数据集的平均损失称为经验风险，如下：

![image-20180803112656014](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803112656014.png)

关于如何选择模型，监督学习有两种策略：经验风险最小化和结构风险最小化。

经验风险最小化的策略认为，经验风险最小的模型是最优的模型，即求解如下最优化问题：

![image-20180803113332965](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803113332965.png)

当样本容量很小时，经验风险最小化的策略容易产生过拟合的现象。结构风险最小化可以防止过拟合。结构风险是在经验风险的基础上加上表示模型复杂度的正则化项或罚项，结构风险定义如下：

![image-20180803113641565](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803113641565.png)

其中 J(f) 为模型的复杂度，模型 f 越复杂，J(f) 值就越大，也就是说 J(f) 是对复杂模型的惩罚。λ>=0 是系数，用以权衡经验风险和模型复杂度。结构风险最小化的策略认为结构风险最小的模型是最优的模型，所以求最优的模型就是求解下面的最优化问题：

![image-20180803113950969](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803113950969.png)

这样，监督学习问题就变成了经验风险或结构风险函数的最优化问题。



### 最小二乘法

- 什么是最小二乘法？

口头中经常说：一般来说，平均来说。如平均来说，不吸烟的健康优于吸烟者，之所以要加”平均“二字，是因为凡事皆有例外，总存在某个特别的人他吸烟但由于经常锻炼，他的健康状况可能会优于他身边不吸烟的朋友。而最小二乘法的一个最简单的例子便是算术平均。

最小二乘法（又称最小平方法）是一种数学优化技术。它通过最小化误差的平方来寻找数据的最佳函数匹配。利用最小二乘法可以简便地求得未知的数据，并使得这些求得的数据与实际数据之间误差的平方和为最小。用函数表示：

![image-20180803115251010](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803115251010.png)

使误差平方和达到最小以寻求估计值的方法，就叫做最小二乘法。用最小二乘法得到的估计，叫做最小二乘估计。当然，取平方和作为目标函数只是众多可取的方法之一。

有效的最小二乘法是勒让德在 1805 年发表的，其基本思想就是认为测量中有误差，所以所有方程的累计误差为：

![image-20180803120300596](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803120300596.png)

求解出导致累计误差最小的参数即可：

![image-20180803120415871](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803120415871.png)

勒让德在论文中对最小二乘法的优良性做了几点说明：

1.最小二乘使得误差平方和最小，并在各个方程的误差之间建立了一种平衡，从而防止某一个极端误差取得支配地位。

2.计算中只要求偏导后求解线性方程组，计算过程明确便捷

3.最小二乘可以导出算术平均值作为估计值。

对于最后一点，从统计学的角度来看是很重要的一个性质。推理如下：![image-20180803120743727](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803120743727.png)

由于算术平均是一个历经考验的方法，而以上的推理说明，算术平均是最小二乘的一个特例，所以从另一个角度说明了最小二乘方法的优良性。

本质上说，最小二乘法是一种参数估计方法。就参数估计，得从一元线性模型说起。

- 最小二乘法的解法

什么是一元线性模型呢？先来梳理几个基本概念：

1.监督学习中，如果预测的变量是离散的称其为分类（如决策树，支持向量机等），如果预测的变量是连续的，称其为回归。

2.回归分析中，如果只包含一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。

3.如果回归分析中包括两个或两个以上的自变量，且因变量和自变量之间是线性关系，则称为多元线性回归分析。

4.对于二维空间是一条直线；对于三维空间是一个平面；对于多维空间线性是一个超平面。

对于一元线性回归模型，假设从总体中获取了 n 组观察值(X1,Y1)，(X2,Y2)，...，(Xn,Yn)。对于平面中的这 n 个点，可以使用无数条曲线来拟合。要求样本回归函数尽可能好地拟合这组值。综合起来，这条直线处于样本数据的中心位置最合理。

选择最佳拟合曲线的标准可以确定为，使总的拟合误差（即总残差）达到最小。有以下三个标准可以选择：

1.用“残差和最小”确定直线位置为一个途径。但很快发现计算“残差和”存在相互抵消的问题。

2.用“残差绝对值和最小”确定直线位置也是一个途径。但绝对值的计算比较麻烦。

3.最小二乘法的原则是以“残差平方和最小”确定直线位置。用最小二乘法除了计算比较方便外，得到的估计量还具有优良特性。这种方法对异常值非常敏感。

最常用的是普通最小二乘法（Ordinary Least Square，OLS）：所选择的回归模型应该使所有观察值的残差平方和达到最小，即采用平方损失函数。

定义样本回归模型为：

![image-20180803161627981](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803161627981.png)

其中 ei 为样本的误差。

定义平方损失函数 Q：

![image-20180803161716942](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803161716942.png)

则通过 Q 最小确定这条直线，可求导得到：

![image-20180803161845226](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803161845226.png)

解得：

![image-20180803161947349](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803161947349.png)

求解最小二乘法与求解 SVM 问题何等相似，尤其是定义损失函数，然后通过偏导求得极值。



### SMO 算法

SMO 算法在 1988 年提出，很快成为最快的二次规划优化算法，特别针对线性 SVM 和数据稀疏时性能更优。

- SMO 算法的解法

定义特征到结果的输出函数：

![image-20180803162429928](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803162429928.png)

这个 u 与我们之前定义的 f(x) 实质是一样的。

重新定义原始的优化问题：

![image-20180803162530286](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803162530286.png)

求导得：

![image-20180803162608689](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803162608689.png)

代回得：

![image-20180803162902000](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803162902000.png)

引入对偶因子：

![image-20180803163030807](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163030807.png)

这里得到的 min 函数与之前的 max 函数实质是一样的，因为把符号改变了，即有 min 转化为 max 的问题。

加入松弛变量后，模型修改为： 

![image-20180803163207838](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163207838.png)

最终问题变为：

![image-20180803163244081](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163244081.png)

根据 KKT 条件可以得出其中 αi 取值的意义：

![image-20180803163354605](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163354605.png)

这里的 αi 是拉格朗日乘子。

1.对于第 1 种情况，表明 αi 是正常分类，在边界内部。

2.对于第 2 种情况，表明了 αi 是支持向量，在边界上。

3.对于第 3 种情况，表明了 αi 是在两条边界之间。

而最优解需要满足 KKT 条件，即上述 3 个条件都得满足，以下几种情况出现将会出现不满足：

![image-20180803163829436](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163829436.png)

所以要找出不满足 KKT 条件的这些 αi，并更新这些 αi，但这些 αi 又受到另外一个约束，即：

![image-20180803163951035](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803163951035.png)

因此，通过另一个方法，即同时更新 αi 和 αj，满足以下等式：

![image-20180803164051125](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803164051125.png)

就能保证和为 0 的约束。

消去 αi，得到一个关于单变量 αj 的凸二次规划问题。

乘子的选择务必遵循两个原则：

1.使乘子能满足 KKT 条件

2.对一个满足 KKT 条件的乘子进行更新，应能最大限度增大目标函数的值。

综上，SMO 算法的基本思想是每次迭代只选出两个分量 αi 和 αj 进行调整，其他分量则保持固定不变，在得到解 αi 和 αj 之后，再用它们改进其他分量。与通常的分解算法比较，尽管它可能需要更多的迭代次数，但每次迭代的计算量比较小，所以该算法表现出整体的快速收敛性，且不需要存储核矩阵，也没有矩阵运算。

- SMO 算法的实现

SVM 理解到了一定程度后，的确能在脑海里从头至尾推导出相关公式的，最初分类函数，最大化分类间隔![image-20180803172745433](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180803172745433.png)，凸二次规划，拉格朗日函数，转化为对偶问题，SMO 算法，都为寻找一个最优解，一个最优分类平面。

至于核函数则是为了更好地处理非线性可分的情况，而松弛变量则是为了纠正或约束少量“不安分”或脱离集体不好归类的因子。



### SVM 应用

SVM 在很多诸如文本分类，图像分类，生物序列分析和生物数据挖掘，手写字符识别等领域有很多的应用，但或许你并没强烈地意识到，SVM 可以成功应用的领域远超出现在已经在开发应用的领域。

- 文本分类

一个文本分类系统不仅是一个自然语言处理系统，也是一个典型的模式识别系统，系统的输入是需要进行分类处理的文本，系统的输出则是与文本关联的类别。由于篇幅所限，其他更具体内容本文将不再详述。





# CountVectorizer

对特征进行抽取和向量化。

在文本数据处理中，遇到的经常是一个个字符串。且对于中文来说，常要处理没有分隔符的大段原始的字符串（这种数据需要先分词，转化为分割好的字符串）。

对于这些数据会想到它们的哪些特点呢？词，统计它们包含词语的特点。

常用数据输入形式为：列表，列表元素为代表文章的字符串。一个字符串代表一篇文章，字符串是已经分割好的。

![image-20180824141045088](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180824141045088.png)

![image-20180824141118280](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180824141118280.png)

CounterVectorizer 是通过 fit_transform 函数将文本中的词语转换为词频矩阵。矩阵元素 a[i]\[j] 表示 j 词在第 i 个文本下的词频，即各个词语出现的次数。通过 get_feature_names() 可看到所有文本的关键字，通过 toarray() 可看到词频矩阵的结果。

![image-20180824141431911](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180824141431911.png)



## 对中文数据进行分词处理

~~~python
import jieba
a ="自然语言处理是计算机科学领域与人工智能领域中的一个重要方向。它研究能实现人与计算机之间用自然语言进行有效通信的各种理论和方法。自然语言处理是一门融语言学、计算机科学、数学于一体的科学"
b = "因此，这一领域的研究将涉及自然语言，即人们日常使用的语言，所以它与语言学的研究有着密切的联系，但又有重要的区别。自然语言处理并不是一般地研究自然语言，而在于研制能有效地实现自然语言通信的计算机系统，特别是其中的软件系统。"
c ="因而它是计算机科学的一部分。自然语言处理（NLP）是计算机科学，人工智能，语言学关注计算机和人类（自然）语言之间的相互作用的领域。"
allList=[' '.join(jieba.cut(s,cut_all=False)) for s in [a,b,c]]
print(allList)
~~~

分词后结果为：

~~~python
['自然语言  处理  是  计算机科学  领域  与  人工智能  领域  中  的  一个  重要  方向  。  它  研究  能  实现  人  与  计算机  之间  用  自然语言  进行  有效  通信  的  各种  理论  和  方法  。  自然语言  处理  是  一门  融  语言学  、  计算机科学  、  数学  于  一体  的  科学', '因此  ，  这一  领域  的  研究  将  涉及  自然语言  ，  即  人们  日常  使用  的  语言  ，  所以  它  与  语言学  的  研究  有着  密切  的  联系  ，  但  又  有  重要  的  区别  。  自然语言  处理  并  不是  一般  地  研究  自然语言  ，  而  在于  研制  能  有效  地  实现  自然语言  通信  的  计算机系统  ，  特别  是  其中  的  软件系统  。', '因而  它  是  计算机科学  的  一部分  。  自然语言  处理  （  NLP  ）  是  计算机科学  ，  人工智能  ，  语言学  关注  计算机  和  人类  （  自然  ）  语言  之间  的  相互作用  的  领域  。']
~~~

注意：分词输出的形式和 CountVectorize 所需要的数据形式要统一。



## 去停用词，转化为代表词频的特征向量（矩阵）

~~~python
from sklearn.feature_extraction.text import CountVectorizer
#从文件导入停用词表
stpWrdPath='./中文停用词库.txt'
with open(stpWrdPath,'rb') as fp:
    stopWord=fp.read().decode('utf-8')	#停用词提取
#将停用词表转换为list
stpWrdLst=stopWord.splitlines()
#对CountVectorizer进行初始化(去除中文停用词)
countVec=CountVectorizer(stop_words=stpWrdLst)	#创建词袋数据结构
xCountTrain=countVec.fit_transform(allList[:2])
#将原始训练和测试文本转化为特征向量
xCountTrain=xCountTrain.toarray()
xCountTest=countVec.transform(allList[2]).toarray()
print(xCountTrain)
#词汇表
print('\nvocabulary list:\n\n',countVec.get_feature_names())
print('\nvocabulary dic:\n\n',countVec.vocabulary_)
print('vocabulary:\n\n')
for key,value in countVec.vocabulary_.items():
    print(key,value)
~~~

训练集，也就是说a，b的词频统计结果，词汇列表、字典为：

~~~python
[[1 1 1 1 1 0 0 2 1 0 1 1 1 0 1 0 0 0 1 0 1 1 0 3 1 2 0 0 1 0 0 1 1 1 2]
 [0 0 0 0 0 1 1 1 1 1 0 0 0 1 1 1 1 1 0 1 3 0 1 4 0 0 1 1 1 1 1 0 1 1 1]]

vocabulary list:

 ['一个', '一体', '一门', '之间', '人工智能', '使用', '区别', '处理', '实现', '密切', '数学', '方向', '方法', '日常', '有效', '有着', '涉及', '特别', '理论', '研制', '研究', '科学', '联系', '自然语言', '计算机', '计算机科学', '计算机系统', '语言', '语言学', '软件系统', '这一', '进行', '通信', '重要', '领域']

vocabulary dic :

 {'区别': 6, '特别': 17, '一体': 1, '数学': 10, '方法': 12, '方向': 11, '计算机科学': 25, '研制': 19, '涉及': 16, '实现': 8, '日常': 13, '有着': 15, '语言学': 28, '这一': 30, '重要': 33, '人工智能': 4, '进行': 31, '理论': 18, '一门': 2, '自然语言': 23, '有效': 14, '通信': 32, '研究': 20, '联系': 22, '使用': 5, '科学': 21, '软件系统': 29, '计算机系统': 26, '领域': 34, '计算机': 24, '密切': 9, '之间': 3, '语言': 27, '一个': 0, '处理': 7}
vocabulary:


区别 6
特别 17
一体 1
数学 10
方法 12
方向 11
计算机科学 25
研制 19
涉及 16
实现 8
日常 13.....
~~~

停用词的配置：可默认配置 countVec=CountVectorizer(stop_words=None)，表示不去掉停用词；如果是英文的话，停用词不需要构建，直接 countVec=CountVectorizer(stop_words=

‘english’)，则去掉英文停用词。

countVec.fit_transform(data) 的结果是如下的格式：

~~~python
print(countVec.fit_transform(X_test))
 （data列表中第一个元素，词典里第几个词） 词频
  (0, 7)    2
  (0, 25)   2
  (0, 34)   2
  (0, 4)    1
  (0, 0)    1
  。。。。。
print(countVec.fit_transform(X_test).toarray())
[[1 1 1 1 1 0 0 2 1 0 1 1 1 0 1 0 0 0 1 0 1 1 0 3 1 2 0 0 1 0 0 1 1 1 2]
 [0 0 0 0 0 1 1 1 1 1 0 0 0 1 1 1 1 1 0 1 3 0 1 4 0 0 1 1 1 1 1 0 1 1 1]]
~~~

.toarray() 是将结果转化为稀疏矩阵的表示方法。

每个词在所有文档中的词频：

~~~python
print(cv_fit.toarray())

['bird', 'cat', 'dog', 'fish']
[[0 1 1 1]
 [0 2 1 0]
[1 0 0 1]
 [1 0 0 0]]
print(cv_fit.toarray().sum(axis=0))
[2 3 2 2]
~~~



## 训练集和测试集分割

数据量大的时候 cross_validation 分割代码：

~~~ python
from sklearn.cross_validation import train_test_split
#对数据进行分割，25%的文本用作测试集，75%作为训练集
xTrain,xTest,yTrain,yTest=train_test_split(x,y,test_size=0.25,random_state=33)
~~~



## 关于 token_pattern 参数

按指定字符切分字符串。

默认情况下，由于英文专有名词如 People’s Republic of China(中华人民共和国)默认是按空格分开，有时并不符合需求，需要将其作为一个整体来处理。可通过一个正则表达式来按指定字符匹配关键词，分开字符串。

~~~python
from sklearn.feature_extraction.text import CountVecorizer
nn=["People's Republic of China@中华人民共和国"]
vectorizer=CountVectorizer(token_pattern=r"(?u)\b[^@]+\b")	#每篇文章关键词按@分开
wordFrequencyMatrix=vectorizer.fit_transform(nn)
for f in vectorizer.get_feature_names():
    print f
~~~

结果为：

~~~python
People's Republic of China
中华人民共和国
~~~





# TfidfVectorizer

TF-IDF（term frequency-inverse document frequency）是一种用于信息检索和数据挖掘的常用加权技术。TF 意思为词频，IDF 意思为逆文本频率指数。

Tfidf 考虑到一些非常大众化的词，如“我们”等，几乎所有文章都会出现。这些词对文本特点刻画的贡献就比较小了，这就是逆文档频率的思想。这种方法，文档数目越多，优势更明显。

![image-20180824151521350](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180824151521350.png)

![image-20180824151535562](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180824151535562.png)



## 去停用词，转化为逆文档词频的特征向量（矩阵）

~~~python
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf=TfidfVectorizer(stop_words=stpWrdLst)
weight=tfidf.fit_transform(allList).toarray()
word=tfidf.get_feature_names()
print('TFIDF词频矩阵：\n')
print(weight)
for i in range(len(weight)):
    print("-------这里输出第",i,"个文本的词语tf-idf权重------")
    for j in range(len(word)):
        print(word[j],weight[i][j])	#第i个文本中，第j个词的tfidf值
~~~

结果：

~~~python
IFIDF词频矩阵:

[[0.         0.21214414 0.21214414 0.         0.21214414 0.16134109
  0.16134109 0.         0.         0.         0.         0.25059149
  0.16134109 0.         0.21214414 0.21214414 0.21214414 0.
  0.16134109 0.         0.         0.         0.21214414 0.
  0.         0.16134109 0.21214414 0.         0.         0.37588724
  0.16134109 0.32268217 0.         0.         0.12529575 0.
  0.         0.21214414 0.16134109 0.16134109 0.25059149]
 [0.         0.         0.         0.         0.         0.
  0.         0.         0.19343658 0.         0.19343658 0.11424676
  0.1471135  0.19343658 0.         0.         0.         0.19343658
  0.1471135  0.19343658 0.19343658 0.19343658 0.         0.
  0.19343658 0.44134051 0.         0.19343658 0.         0.45698704
  0.         0.         0.19343658 0.1471135  0.11424676 0.19343658
  0.19343658 0.         0.1471135  0.1471135  0.11424676]
 [0.28840482 0.         0.         0.28840482 0.         0.2193393
  0.2193393  0.28840482 0.         0.28840482 0.         0.17033653
  0.         0.         0.         0.         0.         0.
  0.         0.         0.         0.         0.         0.28840482
  0.         0.         0.         0.         0.28840482 0.17033653
  0.2193393  0.4386786  0.         0.2193393  0.17033653 0.
  0.         0.         0.         0.         0.17033653]]
-------这里输出第 0 个文本的词语tf-idf权重------
nlp 0.0
一个 0.2121441385465931
一体 0.2121441385465931
一部分 0.0
一门 0.2121441385465931
之间 0.16134108547600245
人工智能 0.16134108547600245
人类 0.0
使用 0.0
关注 0.0
区别 0.0
处理 0.2505914913745825
实现 0.16134108547600245
密切 0.0
数学 0.2121441385465931
方向 0.2121441385465931
方法 0.2121441385465931
日常 0.0
有效 0.16134108547600245
有着 0.0
涉及 0.0
特别 0.0
理论 0.2121441385465931
相互作用 0.0
研制 0.0
研究 0.16134108547600245
科学 0.2121441385465931
联系 0.0
自然 0.0
自然语言 0.3758872370618737
计算机 0.16134108547600245
计算机科学 0.3226821709520049
计算机系统 0.0
语言 0.0
语言学 0.12529574568729124
软件系统 0.0
这一 0.0
进行 0.2121441385465931
通信 0.16134108547600245
重要 0.16134108547600245
领域 0.2505914913745825
-------这里输出第 1 个文本的词语tf-idf权重------
nlp 0.0
一个 0.0
一体 0.0
一部分 0.0
一门 0.0
之间 0.0
人工智能 0.0
人类 0.0
使用 0.19343657854281884
关注 0.0
区别 0.19343657854281884
处理 0.11424675938617863
实现 0.14711350389729444
密切 0.19343657854281884
数学 0.0
方向 0.0
方法 0.0
日常 0.19343657854281884
有效 0.14711350389729444
有着 0.19343657854281884
涉及 0.19343657854281884
特别 0.19343657854281884
理论 0.0
相互作用 0.0
研制 0.19343657854281884
研究 0.4413405116918833
科学 0.0
联系 0.19343657854281884
自然 0.0
自然语言 0.45698703754471454
计算机 0.0
计算机科学 0.0
计算机系统 0.19343657854281884
语言 0.14711350389729444
语言学 0.11424675938617863
软件系统 0.19343657854281884
这一 0.19343657854281884
进行 0.0
通信 0.14711350389729444
重要 0.14711350389729444
领域 0.11424675938617863
-------这里输出第 2 个文本的词语tf-idf权重------
nlp 0.28840481933429657
一个 0.0
一体 0.0
一部分 0.28840481933429657
一门 0.0
之间 0.2193392988686609
人工智能 0.2193392988686609
人类 0.28840481933429657
使用 0.0
关注 0.28840481933429657
区别 0.0
处理 0.17033653225522746
实现 0.0
密切 0.0
数学 0.0
方向 0.0
方法 0.0
日常 0.0
有效 0.0
有着 0.0
涉及 0.0
特别 0.0
理论 0.0
相互作用 0.28840481933429657
研制 0.0
研究 0.0
科学 0.0
联系 0.0
自然 0.28840481933429657
自然语言 0.17033653225522746
计算机 0.2193392988686609
计算机科学 0.4386785977373218
计算机系统 0.0
语言 0.2193392988686609
语言学 0.17033653225522746
软件系统 0.0
这一 0.0
进行 0.0
通信 0.0
重要 0.0
领域 0.17033653225522746
~~~





# GridSearchCV

自动调参，给出最优化的结果和参数，适合于小数据集。

用于系统地遍历多种参数组合，通过交叉验证确定最佳效果参数。



## 参数

**estimator**：分类器，并且需要传入需要确定的最佳参数之外的其他参数。每一个分类器都需要一个 scoring 参数，或 score 方法。

**param_grid**：字典或列表，即需要最优化的参数的取值。

**scoring**：准确度评价标准，默认 None。字符串（函数名），或是可调用对象；如果是 None，则使用 estimator 的误差估计函数。

**cv**：交叉验证参数，默认 None，使用三折交叉验证。指定 fold 数量，默认为 3。

**refit**：默认为 True，程序将会以交叉验证训练集得到的最佳参数，重新对所有可用的训练集与开发集进行，作为最终用于性能评估的最佳模型参数。即在搜索参数结束后，用最佳参数结果再次 fit 一遍全部数据集。

**iid**：默认 True，各个样本 fold 概率分布一致，误差估计为所有样本之和，而非各个 fold 的平均。

**verbose**：日志冗长度。0，不输出训练过程，1：偶尔输出，>1，对每个子模型都输出。

**n_jobs**：并行数，-1：跟 CPU 核数一致，1：默认值，int，个数。

**pre_dispatch**：指定总共分发的并行任务数。当 n_jobs 大于 1 时，数据将在每个运行点进行复制，这可能导致 OOM。而设置 pre_dispatch 参数，则可以预先划分总共的 job 数量，使得数据最多被复制pre_dispatch次。



## 方法

grid.fit()：运行网格搜索。

grid_scores_：给出不同参数情况下的评价结果

best_params_：描述已取得最佳结果的参数的组合

best_score_：提供优化过程期间观察到的最好的评分



## 属性

best_estimator_：Estimator that was chosen by the search,i.e. estimator which gave highest score(or smallest loss if specified) . Not available if refit=False.





# SGD

随机梯度下降（SGD)是一种简单但又非常高效的方法，主要用于凸损失函数下线性分类器的判别式学习，例如支持向量机和 Logistic 回归。

SGD 的优势：

高效；易于实现

劣势：

需要一些超参数，例如 regularization（正则化）参数和 number of iterations（迭代次数）；对 feature scaling（特征缩放）敏感。



## 随机梯度下降分类

loss function（损失函数）可以通过 loss 参数来设置。

SGDClassifier 支持以下的 loss function：

- loss=“hinge”：linear Support Vector Machine（线性支持向量机）
- loss=“modified——huber”：smooth hinge loss（平滑的 hinge 损失）
- loss=“log”：logistic regression（logistic 回归）

惩罚方法可以通过 penalty 参数来设定。SGD 支持以下 penalties（正则）：

- penalty=”l2”: L2 norm penalty on coef_
- penalty=”l1”: L1 norm penalty on coef_
- penalty=”elasticnet”: Convex combination of L2 and L1（L2 型和 L1 型的凸组合）; (1 - l1_ratio) * L2 + l1_ratio * L1

默认设置为 penalty=“l2”。L1 penalty 导致稀疏解，使得大多数系数为零。Elastic Net（弹性网）解决了在特征高相关时 L1 penalty（惩罚）的一些不足。参数 l1_ratio 控制了 L1 和 L2 penalty（惩罚）的 convex combination （凸组合）。

~~~python
from sklearn.linear_model import SGDClassifier
from sklearn.datasets.samples_generator import make_blobs

x,y=make_blobs(n_samples=50,centers=2,random_state=0,cluster_std=0.60)

clf=SGDClassifier(loss="hinge",alpha=0.01,max_iter=200,fit_intercept=True)
clf.fit(x,y)	#训练模型

print('回归系数：',clf.coef_)
print('偏差：',clf.intercept_)

~~~





## 随机梯度下降法进行多分类

通过利用“one versus all”（OVA）方法来组合多个二分类器，从而实现多分类。对于每一个 K 类，可以训练一个二分类器来区分自身和其他 K-1 个类。

在 multi-class classification （多类分类）的情况下， coef_ 是 shape=[n_classes, n_features] 的一个二维数组， intercept_ 是 shape=[n_classes] 的一个一维数组。 coef_ 的第 i 行保存了第 i 类的 OVA 分类器的权重向量；类以升序索引。 注意，原则上，由于允许创建一个概率模型，所以 loss=”log” 和 loss=”modified_huber” 更适合于 one-vs-all 分类。 

~~~python
#随机梯度下降进行多分类
from sklearn.linear_model import SGDClassifier
from sklearn.metrics import accuracy_score
from sklearn import datasets
iris=datasets.load_iris()
x,y=iris.data,iris.target
clf=SGDClassifier(alpha=0.001,max_iter=100).fit(x,y)
yPred=clf.predict(x)
print('三分类花卉数据准确率：',accuracy_score(y,yPred))
print('包含的二分类器索引：',clf.classes_)	#one versus all 方法来组合多个二分类器
print('回归系数：',clf.coef_)	#每个二分类器的回归系数
print('偏差：',clf.intercept_)	#每个二分类器的偏差
~~~





# 朴素贝叶斯

## 朴素贝叶斯算法说明

朴素贝叶斯算法是建立在每个特征值间独立的基础上的监督学习分类算法，这也是它称为“朴素”贝叶斯的缘由。在现实环境中，很难达到两个特征值间绝对的相互独立。在给定一个类变量 y 和依赖的特征向量 x ，贝叶斯定理如下：

 ![image-20180825105841681](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825105841681.png)

假设两个特征值间相互独立，关系被简化为：

![image-20180825110057689](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825110057689.png)

由于分母是恒定的输入，可以使用以下的分类规则：

![image-20180825110205100](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825110205100.png)

可以使用最大后验概率(MAP)，来估计 p(y) 和 p(x|y)。不同朴素贝叶斯分类算法是因为它们对 p(x|y) 做出了不同的假设。

尽管朴素贝叶斯的假设过于简单，但在已有的应用中，如文档分类和垃圾邮件分类，它都表现出了相当好的效果。

和其他更先进的方法相比，朴素贝叶斯算法学习和分类的过程效率更高。每个类的条件特征的独立分布意味着每个类分布可以独立地估计为一维分布，这反过来有助于缓解数据降维所带来的麻烦。



## 高斯朴素贝叶斯

GaussianNB 继承高斯朴素贝叶斯，特征可能性被假设为：

![image-20180825110807931](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825110807931.png)

~~~python
import numpy as np
from sklearn.naive_bayes import GaussianNB
x=np.array([[-1,-1][-2,-1],[-3,-2],[1,1],[2,1,],[3,2]])
y=np.array([1,1,1,2,2,2])
clf.GaussianNB.fit(x,y)
print clf.predict([[-0.8,-1]])

#partial_fit说明：增量地训练一批样本
#这种方法连续几次在不同的数据集，从而实现核心和在线学习。当数据集很大的时候，不适合在内存中运算
clfPf=GaussianNB().partial_fit(x,y,np.unique(y))
print clfPf.predict([[-0.8,-1]])
~~~



## 多项式分布

MultinomialNB 实现 multinomially 分布数据的贝叶斯算法，是一个经典的朴素贝叶斯文本分类使用的变种。

对每一个 y 来说，分布通过向量：

![image-20180825111652498](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825111652498.png)

![image-20180825111731781](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825111731781.png)

![image-20180825111808537](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825111808537.png)

![image-20180825111853102](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825111853102.png)

平滑先验 α>=0 防止学习样本中不存在的特征在计算中概率为 0，设置 α=1，称为拉普拉斯平滑，α<1，称为 Lidstone 平滑。

~~~python
import numpy as np
from sklearn.naive_bayes import MultinomialNB
x=np.random.randint(5,size=(6,100))
y=np.array([1,2,3,4,5,6])
clf=MultinomialNB().fit(x,y)
print clf.predict(x[2:3])
~~~



## 伯努利朴素贝叶斯

BernoulliNB 实现多元伯努利分布数据的贝叶斯算法。例如，可能会有多个特征，但每个被假定为一个二进制值（伯努利，布尔）变量。因此，这类要求的样本被表示为二进制值的特征向量。

![image-20180825112738711](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180825112738711.png)

在文本分类的情况下，词的出现向量（而不是字计数向量）可以用来训练和分类。

~~~python
import numpy as np
from sklearn.naive_bayes import BernoulliNB
x=np.random.randint(2,size=(6,100))
y=np.array([1,2,3,4,4,5])
clf=BernoulliNB()
clf.fit(x,y)
BernoulliNB(alpha=0.1,binarize=0.0,class_prior=None,fit_prior=True)
print(clf.predict(x[2:3]))
~~~





# yield

## 可迭代对象

~~~python
#mylist是一个可迭代的对象。当使用一个列表生成式来建立列表的时候，就建立了一个可迭代的对象
mylist=[x*x for x in range(3)]
for i in mylist:
    print(i)
~~~

输出为：

~~~python
0
1
4
~~~

在这里，所有的值都存在内存中，所以并不适合大量数据



## 生成器

- 可迭代
- 只能读取一次
- 实时生成数据，不全存在内存中

~~~python
mygenerator=(x*x for x in range(3))
for i in mygenerator:
    print(i)
~~~

注意：之后不能再使用 for i in mygenerator 了。



## yield 关键字

- yield 是一个类似于 return 的关键字，只是这个函数返回的是个生成器
- 当调用这个函数的时候，函数内部的代码并不立马执行，只是返回一个生成器对象
- 当使用 for 进行迭代的时候，函数中的代码才会执行

~~~python
def createGenerator():
    mylist=range(3)
    for i in mylist:
        yield i*i
        
mygenerator=createGenerator()
print(mygenerator)
for i in mygenerator:
    print(i)
~~~

结果为：

~~~python
<generator object createGenerator at 0xb7555c34>
0
1
4
~~~

第一次迭代中函数会执行，从开始到达 yield 关键字，然后返回 yield 后的值作为第一次迭代的返回值。然后，每次执行这个函数都会继续执行在函数内部定义的循环的下一次，再返回那个值，直到没有可以返回的。





# Keras

Keras 是一个开源的 python 深度学习库，可以基于 theano 或者 TensorFlow。



## 优化器（optimizers）

优化器是调整每个节点权重的方法。

~~~python
model=Sequential()
model.add(Dense(64,init='uniform',input_dim=10))
model.add(Activation('tanh'))
model.add(Activation('softmax'))
sgd=SGD(lr=0.01,decay=1e-6,momentum=0.9,nesterov=True)
model.compile(loss='mean_squared_error',optimizer=sgd)
~~~

可以看到优化器在模型编译前定义，作为编译时的两个参数之一。

代码中的 sgd 是随机梯度下降算法，lr 表示学习速率，momentum 表示动量项。decay 是学习速率的衰减系数（每个 epoch 衰减一次），Nesterov 的值是 False 或者 True，表示使不使用 Nesterov momentum。

除了 sgd，还可以选择的优化器有 RMSprop（适合递归神经网络）、Adagrad、Adadelta、Adam、Adamax、Nadam



## 目标函数（objectives）

目标函数又称损失函数（loss），目的是计算神经网络的输出与样本标记的差的一种方法，代码示例：

~~~python
model=Sequential()
model.add(Dense(64,init='uniform',input_dim=10))
model.add(Activation('tanh'))
model.add(Activation('softmax'))
sgd=SGD(lr=0.01,decay=1e-6,momentum=0.9,nesterov=True)
model.compile(loss='mean_squared_error',optimizer=sgd)
~~~

mean_squared_error 是损失函数的名称。

可以选择的损失函数有：mean_squared_error，mean_absolute_error，squared_hinge，hinge，binary_crossentropy，categorical_crossentropy

这里 binary_crossentropy 和 categorical_crossentropy 也就是 logloss



## 激活函数（activations）

每一个神经网络层都需要一个激活函数，代码示例：

~~~python
from keras.layers.core import Activation,Dense

model.add(Dense(64))
model.add(Activation('tanh'))

#或把上面两行合并为：
model.add(Dense(64,activation='tanh'))
~~~

可以选择的激活函数有：linear，sigmoid，hard_sigmoid，tanh，softplus，relu，softplus，softmax，softsign，还有一些高级激活函数，比如 PReLU，LeakyReLU 等。



## 参数初始化（Initializations）

在添加 layer 时调用 init 进行这一层的权重初始化，有两种初始化方法。

###  指定初始化方法名称

示例代码：

~~~python
model.add(Dense(64,init='uniform'))
~~~

可以选择的初始化方法有：uniform，lecun_uniform，normal，orthogonal，zero，glorot_normal，he_normal 等。

### 调用对象

该对象必须包含两个参数：shape（待初始化的变量的 shape）和 name（该变量的名字），该可调用对象必须返回一个（Keras）变量，例如 K.variable() 返回的就是这种变量，示例代码：

~~~python
from keras import backend as K
import numpy as np

def myInit(shape,name=None):
    value=np.random.random(shape)
    return K.variable(value,name=name)

model.add(Dense(64,init=myInit))

#或者
from keras import initializations

def myInit(shape,name=None):
    return initializations.normal(shape,scale=0.01,name=name)

model.add(Dense(64,init=myInit))
~~~

可以通过库中的方法设定每一层的初始化权重，也可以自己初始化权重。自己设定的话，可以精确到每个节点的权重。



## 层（layer）

keras 的层主要包括：常用层（Core），卷积层（Convolutional），池化层（Pooling），局部连接层，递归层（Recurrent），嵌入层（Embedding）高级激活层，规范层，噪声层，包装层。当然，也可以编写自己的层。

### 对于层的操作

~~~python
layer.get_weights()	#返回该层的权重
layer.set_weights(weights)	#将权重加载到该层
config=layer.get_config()	#保存该层的配置
layer=layer_from_config(config)	#加载一个配置到该层
#该层有一个节点时，获得输入张量、输出张量，以及各自的形状：
layer.input
layer.output
layer.input_shape
layer.output_shape
#该层有多个节点时(node_index 为节点序号)
layer.get_input_at(node_index)
layer.get_output_at(node_index)
layer.get_input_shape_at(node_index)
layer.get_output_shape_at(node_index)
~~~

### Dense 层（全连接层）

~~~python
keras.layers.core.Dense(output_dim,init='glorot_uniform',activation='linear',weights=None,W_regularizer=None,b_regularizer=None,activity_regularizer=None,W_constraint=None,bias=True,input_dim=None)
~~~

output_dim：输出数据的维度

init：初始化该层权重的方法

activation：该层的激活函数

weights：numpy array 的 list。该 list 应含有一个形如（input_dim,output_dim）的权重矩阵和一个形如（output_dim,) 的偏置向量

regularizer：正则项，w 为权重的，b 为偏置的，activity 为输出的

constraints：约束项

bias：是否包含偏置向量，是布尔值

input_dim：输入数据的维度

### dropout 层

~~~python
keras.layers.core.Dropout(p)
~~~

为输入数据施加 Dropout。Dropout 将在训练过程中的每次参数更新时，随机断开一定百分比（p）的输入神经元连接，用于防止过拟合。

### 递归层（Recurrent）

递归层包含三种模型：LSTM，GRU 和 simpleRNN

#### 抽象层（不可直接使用）

~~~python
keras.layers.recurrent.Recurrent(weights=None,return_sequences=False,go_backwards=False,stateful=False,unroll=False,consume_less='cpu',input_dim=None,input_length=None)
~~~

return_sequences：True 返回整个序列，False 返回输出序列的最后一个输出

go_backwards：True 逆向处理输入序列，默认为 False

stateful：布尔值，默认为 False。若为 True，则一个 batch 中下标为 i 的样本的最终状态会用作下一个 batch 同样下标的样本的初始状态

#### 全连接 RNN 网络

~~~python
keras.layers.recurrent.SimpleRNN(output_dim,init='glorot_uniform',inner_init='orthogonal',activation='tanh',W_regularizer=None,U_regularizer=None,b_regularizer=None,dropout_W=0.0,dropout_U=0.0)
~~~

inner_init：内部单元的初始化方法

dropout_W：0~1 之间的浮点数，控制输入单元到输入门的连接断开比例

dropout_U：0~1 之间的浮点数，控制输入单元到递归连接的断开比例

#### LSTM 层

~~~~python
keras.layers.recurrent.LSTM(output_dim,init='glorot_uniform',inner_init='orthogonal',forget_bias_init='one',activation='tanh',inner_activation='hard_sigmoid',W_regularizer=None,b_regularizer=None,dropout_W=0.0,dropout_U=0.0)
~~~~

forget_bias_init：遗忘门偏置的初始化函数

inner_activation：内部单元激活函数

### Embedding 层

model 层是最主要的模块，可以将上面定义的各种基本组件组合起来。

**model 的方法**：

model.summary()：打印出模型概况

model.get_config()：返回包含模型配置信息的 python 字典

model.get_weights()：返回模型权重张量的列表，类型为 numpy array

model.set_weights()：从 numpy array 里将权重载入给模型

model.to_json：返回代表模型的 JSON 字符串，仅包含网络结构，不包含权值。可以从 JSON 字符串中重构原模型：

~~~python
from models import model_from_json

json_string=model.to_json()
model=model_from_json(json_string)
~~~

model.to_yaml：与 model.to_json 类似，同样可以从产生的 YAML 字符串中重构模型：

~~~python
from model import model_from_yaml

yaml_string=model.to_yaml()
model=model_from_yaml(yaml_string)
~~~

model.save_weights(filepath)：将模型权重保存到指定路径，文件类型是 HDF5（后缀是 .h5）

model.load_weights(filepath,by_name=False)：从 HDF5 文件中加载权重到当前模型中，默认情况下模型的结构将保持不变。如果想将权重载入不同的模型（有些层相同）中，则设置 by_name=True，只有名字匹配的层才会载入权重。

keras 有两种 model，分别是 Sequential 模型和泛型模型。

#### Sequential 模型

Sequential 是多个网络层的线性堆叠。

可以通过向 Sequential 模型传递一个 layer 的 list 来构造该模型：

~~~python
from keras.models import Sequential
from keras.layers import Dense,Activation

model=Sequential([Dense(32,input_dim=784),Activation('relu'),Dense(10),Activation('softmax')])
~~~

也可以通过 .add() 方法一个个的将 layer 加入模型：

~~~python
model=Sequential()
model.add(Dense(32,input_dim=784))
model.add(Activation('relu'))
~~~

还可以通过 merge 将两个 Sequential 模型通过某种方式合并。

Sequential 模型的方法：

~~~python
compile(self,optimizer,loss,metrics=[],sample_weight_mode=None)

fit(self,x,y,batch_size=32,nb_epoch=10,verbose=1,callbacks=[],validation_split=0.0,validation_data=None,shuffle=True,class_weight=None,sample_weight=None)

evaluate(self,x,y,batch_size=32,verbose=1,sample_weight=None)

#按batch获得输入数据对应的输出，函数的返回值是预测值的numpy array
predict(self,x,batch_size=32,verbose=0)

#按batch产生输入数据的类别预测结果，函数的返回值是类别预测结果的numpy array或numpy
predict_classes(self,x,batch_size=32,verbose=1)

#按batch产生输入数据属于各个类别的概率，函数的返回值是类别概率的numpy array
predict_proba(self,x,batch_size=32,verbose=1)

train_on_batch(self,x,y,class_weight=None,sample_weight=None)

test_on_batch(self,x,y,sample_weight=None)

predict_on_batch(self,x)

fit_generator(self,generator,samples_per_epoch,nb_epoch,verbose=1,callbacks=[],validation_data=None,nb_val_samples=None,class_weight=None,
              max_q_size=10)

evaluate_generator(self,generator,val_samples,max_q_size=10)
~~~

#### 泛型模型

keras 泛型模型：用户定义多输出模型、非循环有向模型或具有共享层的模型等复杂模型

适用于实现：全连接网络和多输入多输出模型

**泛型模型 model 属性**：

model.layers：组成模型图的各个层

model.inputs：模型的输入张量列表

model.outputs：模型的输出张量列表

方法：类似序列模型的方法

补充：

~~~python
get_layer(self,name=None,index=None)
~~~

本函数模型中层的下标或名字获得层对象。泛型模型中层的下标依据自底向上，水平遍历的顺序。

name：字符串，层的名字

index：整数，层的下标

函数的返回值是层对象。



## backend

什么是“后端”？

Keras 是个模型级的库，提供了快速构建深度学习网络的模块。Keras 并不处理如张量乘法、卷积等底层操作。这些操作依赖于特定的、优化良好的张量操作库。Keras 依赖于处理张量的库就称为“后端引擎”。Keras 提供了三种后端引擎 Theano/TensorFlow/CNTK，并将其函数同一封装，使得用户可以以同一个接口调用不同后端引擎的函数

- Theano 是一个开源的符号主义张量操作框架，由蒙特利尔大学 LISA/MILA 实验室开发
- TensorFlow 是一个符号主义的张量操作框架，由 Google 开发
- CNTK 是一个由微软开发的商业级工作包

### keras.json 细节

~~~python
{
    "image_data_format":"channels_last",
    "epsilon":1e-07,
    "floatx":"float32",
    "backend":"tensorflow"
}
~~~

- image_data_format：字符串，“channels_last” 或 “channels_first”，该选项指定了 Keras 将要使用的维度顺序。可通过 keras.backend.image_data_format() 来获取当前的维度顺序。对  2D 数据来说，“channels_last” 假定维度顺序为（rows,cols,channels)，而 “channels_first” 假定维度顺序为（channels,rows,cols)。对 3D 数据言，”channels_last” 假定（conv_dim1,conv_dim2,conv_dim3,channels)，“channels_first” 则是 (channels,

  conv_dim1,conv_dim2,conv_dim3)

- epsilon：浮点数，防止除 0 错误的小数字

- floatx：字符串，”float16”，“float32”，”float64“ 之一，为浮点数精度

- backend：字符串，所使用的后端，为 ”tensorflow“ 或 ”theano“

### 使用抽象的 Keras 后端来编写代码

如果希望编写的 Keras 模块能够同时在 Theano 和 TensorFlow 两个后端上使用，可以通过 Keras 后端接口来编写代码：

~~~python
from keras import backend as K
~~~

 下面的代码实例化了一个输入占位符，等价于 tf.placeholder()，T.matrix()，T.tensor3() 等：

~~~python
input=K.placeholder(shape=(2,4,5))
#also works:
input=K.placeholder(shape=(None,4,5))
#also works:
input=K.placeholder(ndim=3)
~~~

下面的代码实例化了一个共享变量（shared），等价于 tf.variable() 或 theano.shared()：

~~~python
val=np.random.random((3,4,5))
var=K.variable(value=val)

#all-zeros variable:
var=K.zeros(shape=(3,4,5))
#all-ones:
var=K.ones(shape=(3,4,5))
~~~

### Keras 后端函数

~~~python
backend()	#返回当前后端

epsilon()	#以数值形式返回一个(一般来说很小的)数，用以防止除0错误

set_epsilon(e)	#设置在数值表达式中使用的fuzz factor，用于防止除0错误，该值应该是一个较小的浮点数

floatx()	#返回默认的浮点数数据类型，为字符串，如'float16','float32','float64'

cast_to_floatx(x)	#将numpy array转换为默认的keras floatx类型，x为numpy array，返回值也为numpy array但其数据类型变为floatx。
~~~

~~~python
from keras import backend as K

K.floatx()
#'float32'
arr=numpy.array([1.0,2.0],dtype='float64')
arr.dtype
#dtype('float64')
new_arr=K.cast_to_floatx(arr)
new_arr
#array([1.,2.],dtype='float32')

image_data_format()	#返回默认的图像的维度顺序('channels_last'或'channels_first')

set_image_data_format(data_format)	#设置图像的维度顺序

is_keras_tensor(x)	#判断x是否是一个keras tensor，返回布尔值
~~~

~~~python
from keras import backend as K

np_var=numpy.array([1,2])
K.is_keras_tensor(np_var)
#False
keras_var=K.variable(np_var)
L.is_keras_tensor(keras_var)	#A variable is not a Tensor.
#False
keras_placeholder=K.placeholder(shape=(2,4,5))
K.is_keras_tensor(keras_placeholder)	#A placeholder is a Tensor.
#True
~~~

~~~python
variable(value,dtype='float32',name=None)	#实例化一个张量，并返回
~~~

- value：用来初始化张量的值
- dtype：张量数据类型
- 张量的名字（可选）

示例:

~~~python
from keras import backend as K
val=np.array([[1,2],[3,4]])
kvar=K.variable(value=val,dtype='float64',name='example_var')
K.dtype(kvar)
#'float64'
print(kvar)
#example_var
kvar.eval()
#array([[1.,2.],[3.,4.]])
~~~

~~~python
placeholder(shape=None,ndim=None,dtype='float32',name=None)	#实例化一个占位符，并返回
~~~

- shape：占位的 shape（整数 tuple，可能包含 None）
- ndim：占位符张量的阶数，要初始化一个占位符，至少指定 shape 和 ndim 之一，如果都指定则使用 shape
- dtype：占位符数据类型
- name：占位符名称（可选）

~~~python
shape(x)	#返回一个张量的符号shape，符号shape的意思是返回值本身也是一个tensor

from keras import backend as K
tf_session=K.get_session()
val=np.array([[1,2],[3,4]])
kvar=K.variable(value=val)
input=K.placeholder(shape=(2,4,5))
K.shape(kvar)
#<tf.Tensor 'Shape_8:0' shape=(2,) dtype=int32>
K.shape(input)
#<tf.Tensor 'Shape_9:0' shape=(3,) dtype=int32>

K.shape(kvar).eval(session=tf_session)
#array([2, 2], dtype=int32)
K.shape(input).eval(session=tf_session)
#array([2, 4, 5], dtype=int32)
~~~

~~~python
int_shape(x)	#以整数Tuple或None的形式返回张量shape

from keras import backend as K
input = K.placeholder(shape=(2, 4, 5))
K.int_shape(input)
#(2, 4, 5)
val = np.array([[1, 2], [3, 4]])
kvar = K.variable(value=val)
K.int_shape(kvar)
#(2, 2)

ndim(x)	#返回张量的阶数，为整数
K.ndim(input)
#3
K.ndim(kvar)
#2

dtype(x)	#返回张量的数据类型，为字符串

 K.dtype(K.placeholder(shape=(2,4,5)))
#'float32'
K.dtype(K.placeholder(shape=(2,4,5), dtype='float32'))
#'float32'
K.dtype(K.placeholder(shape=(2,4,5), dtype='float64'))
#'float64'
 
kvar = K.variable(np.array([[1, 2], [3, 4]]))
K.dtype(kvar)
#'float32_ref'
kvar = K.variable(np.array([[1, 2], [3, 4]]), dtype='float32')
K.dtype(kvar)
#'float32_ref'

eval(x)	#求得张量的值，返回一个numpy array
K.eval(kvar)
#array([[ 1.,  2.],[ 3.,  4.]], dtype=float32)

zeros(shape,dtype='float32',name=None)	#生成全0张量

kvar = K.zeros((3,4))
K.eval(kvar)
#array([[ 0.,  0.,  0.,  0.],[ 0.,  0.,  0.,  0.],[ 0.,  0.,  0.,  0.]], dtype=float32)

ones(shape,dtype='float32',name=None)	#生成全1张量

kvar = K.ones((3,4))
K.eval(kvar)
#array([[ 1.,  1.,  1.,  1.],[ 1.,  1.,  1.,  1.],[ 1.,  1.,  1.,  1.]], dtype=float32)

eye(size,dtype='float32',name=None)	#生成单位矩阵

kvar = K.eye(3)
K.eval(kvar)
#array([[ 1.,  0.,  0.],[ 0.,  1.,  0.],[ 0.,  0.,  1.]], dtype=float32

zeros_like(x,name=None)	#生成与另一个张量x的shape相同的全0张量

kvar = K.variable(np.random.random((2,3)))
kvar_zeros = K.zeros_like(kvar)
K.eval(kvar_zeros)
#array([[ 0.,  0.,  0.],[ 0.,  0.,  0.]], dtype=float32)

ones_like(x,name=None)	#生成与另一个张量shape相同的全1张量


~~~





# Docopt

docopt 是一个用来解析命令行参数的工具，当需要在 Python 程序后附加参数时，就不需要再为此而发愁了。docopt 最大的特点在于不用考虑如何解析命令行参数，而是把需要的格式按一定规则写出来，解析也就完成了。

在 Python 中有一个属性 `__doc__` ，它的值是字符串，一般表示帮助信息。而 docopt 正是利用了这一属性，把帮助信息替换成命令行参数解析说明，再对它进行解析。

~~~pyt&#39;h&#39;p&#39;n
"""Naval Fate.
Usage:
 naval_fate.py ship new <name>...
 naval_fate.py ship <name> move <x> <y> [--speed=<kn>]
 naval_fate.py ship shoot <x> <y>
 naval_fate.py mine (set|remove) <x> <y> [--moored | --drifting]
 naval_fate.py (-h | --help)
 naval_fate.py --version
Options:
 -h --help  Show this screen.
 --version  Show version.
 --speed=<kn> Speed in knots [default: 10].
 --moored  Moored (anchored) mine.
 --drifting Drifting mine.
"""
from docopt import docopt
if __name__ == '__main__':
 arguments = docopt(__doc__, version='Naval Fate 2.0')
 print(arguments)
~~~

上述代码段中，很大一段帮助信息是命令行参数解析说明。在函数入口处调用docopt函数进行解析，返回的arguments变量是字典型变量，记录了选项是否被选用，参数的值是什么等信息。当程序从命令行运行时，根据arguments变量的记录来得知用户输入的选项和参数信息。

命令行解析信息包含两部分，分别是使用模式格式和选项描述格式。

## 使用模式格式

以usage:开始，以空行结束，如上代码段所示。它主要描述了用户添加命令行参数时的格式，也就是使用时的格式，解析也是按照此格式来进行。

每一个使用模式都包含如下元素：

- 参数

参数使用大写字母或尖括号<>围起来。

- 选项

选项以短横线开始（-或者—）。只有一个字母时-o，多于一个字母时--output。同时，可以把多个单字母选项合并，-ovi等同于-o、-v、-i。选项也可以有参数，此时别忘了给选项添加描述说明。

接下来是使用模式中用到的一些标识的含义，正确地使用能够更好地完成解析任务。

- []

代表可选的元素，方括号内的元素可有可无。

- ()

代表必须要有的元素，括号内的元素必须要有，哪怕是多个里选一个。

- |

互斥的元素，竖线两旁的元素只能有一个留下。

- ...

代表元素可以重复出现，最后解析的结果是一个列表。

- [options]

指定特定的选项，完成特定的任务。



## 选项描述格式

选项描述同样必不可少，尤其是当选项有参数，并且还需要为它赋默认值时。

为选项添加参数的格式有两种：

~~~python
-o FILE --output-FILE  
-i <file>, --input <file> 
~~~

为选项添加描述说明，只需要用两个空格分隔选项和说明即可。

为选项添加默认值时，把它添加到选择描述后即可，格式如[default: \<my-default-value>]

~~~python
--coefficient=K The K coefficient [default: 2.95]
--output=FILE Output file [default: test.txt]
--directory=DIR Some directory [default: ./]
~~~

如果选项是可以重复的，那么它的值`[default: ...]`将会一个 list列表，若不可以重复，则它的值是一个字符串。



## 使用

理解了使用模式格式和选项描述格式后，再配合给出的例子就能较好理解了。

接下来就是得到输入信息了，arguments参数是一个字典类型，包含了用户输入的选项和参数信息，还是上面的代码段例子，假如从命令行运行的输入是

~~~python
python3 test.py ship Guardian move 100 150 --speed=15
~~~

那么打印arguments参数如下：

~~~python
{'--drifting': False,
 '--help': False,
 '--moored': False,
 '--speed': '15',
 '--version': False,
 '<name>': ['Guardian'],
 '<x>': '100',
 '<y>': '150',
 'mine': False,
 'move': True,
 'new': False,
 'remove': False,
 'set': False,
 'ship': True,
 'shoot': False}
~~~

从打印信息可以看到，对于选项，使用布尔型来表示用户是否输入了该选项；对于参数，则使用具体值来表述。

这样一来，程序就可以从arguments变量中得到下一步操作。若是用户什么参数都没输入，则打印Usage说明提示输入内容。





# Epoch

当完整的数据集通过了神经网络一次，这个过程称为一个 epoch。然而，当一个 epoch 对于计算机而言太庞大的时候，就需要把它分成多个小块。

为什么要使用多于一个 epoch?

在神经网络中传递完整的数据集一次是不够的，需要将完整的数据集在同样的神经网络中传递多次。



# BATCH

在不能将数据一次性通过神经网络时，就需要将数据集分成几个 batch。

Batch size指一个 batch 中的样本总数。记住：batch size 和 number of batches 是不同的。



# 迭代

**迭代**

迭代是 batch 需要完成一个 epoch 的次数。记住：在一个 epoch 中，batch 数和迭代数是相等的。



# pd.read_csv()

dl_data.csv文件：

![image-20190426105832337](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190426105832337.png)

行索引为：[-20......19]	列索引为：[-25......25]

~~~python
q_table = pd.read_csv('dl_data.csv',encoding = "utf-8",header = 0,names = range(0,50))
~~~

将原来的列索引[-25......25]替换成[0....49]。header = 0是默认情况（即不标明，默认是header = 0），表示以数据第一行为列索引。

    encoding = "utf-8"表明以utf-8为编码规则。
    names = range(0,50)表示以[0....49]为列索引名字
~~~python
q_table = pd.read_csv('dl_data.csv',encoding = "utf-8",header = None,names = range(0,50))
~~~

给数据添加列索引[0....49]，header = None表示原始数据没有列索引。就算数据里有列索引，这时就把原来的列索引当成数据。

上面两个语句都会默认为数据添加行索引，即会把原来的行索引当成数据，自己再添加新的行索引[0,1,2.....]。如果不想添加新的行索引，代码如下:

~~~python
q_table = pd.read_csv('dl_data.csv',encoding = "utf-8",index_col=0)
~~~

index_col=0表示以原有数据的第一列(索引为0)当作行索引。

- 参数详解

**filepath_or_buffer** : str，pathlib。可以是URL，可用URL类型包括：http, ftp, s3和文件。本地文件读取实例：://localhost/path/to/table.csv

**sep** : str, default ‘,’。指定分隔符。如果不指定参数，则会尝试使用逗号分隔。

**header** : int or list of ints。指定行数用来作为列名，数据开始行数。header参数可以是一个list，例如：[0,1,3]，这个list表示将文件中的这些行作为列标题（意味着每一列有多个标题），介于中间的行将被忽略（例如本例中的数据1,2,4行将被作为多级标题出现，第3行数据将被丢弃，dataframe的数据从第5行开始）。注意：如果skip_blank_lines=True 那么header参数忽略注释行和空行，所以header=0表示第一行数据而不是文件第一行。

**names** : array-like, default None。用于结果的列名列表，如果数据文件中没有列标题行，就需要执行header=None。默认列表中不能出现重复，除非设定参数mangle_dupe_cols=True。

**index_col** : int or sequence or False, default None。用作行索引的列编号或者列名，如果给定一个序列则有多个行索引。

**mangle_dupe_cols** : boolean, default True。重复的列，将‘X’...’X’表示为‘X.0’...’X.N’。如果设定为false，则会将所有重名列覆盖。

**dtype** : Type name or dict of column -> type, default None。每列数据的数据类型。例如 {‘a’: np.float64, ‘b’: np.int32}

**converters** : dict, default None。列转换函数的字典。key可以是列名或列的序号。

**skiprows** : list-like or integer, default None。需要忽略的行数（从文件开始处算起），或需要跳过的行号列表（从0开始）。

**encoding** : str, default None。指定字符集类型，通常指定为'utf-8'.



# eval()

eval() 函数用来执行一个字符串表达式，并返回表达式的值。

```python
eval(expression[, globals[, locals]])
```

- expression -- 表达式。
- 返回值 -- 返回表达式计算结果。

```python
>>> x = 7
>>> eval( '3 * x' )
21
>>> eval('pow(2,2)')
4
>>> eval('2 + 2')
4
>>> n=81
>>> eval("n + 4")
85
```





# 切片

对于X[:,0]：取二维数组中第一维的所有数据。

对于X[:,1]：取二维数组中第二维的所有数据。

对于X[:,m:n]：取二维数组中第m维到第n-1维的所有数据。

对于X[:,:,0]：取三维矩阵中第一维的所有数据。

对于X[:,:,1]：取三维矩阵中第二维的所有数据。

对于X[:,:,m:n]：取三维矩阵中第m维到第n-1维的所有数据。

~~~python
data_list=[[1,2,3],[1,2,1],[3,4,5],[4,5,6],[5,6,7],[6,7,8],[6,7,9],[0,4,7],[4,6,0],[2,9,1],[5,8,7],[9,7,8],[3,7,9]]
data_list=np.array(data_list)

X[:,0]结果输出为：
[1 1 3 4 5 6 6 0 4 2 5 9 3]
X[:,1]结果输出为：
[2 2 4 5 6 7 7 4 6 9 8 7 7]
X[:,0:1]结果输出为：
[[1]
 [1]
 [3]
 [4]
 [5]
 [6]
 [6]
 [0]
 [4]
 [2]
 [5]
 [9]
 [3]]

data_list=[[[1,2],[1,0],[3,4],[7,9],[4,0]],[[1,4],[1,5],[3,6],[8,9],[5,0]],[[8,2],[1,8],[3,5],[7,3],[4,6]],
[[1,1],[1,2],[3,5],[7,6],[7,8]],[[9,2],[1,3],[3,5],[7,67],[4,4]],[[8,2],[1,9],[3,43],[7,3],[43,0]],
[[1,22],[1,2],[3,42],[7,29],[4,20]],[[1,5],[1,20],[3,24],[17,9],[4,10]],[[11,2],[1,110],[3,14],[7,4],[4,2]]]
data_list=np.array(data_list)

X[:,:,0]结果输出为：
[[ 1  1  3  7  4]
 [ 1  1  3  8  5]
 [ 8  1  3  7  4]
 [ 1  1  3  7  7]
 [ 9  1  3  7  4]
 [ 8  1  3  7 43]
 [ 1  1  3  7  4]
 [ 1  1  3 17  4]
 [11  1  3  7  4]]
X[:,:,1]结果输出为：
[[  2   0   4   9   0]
 [  4   5   6   9   0]
 [  2   8   5   3   6]
 [  1   2   5   6   8]
 [  2   3   5  67   4]
 [  2   9  43   3   0]
 [ 22   2  42  29  20]
 [  5  20  24   9  10]
 [  2 110  14   4   2]]

X[:,:,0:1]结果输出为：
[[[ 1]
  [ 1]
  [ 3]
  [ 7]
  [ 4]]
 
 [[ 1]
  [ 1]
  [ 3]
  [ 8]
  [ 5]]
 
 [[ 8]
  [ 1]
  [ 3]
  [ 7]
  [ 4]]
 
 [[ 1]
  [ 1]
  [ 3]
  [ 7]
  [ 7]]
 
 [[ 9]
  [ 1]
  [ 3]
  [ 7]
  [ 4]]
 
 [[ 8]
  [ 1]
  [ 3]
  [ 7]
  [43]]
 
 [[ 1]
  [ 1]
  [ 3]
  [ 7]
  [ 4]]
 
 [[ 1]
  [ 1]
  [ 3]
  [17]
  [ 4]]
 
 [[11]
  [ 1]
  [ 3]
  [ 7]
  [ 4]]]
~~~





# DataFrame

## as_matrix()

~~~python
df = pd.DataFrame(np.arange(12).reshape(3, 4))

df
   0  1   2   3
0  0  1   2   3
1  4  5   6   7
2  8  9  10  11

df.as_matrix() 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
~~~



## set_index()

set_index方法，可以设置单索引和复合索引。 

DataFrame.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False) 

append添加新索引。

~~~python
In [307]: data
Out[307]:
     a    b  c    d
0  bar  one  z  1.0
1  bar  two  y  2.0
2  foo  one  x  3.0
3  foo  two  w  4.0
 
In [308]: indexed1 = data.set_index('c')
 
In [309]: indexed1
Out[309]:
     a    b    d
c              
z  bar  one  1.0
y  bar  two  2.0
x  foo  one  3.0
w  foo  two  4.0
 
In [310]: indexed2 = data.set_index(['a', 'b'])
 
In [311]: indexed2
Out[311]:
         c    d
a   b         
bar one  z  1.0
    two  y  2.0
foo one  x  3.0
    two  w  4.0
~~~



## T

DataFrame.T：Transpose index and columns。



## to_dict()

~~~python
In [4]: df
Out[4]:
  colA colB  colC  colD
0    A    X   100    90
1    A  NaN    50    60
2    B   Ya    30    60
3    C   Xb    50    80
4    A   Xa    20    50

In [5]: df.to_dict(orient='dict')
Out[5]:
{'colA': {0: 'A', 1: 'A', 2: 'B', 3: 'C', 4: 'A'},
 'colB': {0: 'X', 1: nan, 2: 'Ya', 3: 'Xb', 4: 'Xa'},
 'colC': {0: 100, 1: 50, 2: 30, 3: 50, 4: 20},
 'colD': {0: 90, 1: 60, 2: 60, 3: 80, 4: 50}}

In [6]: df.to_dict(orient='list')
Out[6]:
{'colA': ['A', 'A', 'B', 'C', 'A'],
 'colB': ['X', nan, 'Ya', 'Xb', 'Xa'],
 'colC': [100, 50, 30, 50, 20],
 'colD': [90, 60, 60, 80, 50]}
~~~



## fillna()

`fillna`(*value=None*)

使用指定的方法填充NA / NaN值。

**value** : 变量, 字典。　　

返回：被充满的DataFrame。

~~~python
>>> df
     A       B      C        D
0  NaN  2.0    NaN    0
1  3.0    4.0   NaN     1
2  NaN  NaN  NaN     5
3  NaN  3.0    NaN    4
 
#将NAN值转换为0
>>>df.fillna(0)
    A   B   C   D
0   0.0 2.0 0.0 0
1   3.0 4.0 0.0 1
2   0.0 0.0 0.0 5
3   0.0 3.0 0.0 4

#用字典替换
>>>values = {'A': 0, 'B': 1, 'C': 2, 'D': 3}
>>> df.fillna(value=values)
    A   B   C   D
0   0.0 2.0 2.0 0
1   3.0 4.0 2.0 1
2   0.0 1.0 2.0 5
3   0.0 3.0 2.0 4
~~~





# Numpy

## random.seed()

np.random.seed()，每次运行代码时设置相同的seed，则每次生成的随机数相同。如果不设置seed，每次生成的随机数都会不一样。



## unique()

去除数组中的重复数字，并进行排序后输出。



## unstack()

![image-20190426143537470](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190426143537470.png)

![image-20190426143739054](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190426143739054.png)



## array()

 Python中提供了list容器，可以当作数组使用。但列表中的元素可以是任何对象，因此列表中保存的是对象的指针。这样一来，为保存一个简单的列表[1,2,3]，就需要三个指针和三个整数对象。对于数值运算来说，这种结构显然不够高效。

Python虽然也提供了array模块，但只支持一维数组，不支持多维数组(在TensorFlow里偏向于理解为矩阵)，也没有各种运算函数。因而不适合数值运算。

NumPy的出现弥补了这些不足。

~~~python
a = np.array([2,3,4])
[2 3 4] int32

b = np.array([2.0,3.0,4.0])
[ 2.  3.  4.] float64

c = np.array([[1.0,2.0],[3.0,4.0]])
[[ 1.  2.]
 [ 3.  4.]] float64
~~~







# TensorFlow

## get_variable()

tf.get_variable(name,  shape, initializer, trainable)。

获取具有这些参数的现有变量或创建一个新变量。 

name：变量名称，shape：变量维度，initializer：变量初始化的方式。

trainable：如果为True，则会默认将变量添加到图形集合GraphKeys.TRAINABLE_VARIABLES中。此集合用于优化器Optimizer类优化的的默认变量列表【可为optimizer指定其他的变量集合】，就是要训练的变量列表。

初始化的方式有以下几种：

tf.constant_initializer：常量初始化函数

tf.random_normal_initializer：正态分布

tf.truncated_normal_initializer：截取的正态分布

tf.random_uniform_initializer：均匀分布

tf.zeros_initializer：全是0

tf.ones_initializer：全是1

tf.uniform_unit_scaling_initializer：满足均匀分布，但不影响输出数量级的随机值。



## contrib.layers.xavier_initializer()

该函数返回一个用于初始化权重的初始化程序 “Xavier” 。这个初始化器用来保持每一层的梯度大小都差不多相同，从而保证输入变量的变化尺度不变，避免变化尺度在最后一层网络中爆炸或弥散。



## nn.relu()

将大于0的数保持不变，小于0的数置为0。

~~~python
a = tf.constant([-2,-1,0,2,3])
with tf.Session() as sess:
 	print(sess.run(tf.nn.relu(a)))
    
[0 0 0 2 3]
~~~



## multiply()

两个矩阵中对应元素各自相乘。

格式: tf.multiply(x, y, name=None)

x: 一个类型为:half, float32, float64, uint8, int8, uint16, int16, int32, int64, complex64, complex128的张量。 
y: 一个类型跟张量x相同的张量。

返回值： x * y element-wise。

（1）multiply这个函数实现的是元素级别的相乘，也就是两个相乘的矩阵元素各自相乘，而不是矩阵乘法。 
（2）两个相乘的数必须有相同的数据类型，不然会报错。



## matmul()

格式: tf.matmul(a, b, transpose_a=False, transpose_b=False, adjoint_a=False, adjoint_b=False, a_is_sparse=False, b_is_sparse=False, name=None)

参数: 
a: 一个类型为 float16, float32, float64, int32, complex64, complex128 且秩 > 1 的张量。 
b: 一个类型跟张量a相同的张量。 
transpose_a: 如果为真, a在进行乘法计算前进行转置。 
transpose_b: 如果为真, b在进行乘法计算前进行转置。 
adjoint_a: 如果为真, a在进行乘法计算前进行共轭和转置。 
adjoint_b: 如果为真, b在进行乘法计算前进行共轭和转置。 
a_is_sparse: 如果为真, a被处理为稀疏矩阵。 
b_is_sparse: 如果为真, b被处理为稀疏矩阵。 
name: 操作的名字（可选参数） 
返回值： 一个跟张量a和b类型一样的张量，且内部矩阵是a和b中的相应矩阵的乘积。

注意： （1）输入必须是矩阵，且其在转置后有相匹配的矩阵尺寸。 
（2）两个矩阵必须都是同样的类型。



## reduce_sum()

压缩求和，用于降维。

~~~python
# 'x' is [[1, 1, 1],
#		  [1, 1, 1]]

#求和
tf.reduce_sum(x) ==> 6

#按列求和
tf.reduce_sum(x, 0) ==> [2, 2, 2]

#按行求和
tf.reduce_sum(x, 1) ==> [3, 3]

#按照行的维度求和
tf.reduce_sum(x, 1, keep_dims=True) ==> [[3], [3]]

#行列求和
tf.reduce_sum(x, [0, 1]) ==> 6
~~~



## reset_default_graph()

用于清除默认图形堆栈并重置全局默认图形。

注意：默认图形是当前线程的一个属性，该tf.reset_default_graph函数只适用于当前线程。当一个tf.Session或tf.InteractiveSession激活时调用这个函数会导致未定义的行为：调用此函数后使用任何以前创建的tf.Operation或tf.Tensor对象将导致未定义的行为。



## placeholder()

~~~python
tf.placeholder(
    dtype,
    shape=None,
    name=None
)
~~~

**参数：**

1. dtype：数据类型。常用的是tf.float32，tf.float64等数值类型。
2. shape：数据形状。默认是None，就是一维值，也可以是多维（比如[2,3], [None, 3]表示列是3，行不定）
3. name：名称。

为什么要用placeholder？

```
Tensorflow的设计理念称为计算流图。在编写程序时，首先构筑整个系统的graph，代码不会直接生效，这一点和python的其他数值计算库（如Numpy等）不同，graph为静态的，类似于docker中的镜像。然后，在实际运行时，启动一个session，程序才会真正运行。这样做的好处是：避免反复切换底层程序实际运行的上下文。我们知道，很多python程序的底层为C语言或其他语言，执行一行脚本，就要切换一次，是有成本的，tensorflow通过计算流图的方式，优化整个session需要执行的代码，是很有优势的。

所以placeholder()函数是在神经网络构建graph时在模型中的占位，此时并没有把要输入的数据传入模型，它只会分配必要的内存。等建立session，在会话中，运行模型的时候通过feed_dict()函数向占位符喂入数据。
```


## nn.embedding_lookup()

~~~python
tf.nn.embedding_lookup(
               params,
               ids
)
~~~

- params: 表示完整的嵌入张量。
- ids: 一个类型为int32或int64的Tensor，包含要在params中查找的id。

tf.nn.embedding_lookup()函数的用法主要是选取一个张量里面索引对应的元素。

tf.nn.embedding_lookup(tensor,id)：即tensor就是输入的张量，id 是张量对应的索引。

tf.nn.embedding_lookup()是根据input_ids中的id，寻找embeddings中的第id行。比如input_ids=[1,3,5]，则找出embeddings中第1，3，5行，组成一个tensor返回。embedding_lookup不是简单的查表，id对应的向量是可训练的，训练参数个数应该是 category num*embedding size，也就是说lookup是一种全连接层。



## reshape()

~~~python
tf.reshape(tensor,shape,name=None)
~~~

将tensor变换为参数shape形式，其中的shape为一个列表形式。-1所代表的含义是不用亲自去指定这一维的大小，函数会自动进行计算，但列表中只能存在一个-1。
reshape进行矩阵变换的流程是： 将矩阵t变换为一维矩阵，然后再对矩阵形式进行更改。

~~~python
reshape(t,shape) =>reshape(t,[-1]) =>reshape(t,shape)
~~~



## concat()

用来拼接张量。

~~~python
tf.concat([tensor1, tensor2, tensor3,...], axis)

t1 = [[1, 2, 3], [4, 5, 6]]
t2 = [[7, 8, 9], [10, 11, 12]]
tf.concat([t1, t2], 0)  # [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]
tf.concat([t1, t2], 1)  # [[1, 2, 3, 7, 8, 9], [4, 5, 6, 10, 11, 12]]
 
# tensor t3 with shape [2, 3]
# tensor t4 with shape [2, 3]
tf.shape(tf.concat([t3, t4], 0))  # [4, 3]
tf.shape(tf.concat([t3, t4], 1))  # [2, 6]
~~~



## reduce_mean()

用于计算张量tensor沿指定数轴（tensor某一维度）上的平均值，主要用作降维或者计算平均值。

~~~python
reduce_mean(input_tensor,
                axis=None,
                keep_dims=False,
                name=None)
~~~

- input_tensor： 输入的待降维的tensor。
- axis： 指定的轴，如果不指定，则计算所有元素的均值。
- keep_dims：是否降维度，设置为True，输出的结果保持输入tensor的形状，设置为False，输出结果会降低维度。
- name： 操作名称。

~~~python
 x = [[1,2,3],
      [1,2,3]]
 
xx = tf.cast(x,tf.float32)
 
mean_all = tf.reduce_mean(xx, keep_dims=False)
mean_0 = tf.reduce_mean(xx, axis=0, keep_dims=False)
mean_1 = tf.reduce_mean(xx, axis=1, keep_dims=False)
  
with tf.Session() as sess:
    m_a,m_0,m_1 = sess.run([mean_all, mean_0, mean_1])
 
print m_a    # output: 2.0
print m_0    # output: [ 1.  2.  3.]
print m_1    #output:  [ 2.  2.]
~~~

如果设置保持原来的张量维度，keep_dims=True ，结果：

~~~python
print m_a    # output: [[ 2.]]
print m_0    # output: [[ 1.  2.  3.]]
print m_1    #output:  [[ 2.], [ 2.]]
~~~



## train.Optimizer.minimize()

~~~python
tf.train.Optimizer.minimize(loss, global_step=None, var_list=None, gate_gradients=1, 
aggregation_method=None, colocate_gradients_with_ops=False, name=None, grad_loss=None)
~~~

添加操作节点，用于最小化loss，并更新var_list。该函数是简单的合并了compute_gradients()与apply_gradients()函数，返回为优化更新后的var_list。如果global_step非None，该操作还会为global_step做自增。



## ConfigProto()

用在创建session时，对session进行参数配置。

~~~python
config = tf.ConfigProto(log_device_placement=True allow_soft_placement=True)
config.gpu_options.per_process_gpu_memory_fraction = 0.4  #占用40%显存
sess = tf.Session(config=config)
~~~

**记录设备指派情况 :  tf.ConfigProto(log_device_placement=True)**

获取 operations 和 Tensor 被指派到哪个设备(几号CPU或几号GPU)上运行，会在终端打印出各项操作是在哪个设备上运行的。

**自动选择运行设备 ： tf.ConfigProto(allow_soft_placement=True)**

允许tf自动选择一个存在且可用的设备来运行操作。

**限制GPU资源使用：**

为加快运行效率，TensorFlow在初始化时会尝试分配所有可用的GPU显存资源给自己，这在多人使用的服务器上工作就会导致GPU占用，别人无法使用GPU。

tf提供了两种控制GPU资源使用的方法，一是让TensorFlow在运行过程中动态申请显存，需要多少就申请多少；第二种方式是限制GPU的使用率。

**一、动态申请显存**

~~~python
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
session = tf.Session(config=config)
~~~

**二、限制GPU使用率**

~~~python
config = tf.ConfigProto()
config.gpu_options.per_process_gpu_memory_fraction = 0.4  #占用40%显存
session = tf.Session(config=config)


gpu_options=tf.GPUOptions(per_process_gpu_memory_fraction=0.4)
config=tf.ConfigProto(gpu_options=gpu_options)
session = tf.Session(config=config)
~~~



## InteractiveSession()

在运行图的时候，可以插入一些计算图。这些计算图是由某些操作(operations)构成的。这对于工作在交互式环境中的人来说非常便利，比如使用IPython。

tf.Session()：需要在启动session前构建整个计算图，然后启动该计算图。

意思就是在使用tf.InteractiveSession()来构建会话时，可以先构建一个session然后再定义操作（operation），如果使用tf.Session()来构建会话，需要在会话构建前定义好全部的操作（operation），然后再构建会话。

~~~python
tf.InteractiveSession：可以在运行图的时候，插入新的图，方便地使用可交互环境执行
sess = tf.InteractiveSession()
a = tf.constant(5.0)
b = tf.constant(6.0)
c = a * b
# We can just use 'c.eval()' without passing 'sess'
print(c.eval())
sess.close()

tf.Session：先用操作构建好图，再创建session，再执行图。
# Build a graph.
a = tf.constant(5.0)
b = tf.constant(6.0)
c = a * b
# Launch the graph in a session.
sess = tf.Session()
# Evaluate the tensor `c`.
print(sess.run(c))
~~~



## global_variables_initializer()

以一个简单的线性模型为例。首先，使用`tf.placeholder`定义模型输入，然后定义两个全局变量，同时它们都是训练参数，最后定义学习模型。

~~~python
x  = tf.placeholder(tf.float32, [None, 784])
W = tf.Variable(tf.zeros([784,10]), name='W')
b = tf.Variable(tf.zeros([10]), name='b') 
y = tf.matmul(x, W) + b
~~~

在使用变量前，必须对变量进行初始化。按照习惯用法，使用`tf.global_variables_initializer()`将所有全局变量的初始化器汇总，并对其进行初始化。

~~~python
init = tf.global_variables_initializer()
with tf.Session() as sess:
  sess.run(init)
~~~

按照既有经验，其计算图大致如下图所示：

![image-20190428194935825](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190428194935825.png)





# sklearn.utils.shuffle()

打乱样本。

![image-20190428202254380](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190428202254380.png)

![image-20190428202605581](/Users/vanrhyga/Library/Application Support/typora-user-images/image-20190428202605581.png)

