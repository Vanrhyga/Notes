[TOC]

## 导入包

~~~python
import pandas as pd
~~~



## 创建对象

用值的序列创建一个 Series，pandas 会创建一个默认的整数索引：

~~~python
s=pd.Series([1,3,5,np.nan,6,8])
~~~

用 numpy 数值创建一个带有 datetime 索引和列标签的数据框：

~~~python
dates=pd.date_range('20130101',periods=6)
df=pd.DataFrame(np.random.randn(6,4),index=dates,columns=list('ABCD'))
~~~

用包含对象的字典创建一个数据框，该方法与创建 Series 的方法相似：

~~~python
df2=pd.DataFrame({'A':1.,
				'B':pd.Timestamp('20130102'),
				'C':pd.Series(1,index=list(range(4)),dtype='float32'),
				'D':np.array([3]*4,dtype='int32'),
				'E':pd.Categorical(["test","train","test","train"]),
				'F':'foo'})

df2.dtypes
A	float64
B	datetime64[ns]
C	float32
D	int32	
E	category	
F	object
~~~



## 查看数据

查看整个数据的头部或尾部：

~~~python
df.head()
df.tail(3)
~~~

显示数据框的索引、列名和值：

~~~python
df.index
df.columns
df.values
~~~

描述性显示关于数据的简短统计摘要：

~~~python
df.describe()
~~~

转置数据：

~~~python
df.T
~~~

通过轴分类数据（相当于排序，axis = 1 理解为按照列名，= 0 则为索引名）：

~~~python
df.sort_index(axis=1,ascending=False)
~~~

通过值分类：

~~~python
df.sort_values(by='B')
~~~



## 选择数据

对于选择和设置数据来说，标准的 python 和 numpy 表达式非常直观，但对于交互式工作来说很难进行。因而，对于应用型代码，较为推荐最优化的 pandas 数据获取方法。

### 获取

在方括号中输入列名，来获得一个 Series，该操作相当于 df.A：

~~~python
df['A']
~~~

通过对行进行切片获取数据：

~~~python
df[0:3]						
df['20130102':'20130104']
~~~

### 由标签获取数据

用标签来截取一行数据：

~~~~python
df.loc[dates[0]]
~~~~

在多个轴上通过标签来选取数据：

~~~python
df.loc[:,['A','B']]
~~~

同时用切片和标签名索引来获取数据：

~~~python
df.loc['20130102':'20130104',['A','B']]
df.loc['20130102',['A','B']]
~~~

仅仅获取标量值：

~~~python
df.loc[dates[0],'A']
~~~

更快地获取标量值：

~~~python
df.at[dates[0],'A']
~~~

### 通过位置进行索引

通过适合的整数代表位置进行索引：

~~~python
df.iloc[3]
~~~

与 numpy 和 python 相似的操作，使用整数切片获取数据：

~~~python
df.iloc[3:5,0:2]
~~~

通过使用代表位置的整数列表获取数据，与 numpy 和 python 风格相似：

~~~python
df.iloc[[1,2,4],[0,2]]
~~~

显式切片索引行：

~~~python
df.iloc[1:3,:]
~~~

显式切片索引列：

~~~python
df.iloc[:,1:3]
~~~

显式索引数据值：

~~~python
df.iloc[1,1]
~~~

快速获取标量值：

~~~python
df.iat[1,1]
~~~

### 布尔值索引

使用单一列值选取数据：

~~~python
df[df.A>0]
~~~

从给出布尔条件的数据框获取数据：

~~~python
df[df>0]
~~~

使用 isin() 方法过滤数据：

~~~python
df2=df.copy()
df2['E']=['one','one','two','three','four','three']

df2[df2['E'].isin(['two','four'])]
~~~

### 安插

安插新列时通过索引值自动排序：

~~~python
s1=pd.Series([1,2,3,4,5,6],index=pd.date_range('20130102',periods=6))
~~~

通过标签安插值：

~~~python
df.at[dates[0],'A']=0
~~~

通过位置安插值：

~~~python
df.iat[0,1]=0
~~~

通过分配 numpy 数组安插新列：

~~~python
df.loc[:,'D']=np.array([5]*len(df))
~~~

使用 where 操作安插数据：

~~~python
df2=df.copy()
df2[df2>0]=-df2
~~~

### 缺失值

早先的 pandas 使用 np.nan 代表缺失值。

