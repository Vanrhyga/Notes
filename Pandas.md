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

