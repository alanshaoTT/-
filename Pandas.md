# Pandas

## 1.简介

`numpy`能够帮我们处理处理数值型数据，但是这还不够， 很多时候，我们的数据除了数值之外，还有字符串，还有时间序列等

Pandas是一个强大的**分析结构化数据的工具集**，基于`NumPy`构建，提供了**高级数据结构和数据操作工具**，它是使Python成为强大而高效的数据分析环境的重要因素之一。

* 一个强大的分析和操作大型结构化数据集所需的工具集

* 基础是`NumPy`，提供了高性能矩阵的运算

* 提供了大量能够快速便捷地处理数据的函数和方法

* 应用于数据挖掘，数据分析

* 提供数据清洗功能

## 2.Series

类似于一维数组，有一组数据和一组索引组成。

### 1. 基本使用

```
import numpy as np
from pandas import Series

print(Series(range(3)))
print("#" * 30)
print(Series(range(3), index = ["first", "second", "third"]))
print("#" * 30)
print(Series(range(3), index = ["first", "second", "third"], dtype=int))
print("#" * 30)
print(Series(np.array(range(3)), index = ["first", "second", "third"], dtype=int))
print("#" * 30)
print(Series({"first": 1, "second": 2, "third": 3}, dtype=int))

0    0
1    1
2    2
dtype: int64
##############################
first     0
second    1
third     2
dtype: int64
##############################
first     0
second    1
third     2
dtype: int32
##############################
first     0
second    1
third     2
dtype: int32
##############################
first     1
second    2
third     3
dtype: int32
```

### 2.属性

Series对象的属性有：`dtype, index, values, name`
`Series.index`有属性:`name`

```
series0 = Series(np.array(range(3)), index = ["first", "second", "third"], dtype=int)
series0.index = ["语文", "数学", "英语"]
print(series0.dtype)
print("##############################")
print(series0.index)
print("##############################")
print(series0.values)

series0.name = "Series0"
series0.index.name = "idx"
print("##############################")
print(series0)

int32
##############################
Index(['语文', '数学', '英语'], dtype='object')
##############################
[0 1 2]
##############################
idx
语文    0
数学    1
英语    2
Name: Series0, dtype: int32
```

## 3.Pandas的索引操作

### 1. Series和DataFrame中的索引都是Index对象

```
print(type(ser_obj.index))
print(type(df_obj2.index))

print(df_obj2.index)

<class 'pandas.indexes.range.RangeIndex'>
<class 'pandas.indexes.numeric.Int64Index'>
Int64Index([0, 1, 2, 3], dtype='int64')
```

### 2.索引对象不可变，保证数据安全

### 3.常用索引对象

1. Index，索引
2. Int64Index，整数索引
3. MultiIndex，层级索引
4. DatetimeIndex，时间戳类型

### 4.series索引

#### 4.1 index指定行索引名

```
ser_obj = pd.Series(range(5), index = ['a', 'b', 'c', 'd', 'e'])
print(ser_obj.head())

a    0
b    1
c    2
d    3
e    4
dtype: int64
```

#### 4.2 行索引

```
# 行索引
print(ser_obj['b'])
print(ser_obj[2])

1
2
```

####4.3 切片索引

```ser_obj[2:4], ser_obj[‘label1’: ’label3’]```

```
# 切片索引
print(ser_obj[1:3])
print(ser_obj['b':'d'])

b    1
c    2
dtype: int64
b    1
c    2
d    3
dtype: int64
```

#### 4.4 不连续索引

```
# 不连续索引
print(ser_obj[[0, 2, 4]])
print(ser_obj[['a', 'e']])

a    0
c    2
e    4
dtype: int64
a    0
e    4
dtype: int64
```

#### 4.5 布尔索引

```
# 布尔索引
ser_bool = ser_obj > 2
print(ser_bool)
print(ser_obj[ser_bool])

print(ser_obj[ser_obj > 2])

a    False
b    False
c    False
d     True
e     True
dtype: bool
d    3
e    4
dtype: int64
d    3
e    4
dtype: int64
```

### 5 DataFrame索引

#### 5.1 columns指定列索引名

