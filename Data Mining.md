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





