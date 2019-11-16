[TOC]

## sum

`S = sum(A)` 返回 A 沿大小不等于 1 的第一个数组维度的元素之和。

- 如果 `A` 是向量，则 `sum(A)` 返回元素之和。
- 如果 `A` 是矩阵，则 `sum(A)` 将返回包含每列总和的行向量。
- 如果 `A` 是多维数组，则 `sum(A)` 沿大小不等于 1 的第一个数组维度计算，并将这些元素视为向量。此维度会变为 `1`，而所有其他维度的大小保持不变

`S = sum(A,'all')` 计算 `A` 的所有元素的总和。

`S = sum(A,dim)` 沿维度 `dim` 返回总和。例如，如果 `A` 为矩阵，则 `sum(A,2)` 是包含每一行总和的列向量。



## linspace

`y = linspace(x1,x2)` 返回包含 `x1` 和 `x2` 之间的 100 个等间距点的行向量。

`y = linspace(x1,x2,n)` 生成 `n` 个点。这些点的间距为 `(x2-x1)/(n-1)`。



## spconvert

`S = spconvert(D)` 根据 `D` 的列，按与 [`sparse`](https://ww2.mathworks.cn/help/matlab/ref/sparse.html) 函数类似的方式构造稀疏矩阵 `S`。

- 如果 `D` 的大小为 `N`×`3`，则 `spconvert` 将使用 `D` 的列 `[i,j,re]` 构造 `S`，以使 `S(i(k), j(k)) = re(k)`。



## eye

`I = eye` 返回标量 `1`。

`I = eye(n)` 返回一个主对角线元素为 1 且其他位置元素为 0 的 `n`×`n` 单位矩阵。

`I = eye(n,m)` 返回一个主对角线元素为 1 且其他位置元素为 0 的 `n`×`m` 矩阵。



## size

`sz = size(A)` 返回一个行向量，其元素包含 `A` 的相应维度的长度。例如，如果 `A` 是一个 3×4 矩阵，则 `size(A)` 返回向量 `[3 4]`。

`szdim = size(A,dim)` 返回维度 `dim` 的长度。

当 `A` 是矩阵时，`[m,n] = size(A)` 返回行数和列数。



## eigs

`d = eigs(A)` 返回一个向量，其中包含矩阵 `A` 的六个模最大的特征值。当使用 `eig` 计算所有特征值的计算量很大时（例如对于大型稀疏矩阵来说），这是非常有用的。

`d = eigs(A,k)` 返回 `k` 个模最大的特征值。

`d = eigs(A,k,sigma)` 基于 `sigma` 的值返回 `k` 个特征值。例如，`eigs(A,k,'smallestabs')` 返回 `k` 个模最小的特征值。

`d = eigs(A,B,___)` 解算广义特征值问题 `A*V = B*V*D`。您可以选择指定 `k`、`sigma`、`opts` 或名称-值对组作为额外的输入参数。



## 矩阵切片

A(:,j)表示取A矩阵的第j列全部元素；A(i,:)表示取A矩阵的第i行全部元素。

A(i:i+m,:)表示取A矩阵第i~i+m行全部元素；A(:,k:k+m)表示取A矩阵第k~k+m列全部元素；A(i:i+m,k:k+m)表示取A矩阵第i~i+m行，并在第k~k+m列的全部元素。



## full

`A = full(S)` 将稀疏矩阵 `S` 转换为满存储结构，这样 `issparse(A)` 返回逻辑值 `0` (`false`)。



## spdiags

`spdiags` 函数将 `diag` 函数广义化。可能有四个不同运算，通过输入参数的数量加以区分。

`B = spdiags(A)` 从 `m`×`n` 矩阵 `A` 提取所有非零对角线。`B` 是一个其列为 `A` 的 `p` 条非零对角线的 `min(m,n)`×`p` 矩阵。

`B = spdiags(A,d)` 提取 `d` 指定的对角线。

`A = spdiags(B,d,m,n)` 通过获取 `B` 的列并沿 `d` 指定的对角线放置它们，来创建一个 `m`×`n` 稀疏矩阵。



## struct2cell

`C = struct2cell(S)` 将结构体转换为元胞数组。元胞数组 `C` 包含从 `S` 的字段复制的值。

`struct2cell` 函数不返回字段名称。要返回元胞数组中的字段名称，请使用 `fieldnames` 函数。

```matlab
S.x = linspace(0,2*pi);
S.y = sin(S.x);
S.title = 'y = sin(x)'
```

```
S = struct with fields:
        x: [1x100 double]
        y: [1x100 double]
    title: 'y = sin(x)'
```

将 `S` 转换为元胞数组。

```matlab
C = struct2cell(S)
```

```
C = 3x1 cell array
    {1x100 double}
    {1x100 double}
    {'y = sin(x)'}
```

元胞数组不包含字段名称。要返回元胞数组中的字段名称，请使用 `fieldnames` 函数。`fieldnames` 和 `struct2cell` 以相同的顺序返回字段名称和值。

```matlab
fields = fieldnames(S)
```

```
fields = 3x1 cell array
    {'x'    }
    {'y'    }
    {'title'}
```



## cell2mat

`A = cell2mat(C)` 将元胞数组转换为普通数组。元胞数组的元素必须全都包括相同的数据类型，并且生成的数组也是该数据类型。

`C` 的内容必须支持串联到 N 维矩形中。否则，结果将不确定。例如，同一列中的元胞的内容必须具有相同的列数，但不需要具有相同的行数（见图）。

![1573797627735](C:\Users\Ruijia Wang\AppData\Roaming\Typora\typora-user-images\1573797627735.png)



## norm

向量的1范数，即各元素绝对值之和，norm（a，1）。

 向量的2范数，即各元素的平方和再开平方根，norm（a，2）。

矩阵的L1范数，即各元素绝对值之和，sum(sum(abs(A)))。

 矩阵F范数，即各个元素平方和再开平方根，它通常也叫做矩阵的L2范数，norm（A，‘fro’）。