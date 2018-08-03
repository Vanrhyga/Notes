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