```
df_obj = pd.DataFrame(np.random.randn(5,4), columns = ['a', 'b', 'c', 'd'])
print(df_obj.head())

          a         b         c         d
0 -0.241678  0.621589  0.843546 -0.383105
1 -0.526918 -0.485325  1.124420 -0.653144
2 -1.074163  0.939324 -0.309822 -0.209149
3 -0.716816  1.844654 -2.123637 -1.323484
4  0.368212 -0.910324  0.064703  0.486016
```

#### 5.2 使用索引

```
print(df_obj['a']) # 返回Series类型

0   -0.241678
1   -0.526918
2   -1.074163
3   -0.716816
4    0.368212
Name: a, dtype: float64
```

## 4. Pandas函数应用

### 1.可直接使用NumPy的函数

```
# Numpy ufunc 函数
df = pd.DataFrame(np.random.randn(5,4) - 1)
print(df)

print(np.abs(df))

          0         1         2         3
0 -0.062413  0.844813 -1.853721 -1.980717
1 -0.539628 -1.975173 -0.856597 -2.612406
2 -1.277081 -1.088457 -0.152189  0.530325
3 -1.356578 -1.996441  0.368822 -2.211478
4 -0.562777  0.518648 -2.007223  0.059411

          0         1         2         3
0  0.062413  0.844813  1.853721  1.980717
1  0.539628  1.975173  0.856597  2.612406
2  1.277081  1.088457  0.152189  0.530325
3  1.356578  1.996441  0.368822  2.211478
4  0.562777  0.518648  2.007223  0.059411
```

### 2.通过apply将函数应用到列或行上

```
# 使用apply应用行或列数据
f = lambda x : x.max()
print(df.apply(lambda x : x.max()))

0   -0.062413
1    0.844813
2    0.368822
3    0.530325
dtype: float64
```

注意：函数应用默认按照列方向，可以通过`print(df.apply(lambda x : x.max(), axis=1))`指定按照行方向

### 3.通过applymap将函数应用到每个数据上

```
# 使用applymap应用到每个数据
f2 = lambda x : '%.2f' % x
print(df.applymap(f2))

       0      1      2      3
0  -0.06   0.84  -1.85  -1.98
1  -0.54  -1.98  -0.86  -2.61
2  -1.28  -1.09  -0.15   0.53
3  -1.36  -2.00   0.37  -2.21
4  -0.56   0.52  -2.01   0.06
```

## 5.数据读取与存储

### 1.CSV文件

读取csv文件`read_csv`*(file_path or buf,usecols,encoding)*:`file_path`：文件路径,`usecols`:指定读取的列名，`encoding`:编码

```
data = pd.read_csv('d:/test_data/food_rank.csv',encoding='utf8')
data.head()
    name    num
0    酥油茶    219.0
1    青稞酒    95.0
2    酸奶    62.0
3    糌粑    16.0
4    琵琶肉    2.0

#指定读取的列名
data = pd.read_csv('d:/test_data/food_rank.csv',usecols=['name'])
data.head()
    name
0    酥油茶
1    青稞酒
2    酸奶
3    糌粑
4    琵琶肉

#如果文件路径有中文，则需要知道参数engine='python'
data = pd.read_csv('d:/数据/food_rank.csv',engine='python',encoding='utf8')
data.head()
    name    num
0    酥油茶    219.0
1    青稞酒    95.0
2    酸奶    62.0
3    糌粑    16.0
4    琵琶肉    2.0
#建议文件路径和文件名，不要出现中文
```

### 2.写入文件

`**DataFrame**:to_csv(file_path or buf,sep,columns,header,index,na_rep,mode)：file_path`：保存文件路径,默认`None,sep`:分隔符,默认’,’ ,`columns`:是否保留某列数据,默认`None,header`：是否保留列名,默认`True,index`:是否保留行索引,默认`True,na_rep:`指定字符串来代替空值,默认是空字符,`mode`:默认`’w’`,追加`’a’`

```
**Series**:`Series.to_csv`\
 (_path=None_,_index=True_,_sep='_,_'_,_na\_rep=''_,_header=False_,_mode='w'_,_encoding=None_\)
```















