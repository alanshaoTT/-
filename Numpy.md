

# Numpy

## 1. 简介

是一个由多维数组对象和用于处理数组的例程集合组成的库。

可以使用numpy完成以下功能：

	* 数组的算数和逻辑运算
	* 傅立叶变换和用于图形操作的例程
	* 与线性代数有关的操作。 NumPy 拥有线性代数和随机数生成的内置函数



## 2. Ndarray

NumPy 中定义的最重要的对象是称为 `ndarray` 的 N 维数组类型。 它描述相同类型的元素集合。 可以使用基于零的索引访问集合中的项目。

`ndarray`中的每个元素在内存中使用相同大小的块。 `ndarray`中的每个元素是数据类型的对象（称为 `dtype`）。

从`ndarray`对象提取的任何元素（通过切片）由一个数组标量类型的 Python 对象表示。

基本的`ndarray`是使用 NumPy 中的数组函数创建的，如下所示：

```python
numpy.array

#dtype指定数组类型 copy指定是否允许复制 order指定按照行列 subok是否强制为基类数组 ndmin指定最小维数
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)

```



## 3. Numpy数组属性

### 1.ndarray.shape

返回一个包含数组维度的元组。

```python
import numpy as np
a = np.array([[2,3],[3,2]])
print a.shape
```

```python
(2,2)
```

```
a = np.array([[1,2,3],[4,5,6]])
a.shape =  (3,2)  
print a 
```

```

[[1, 2] 
 [3, 4] 
 [5, 6]]

```

```
a = np.array([[1,2,3],[4,5,6]]) 
b = a.reshape(3,2)  
print b
```

```
[[1, 2] 
 [3, 4] 
 [5, 6]]
```

### 2.ndarray.ndim

返回数组维数

```
a = np.arange(24)  
print a


[0 1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16 17 18 19 20 21 22 23] 
```

```

a = np.arange(24) a.ndim 
# 现在调整其大小
b = a.reshape(2,4,3)  
print b 
# b 现在拥有三个维度


[[[ 0,  1,  2] 
  [ 3,  4,  5] 
  [ 6,  7,  8] 
  [ 9, 10, 11]]  
  [[12, 13, 14] 
   [15, 16, 17]
   [18, 19, 20] 
   [21, 22, 23]]]
```

### 3.numpy.itemsize

返回元素的大小

```

x = np.array([1,2,3,4,5], dtype = np.int8)  
print x.itemsize

1
```



## 4. 数组创建例程

### 1.numpy.zeros ones

```
x = np.zeros(5)  
print x

返回长度为5的0数组
```

```
x = np.ones([2,2], dtype =  int)  
print x


[[1  1] 
 [1  1]]
```

### 2.numpy.asarray

将其他类型数据转换为数组，比如列表、列表的元组、元组、元组的元组、元组的列表

```
numpy.asarray(a, dtype = None, order = None)
```

```
x =  [(1,2,3),(4,5)] 
a = np.asarray(x)  
print a


[(1, 2, 3) (4, 5)]
```

### 3.numpy.arange

返回数值范围的数组

```

numpy.arange(start, stop, step, dtype)


x = np.arange(10,20,2)  
print x

[10  12  14  16  18]

```

## 5. 切片和索引

如前所述，`ndarray`对象中的元素遵循基于零的索引。 有三种可用的索引方法类型： **字段访问，基本切片**和**高级索引**。

基本切片是 Python 中基本切片概念到 n 维的扩展。 通过将`start`，`stop`和`step`参数提供给内置的`slice`函数来构造一个 Python `slice`对象。 此`slice`对象被传递给数组来提取数组的一部分。

```
#通过slice对象获得
a = np.arange(10)
s = slice(2,7,2)  
print a[s]

#通过直接操作数组
a = np.arange(10)
b = a[2:7:2]  
print b

[2  4  6]
```

切片还可以包括省略号（`...`），来使选择元组的长度与数组的维度相同。 如果在行位置使用省略号，它将返回包含行中元素的`ndarray`。

