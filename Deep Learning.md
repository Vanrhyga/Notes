[TOC]

## 神经网络和深度学习

### 什么是神经网络？

常用深度学习这个术语来指训练神经网络的过程。

从趋近于零开始，然后变成一条直线，这个函数被称为 ReLU 激活函数，全称是 Rectified Linear Unit（修正线性单元）。修正，指的是取不小于 0 的值，Rectify 可以理解成 max(0,x)。

每个第一层神经元的输入都来自所有特征。

神经网络的一部分神奇之处在于，当你实现它之后，要做的只是输入 x，就能得到输出 y。因为它可以自己计算训练集中样本的数目以及所有的中间过程。输入层和中间层被紧密地连接起来。



### 神经网络的监督学习

神经网络有很多的种类，但事实表明，到目前几乎所有由神经网络创造的经济价值，本质上都离不开一种叫做监督学习的机器学习类别。

对于图像应用，经常在神经网络上使用卷积（Convolutional Neural Network），缩写为 CNN。

递归神经网络（RNN）非常适合一维序列，数据可能是一个时间组成部分。

结构化数据意味着数据的基本数据库，即每个特征都有一个很好的定义。而非结构化数据，比如音频、图像、文本等。这里的特征可能是图像中的像素值或文本中的单词。



### 为什么深度学习会兴起？

我们收集到了大量的数据，远超过机器学习算法能够高效发挥它们优势的规模。

如果想要获得较高的性能体现，有两个条件要完成。一是需要训练一个足够大的神经网络，以发挥数据规模巨大的优点；二是需要很多的数据。



## 神经网络的编程基础

### 二分类

逻辑回归（logistic regression）是一个用于二分类（binary classification）的算法。

为了保存一张图片，需要保存三个矩阵，它们分别对应图片中的红、绿、蓝三种颜色通道。把这些亮度像素值提取出来，然后放入一个特征向量 x。常用 n 来表示输入特征向量 x 的维度。所以，在二分类问题中，目标就是习得一个分类器，以图片的特征向量作为输入，然后预测输出结果 y 为 1 还是 0。

将样本数据堆叠在矩阵的列中，会更加容易地实现一个神经网络。

X 是一个规模为 n 乘以 m 的矩阵。当使用 Python 实现时，X.shape 用于显示矩阵的规模，等于(n,m)。Y 是一个规模为 1 乘以 m 的矩阵。



### 逻辑回归

用 w 来表示逻辑回归的参数，也是一个 n 维向量（因为 w 实际上是特征权重，维度与特征向量相同），参数中还有 b，表示偏差。输出 y'，是对实际值 y 的估计，或者说表示 y 等于 1 的一种可能性。

尝试 y'=w(转置)x+b，这是在做线性回归时所用到的，但这对于二元分类问题不是一个非常好的算法，因为 y' 应该在 0 到 1 之间。因此，在逻辑回归中，输出 y' 等于由上面得到的线性函数式子作为自变量的 sigmoid 函数，将线性函数转换为非线性函数。



### 逻辑回归的代价函数

通过训练代价函数来得到参数 w 和参数 b。

