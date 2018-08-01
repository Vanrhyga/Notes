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