```
a = np.array([[1,2,3],[3,4,5],[4,5,6]])  
print  '我们的数组是：'  
print a
print  '\n'  
# 这会返回第二列元素的数组：  
print  '第二列的元素是：'  
print a[...,1]  
print  '\n'  
# 现在我们从第二行切片所有元素：  
print  '第二行的元素是：'  
print a[1,...]  
print  '\n'  
# 现在我们从第二列向后切片所有元素：
print  '第二列及其剩余元素是：'  
print a[...,1:]
```

## 6. 高级索引

### 1.布尔索引

```

x = np.array([[  0,  1,  2],[  3,  4,  5],[  6,  7,  8],[  9,  10,  11]])  
print  '我们的数组是：'  
print x 
print  '\n'  
# 现在我们会打印出大于 5 的元素  
print  '大于 5 的元素是：'  
print x[x >  5]

a = np.array([np.nan,  1,2,np.nan,3,4,5])  
print a[~np.isnan(a)]


a = np.array([1,  2+6j,  5,  3.5+5j])  
print a[np.iscomplex(a)]
```



## 7. 广播

术语**广播**是指 NumPy 在算术运算期间处理不同形状的数组的能力。 对数组的算术运算通常在相应的元素上进行。 如果两个阵列具有完全相同的形状，则这些操作被无缝执行。

```

a = np.array([1,2,3,4]) 
b = np.array([10,20,30,40]) 
c = a * b 
print c


[10   40   90   160]
```

如果两个数组的维数不相同，则元素到元素的操作是不可能的。 然而，在 NumPy 中仍然可以对形状不相似的数组进行操作，因为它拥有广播功能。 较小的数组会**广播**到较大数组的大小，以便使它们的形状可兼容。

如果满足以下规则，可以进行广播：

- `ndim`较小的数组会在前面追加一个长度为 1 的维度。
- 输出数组的每个维度的大小是输入数组该维度大小的最大值。
- 如果输入在每个维度中的大小与输出大小匹配，或其值正好为 1，则在计算中可它。
- 如果输入的某个维度大小为 1，则该维度中的第一个数据元素将用于该维度的所有计算。

如果上述规则产生有效结果，并且满足以下条件之一，那么数组被称为**可广播的**。

- 数组拥有相同形状。
- 数组拥有相同的维数，每个维度拥有相同长度，或者长度为 1。
- 数组拥有极少的维度，可以在其前面追加长度为 1 的维度，使上述条件成立。

```

a = np.array([[0.0,0.0,0.0],[10.0,10.0,10.0],[20.0,20.0,20.0],[30.0,30.0,30.0]]) 
b = np.array([1.0,2.0,3.0])  
print  '第一个数组：'  
print a 
print  '\n'  
print  '第二个数组：'  
print b 
print  '\n'  
print  '第一个数组加第二个数组：'  
print a + b

第一个数组：
[[ 0. 0. 0.]
 [ 10. 10. 10.]
 [ 20. 20. 20.]
 [ 30. 30. 30.]]
 
第二个数组：
[ 1. 2. 3.]
 
第一个数组加第二个数组：
[[ 1. 2. 3.]
 [ 11. 12. 13.]
 [ 21. 22. 23.]
 [ 31. 32. 33.]]

```

广播如图

![img](https://www.tutorialspoint.com//numpy/images/array.jpg)



## 8.数组重塑与打平

### 1.重塑 numpy.reshape()

```
a = np.array([1,2,3,4,5,6])
print(a.reshape(2,3))

[[1,2,3],
[4,5,6]]
```

### 2.打平 numpy.flatten()

```
a = np.array([[1,2],[2,3]])
print(a.flatten())

[1,2,2,3]
```



## 8.合并与分裂

### 1.合并 concatenate

```
d1 = np.array([[1,1],[2,2]])
d2 = np.array([[2,2],[1,1]])
np.concatenate([d1,d2],axis = 0)   #axis 为0则竖直连接，为1则水平合并

[[1,1],
[2,2],
[2,2],
[1,1]]
```

### 2.分裂 split

```
a = np.arrange(16).reshape(4,4)
first,second,third = np.split(a,[1,3])

#第二个参数说明第一部分为[:1],第二部分为[1:3],第三部分为[3:]
```