损失函数又叫做误差函数，用来衡量算法的运行情况，Loss function：L(y',y)。一般我们用预测值和实际值的平方差或平方差的一半来衡量，但通常在逻辑回归中不这么做，因为在后期的优化中，它会变成非凸的，从而得到很多个局部最优解或者说梯度下降法，可能找不到全局最优解。

在这门课中有很多的函数效果和这个类似，如果 y 等于 1，就尽可能让 y' 变大；如果 y 等于 0，就尽可能让 y' 变小。

损失函数是在单个训练样本中定义的，因而需要定义一个算法的代价函数 J(w,b)，即对 m 个样本的损失函数求和然后除以 m。



### 梯度下降法

由于逻辑回归的代价函数特性，我们必须定义其为凸函数。

~~~
初始化 w 和 b。对于逻辑回归几乎所有的初始化方法都有效，因为函数是凸函数，无论在哪里初始化，应该达到同一点或大致相同的点。
朝最陡的下坡方向走一步，不断迭代。
直到走到全局最优解或接近全局最优解的地方。
~~~



### 计算图

一个神经网络的计算，都是按照前向或反向传播过程组织的。首先，计算出一个新的网络的输出（前向过程），紧接着进行一个反向传输操作，后者用来计算对应的梯度或导数。



### 计算图的导数计算

链式法则

在程序里 dvar 表示导数，最终变量对各种中间量的导数。



### 逻辑回归中的梯度下降

全局代价函数对 w 的微分是各项损失对 w 微分的平均。

~~~python
J=0;dw1=0;dw2=0;db=0;
for i=1 to m
	z(i)=wx(i)+b;
	a(i)=sigmoid(z(i));
	J+=-[y(i)log(a(i))+(1-y(i))log(1-a(i))];
	dz(i)=a(i)-y(i);
	dw1+=x1(i)dz(i);
	dw2+=x2(i)dz(i);
	db+=dz(i);
J/=m;
dw1/=m;
dw2/=m;
db/=m;
w=w-alpha*dw;
b=b-alpha*db
~~~

这种计算中有两个缺点，也就是需要编写两个 for 循环。第一个 for 循环是一个小循环遍历 m 个训练样本，第二个 for 循环是遍历所有特征。

应用深度学习算法，会发现在代码中显示地使用 for 循环导致算法很低效。所以向量化技术，可以允许摆脱这些显示的 for 循环。



### 向量化

~~~python
z=np.dot(w,x)+b
~~~

~~~python
import numpy as np	#导入 numpy 库
a=np.array([1,2,3,4])	#创建一个数据 a
print(a)
#[1 2 3 4]

import time		#导入时间库
a=np.random.rand(1000000)
b=np.random.rand(1000000)	#通过 round 随机得到两个一百万维度的数组
tic=time.time()		#现在测量一下当前时间

#向量化的版本
c=np.dot(a,b)
toc=time.time()
print("Vectorized version:"+str(1000*(toc-tic))+"ms")	#打印向量化版本的时间

#继续增加非向量化的版本
c=0
tic=time.time()
for i in range(1000000):
	c+=a[i]*b[i]
toc=time.time()
print(c)
print("For loop:"+str(1000*(toc-tic))+"ms")		#打印 for 循环版本的时间
~~~

如果使用了 built-in 函数，像 np.function 或并不要求实现循环的函数，可以让 python 充分利用并行化计算。

GPU 更加擅长 SIMD 计算。

经验法则是，无论什么时候，避免使用明确的 for 循环。

如果有一个向量 v，并且想要对向量 v 的每个元素做指数操作，从而得到向量 u。首先，初始化向量 u=np.zeros(n,1)，并且通过循环依次计算每个元素。但事实可以通过 python 的 numpy 内置函数计算，u=np.exp(v)。

numpy 库有很多向量函数。如 u=np.log 是计算对数函数，np.abs() 是计算数据的绝对值，np.maximum() 计算元素中最大值，也可以用 np.maximum(v,0)，v**2 代表获得每个元素的平方。



### 向量化逻辑回归

Z=np.dot(w.T,X)+b。python 中有个巧妙的地方，这里 b 是一个实数，但是当向量加上这个实数时，会自动扩展成 1xm 的行向量，这被称为广播。



### 向量化逻辑回归的梯度输出

~~~
db=1/m*np.sum(dZ)
dw=1/m*X*dZ.T
~~~



### python 中的广播

~~~python
import numpy as np
A=np.array([[56.0,0.0,4.4,68.0],[1.2,104.0,52.0,8.0],[1.8,135.0,99.0,0.9]])
print(A)

cal=A.sum(axis=0)
print(cal)

percentage=100*A/cal.reshape(1,4)
print(percentage)
~~~

axis 用来指明将要进行的运算是沿哪个轴执行，在 numpy 中，0 轴是垂直的，而 1 轴是水平的。

技术上来讲，不需要再将矩阵 cal 重塑成 1x4。但是当我们不确定矩阵维度时，通常会对矩阵进行重塑来确保得到想要的列向量或行向量。reshape 是个常量时间的操作，调用代价极低。

广播机制与执行的运算种类无关。

如果两个数组的后缘维度的轴长度相符或其中一方的轴长度为 1，则认为它们是广播兼容的。广播会在缺失维度和轴长度为 1 的维度上进行。

后缘维度的轴长度：A.shape[-1]，即矩阵维度元组中最后一个位置的值。



### 关于numpy 向量的说明

如果将一个列向量添加到一个行向量中，你以为会报出维度不匹配或类型错误之类的错误，但实际上会得到一个行向量和列向量的求和。

~~~python
import numpy as np
a=np.random.randn(5)
print(a)	#一维数组，既不是一个行向量，也不是一个列向量

print(a.shape)

print(a.T)	#和 a 相同

print(np.dot(a,a.T))	#得到一个数
~~~

建议编写神经网络时，不要在它的结构是一维数组时使用数据结构。

~~~python
a=np.random.randn(5,1)
print(a)

print(a.T)	#行向量

print(np.dot(a,a.T))	#返回一个矩阵
~~~

每次创建一个数组，都让它成为一个列向量或行向量，其行为会更容易被理解。

使用 assert() 确保它是一个向量。





## 浅层神经网络

### 神经网络的表示

输入特征 x1、x2、x3等，被竖直地堆叠起来，叫做神经网络的输入层。最后一层只由一个结点构成，称为输出层，负责产生预测值。隐藏层的含义：在训练集中，中间结点的准确值是不知道的，也就是说看不到它们在训练集中应具有的值。

alpha表示激活，它意味着网络中不同层的值会传递到它们后面的层中，即每个神经元的输出。

计算网络的层数时，输入层不算入总层数。常将输入层称为第零层。



### 计算一个神经网络的输出

向量化的过程是将神经网络中的一层神经元参数纵向堆积起来。



### 多样本向量化

在垂直方向，索引对应于神经网络中的不同结点。水平方向上，对应于不同的训练样本。



### 激活函数

tanh 函数是 sigmoid 向下平移和伸缩后的结果，使其穿过（0,0），并且值域介于 +1 和 -1 之间。

结果表明，如果在隐藏层上使用 tanh 效果总是优于 sigmoid 函数。因为函数值域在 -1 和 +1 的激活函数，其均值是更接近 0 的，而不是 0.5。

但有一个例外：在二分类问题中，对于输出层，因为 y 的值是 0 或 1，所以需要使用 sigmoid 激活函数。

在不同的神经网络层中，激活函数可以不同。

sigmoid 函数和 tanh 函数共同的缺点是：在 z 特别大或特别小的情况下，导数的梯度会变得特别小，最后接近于 0，导致梯度下降的速度降低。

有一些选择激活函数的经验法则：如果输出是 0、1值（二分类问题），则输出层选择 sigmoid 函数，其他的所有单元都选择 ReLU 函数。

如果在隐藏层上不确定使用哪个激活函数，通常会使用 ReLU 激活函数。有时，也会使用 tanh 激活函数，但 ReLU 的一个优点是：当 z 是负值的时候，导数等于 0。

ReLU 和 Leaky ReLU函数的优点是：第一，程序实现是一个 if-else 语句，而 sigmoid 函数需要进行浮点四则运算，在实践中，使用 ReLU 激活函数神经网络通常会比使用 sigmoid 或 tanh 学习得更快；第二，sigmoid 和 tanh 函数的导数在正负饱和区的梯度都会接近于 0，从而造成梯度弥散。（ReLU 进入负半区的时候，梯度为 0，神经元此时不会训练，产生所谓的稀疏性。而 Leaky ReLU 不会有这种问题）

z 在 ReLU 的梯度一半都是 0，但是，有足够的隐藏层使得 z 值大于 0，所以对大多数的训练数据来说学习过程仍然可以很快。

~~~
sigmoid 函数：除了输出层是二分类问题基本不会用它
tanh 函数：非常优秀，几乎适合所有场合
ReLU 函数：最常用的默认函数
~~~

建议：如果不确定哪一个激活函数效果更好，可以把它们都试试，然后在验证集或发展集上进行评价。如果仅仅遵守使用默认的 ReLU 激活函数，那就很可能在近期或往后，每次解决问题都使用相同的办法。



### 为什么需要非线性激活函数？

如果使用线性激活函数或没有使用一个激活函数，那么无论神经网络有多少层一直在做的只是计算线性函数，所以不如直接去掉全部隐藏层。

只有一个地方可以使用线性激活函数，就是在做机器学习中的回归问题。

总之，不能在隐藏层用线性激活函数，唯一可以用线性激活函数的通常就是输出层；除了这种情况，在隐层用线性函数的，比如与压缩有关的，将不深入讨论。



### 随机初始化

对于一个神经网络，如果把权重或参数都初始化为 0，那么梯度下降将不会起作用。

~~~
W(1)=np.random.randn(2,2)*0.01
b(1)=np.zeros((2,1))
W(2)=np.random.randn(2,2)*0.01
~~~

通常倾向于初始化为很小的随机数，因为这种情况下很可能停在 tanh/sigmoid 函数平坦的地方。这些地方梯度很小也就意味着下降会很慢，因此学习也就很慢。





## 深层神经网络

### 为什么使用深层表示？

深度神经网络的许多隐藏层中，较早的前几层能学习一些低层次的简单特征，等到后几层，就能把简单的特征结合起来，去探测更加复杂的东西。

深层的网络隐藏单元数量相对较少，隐藏层数目较多，如果浅层的网络想要达到同样的计算结果则需要指数级增长的单元数量。



###参数 vs 超参数

什么是超参数？比如算法中的 learning rate（学习率）、iterations（梯度下降法循环的数量）、L（隐藏层数目）、隐藏层单元数目、choice of activation function（激活函数的选择）都需要人为设置。这些数字实际上控制了最后的参数 W 和 b 的值，所以被称作超参数。

如何寻找超参数的最优值？走 Idea-Code-Experiment-Idea 这个循环，尝试各种不同的参数，实现模型并观察是否成功，然后再迭代。





## 改善深层神经网络：超参数调试、正则化以及优化

### 训练，验证，测试集

开始对训练集执行算法，通过验证集或简单交叉验证集选择最好的模型，然后在测试集上进行评估。

小数据量时代，常见做法是将所有数据三七分，即 70% 训练集，30% 测试集；也可以按照 60% 训练集，20% 验证集和 20% 测试集来划分。

大数据时代，假设我们有 100 万条数据，训练集占 98%，验证集和测试集各占 1%；对于数据量过百万的应用，训练集可以占到 99.5%，验证和测试集各占 0.25%。

建议尽量确保验证集和训练集的数据来自同一分布。



### 偏差，方差

高偏差，称为“欠拟合”；方差较高，数据过度拟合。

假定训练集误差是 1%，而验证集误差是 11%，可以看出训练集设置得非常好，而验证集设置相对较差，可能过度拟合了训练集。在某种程度上，验证集并没有充分利用交叉验证集的作用，称之为“高方差”。

假定训练集误差是 15%，而验证集误差是 16%，算法并没有在训练集中得到很好训练，就是数据欠拟合，可以说这种算法偏差比较高。相反，它对于验证集产生的结果却是合理的。

最优误差被称为贝叶斯误差，如果它为 15%，我们再看训练误差 15%，验证误差 16%的这个分类器，偏差不高，方差也非常低。



### 机器学习基础

初始模型训练完成后，如果偏差较高，需要选择一个新的网络，比如含有更多隐藏层或隐藏单元的网络，或花费更多时间来训练网络，或尝试更先进的优化算法。

一旦偏差降低到可以接受的数值，需要检查方差有没有问题。要查看验证集性能。如果方差高，最好的解决方法是采用更多数据；无法获得时，尝试通过正则化来减少过拟合。

只要正则适度，通常构建一个更大的网络便可以在不影响方差的情况下减少偏差；而采用更多数据，通常可以在不过多影响偏差的同时，减少方差。



### 正则化

深度学习可能存在过拟合问题——高方差，有两种解决方法，一是正则化，二是准备更多数据。

将逻辑回归模型 L2 正则化，lambda/(2m) 乘以 w 范数的平方，w 欧几里得范数的平方等于 Wj（j 值从 1 到 n）平方的和。

如果使用 L1 正则化，w 最终会是稀疏的，也就是说 w 向量化中有很多 0。有人说这样有利于压缩模型，存储模型所占用的内存更少。但这并不是 L1 正则化的目的，因而训练网络时，越来越倾向于使用 L2 正则化。

lambda 是正则化参数，通常使用验证集或交叉验证集来配置。需要考虑训练集之间的权衡，把参数设置为较小值，可以避免过拟合。

神经网络的 L2 正则化：矩阵的平方范数被定义为矩阵中所有元素的平方求和，该矩阵范数被称作“弗罗贝尼乌斯范数”，用下标 F 标注。

L2 正则化有时被称为“权重衰减”。



### 为什么正则化有利于预防过拟合呢？

如果正则化 lambda 设置得足够大，权重矩阵 W 被设置为接近于 0 的值，直观理解就是把多隐藏单元的权重设为 0，于是基本上消除了这些隐藏单元的许多影响。这种情况，被大大简化了的神经网络会变成一个很小的网络，可是深度却很大，会使这个网络从过度拟合的状态更接近高偏差状态。但是 lambda 会存在一个中间值。



### dropout 正则化

随机失活，遍历网络的每一层，并设置消除神经网络中结点的概率。

如何实施 dropout 呢？最常用的方法是 inverted dropout（反向随机失活）：keep-prob，表示保留某个隐藏单元的概率。用 python 实现该算法，d 是一个布尔型数组，值为 true 和 false，而不是 0 和 1，乘法运算依然有效。最后向外扩展 a，用它除以 keep-prob 参数，是为了不影响 z 的期望值。

在测试阶段不使用 dropout 函数，因为不期望输出结果是随机的。



### 理解 dropout










