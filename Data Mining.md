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

- 线性分类的一个例子

一个二维平面（一个超平面，在二维空间中的例子就是一条直线），如下图所示，平面上有两种不同的点，分别用两种不同的颜色表示，一种为红颜色的点，另一种则为蓝颜色的点，红颜色的线表示一个可行的超平面。

![image-20180801220430566](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801220430566.png)

从上图可以看出，红颜色的线把红颜色的点和蓝颜色的点分开来。而这条红颜色的线就是所说的超平面，即所谓的超平面的的确确把这两种不同颜色的数据点分隔开来，在超平面一边的数据点所对应的 y 全是 -1，而在另一边全是 1。

接着，可以令分类函数：

![image-20180801220718834](/var/folders/1w/qg5brywj515cgfsy3bp72ll40000gn/T/abnerworks.Typora/image-20180801220718834.png)

显然，若 f(x) = 0，那么 x 是位于超平面上的点。不妨要求对于所有满足 f(x) < 0 的点，其对应的 y 等于 -1，而 f(x)>0 则对应 y=1 的数据点。