缺失值默认不会进行计算。

重新排列索引允许在指定的轴上改变/增加/删除索引。

~~~python
df1=df.reindex(index=dates[0:4],columns=list(df.columns)+['E'])
df1.loc[dates[0]:dates[1],'E']=1
~~~

删除所有含有缺失值的行：

~~~python
df1.dropna(how='any')
~~~

替换缺失值：

~~~python
df1.fillna(value=5)
~~~

通过判断缺失值获取布尔值：

~~~python
pd.isnull(df1)
~~~



## 运算

### 统计表

该操作一般不包含缺失值：

~~~python
df.mean()
~~~

在其他轴上进行相同操作：

~~~python
df.mean(1)
~~~

### 应用

对数据进行函数的应用

~~~python
df.apply(lambda x:x.max()-x.min())
~~~

### 统计值的频数

~~~python
s=pd.Series(np.random.randint(0,7,size=10))

s.value_counts()
~~~

### 字符串操作

Series 具有对字符串集合处理的能力，在 str 属性中可以对数组的每个元素进行便捷操作：

~~~python
s=pd.Series(['A','B','C','Aaba','Baca',np.nan,'CABA','dog','cat'])

s.str.lower()
~~~



## 聚合

### 组合

pandas 提供了不同的工具从而使用不同方式对索引设置逻辑和相关的代数功能。

用 concat()  函数连接 pandas 函数：

~~~python
df=pd.DataFrame(np.random.randn(10,4))

#break into pieces
pieces=[df[:3],df[3:7],df[7:]]

pd.concat(pieces)
~~~

### Join

SQL 风格的聚合方式：

~~~python
left=pd.DataFrame({'key':['foo','foo'],'lval':[1,2]})
right=pd.DataFrame({'key':['foo','foo'],'rval':[4,5]})

pd.merge(left,right,on='key')
~~~

### 附加

对数据框附加行：

~~~python
df=pd.DataFrame(np.random.randn(8,4),columns=['A','B','C','D'])

s=df.iloc[3]

df.append(s,ignore_index=True)
~~~

### 分组运算

在 group by 中，涉及一个或多个下列步骤：

- 基于一个标准分割数据到各组
- 在每个组中独立应用函数
- 结合结果到数据结构中

~~~python
df=pd.DataFrame({'A':['foo','bar','foo','bar','foo','bar','foo','foo'],
				'B':['one','one','two','three','two','two','one','three'],
				'C':np.random.randn(8),
				'D':np.random.randn(8)})
~~~

分组然后应用 sum 函数到分组结果中：

~~~python
df.groupby('A').sum()
~~~

通过多列分组获得多重索引，从而应用函数：

~~~python
df.groupby(['A','B']).sum()
~~~



## 重塑

### 有堆叠

~~~python
tuples=list(zip(*[['bar','bar','baz','baz','foo','foo','qux','qux'],
				['one','two','one','two','one','two','one','two']]))
				
index=pd.MultiIndex.from_tuples(tuples,names=['first','second'])

df=pd.DataFrame(np.random.randn(8,2).index=index,columns=['A','B'])

df2=df[:4]
~~~

stack() 方法压缩 DataFrame 的列：

~~~python
stacked=df2.stack()
~~~

### 数据透视表

~~~python
df=pd.DataFrame({'A':['one','one','two','three']*3,
				'B':['A','B','C']*4,
				'C':['foo','foo','foo','bar','bar','bar']*2
				'D':np.random.randn(12),
				'E':np.random.randn(12)})
				
pd.pivot_table(df,values='D',index=['A','B'],columns=['C'])
~~~

### 分类

pandas 可以在数据框包含分类数据：

~~~python
df=pd.DataFrame({"id":[1,2,3,4,5,6],"raw_grade":['a','b','b','a','a','e']})

df["grade"]=df["raw_grade"].astype("category")
~~~

将分类数据重命名为更有意义的名字：

~~~python
df["grade"].cat.categories=["very good","good","very bad"]
~~~

重新排列分类数据，同时添加缺失的部分：

~~~python
df["grade"]=df["grade"].cat.set_categories(["very good","bad","medium","good","very good"])
~~~

对分类数据排序：

~~~python
df.sort_values(by="grade")
~~~

对分类数据分组，会显示空的分类数据：

~~~python
df.groupby("grade").size()
~~~





 