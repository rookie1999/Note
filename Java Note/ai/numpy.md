>视频地址：https://www.bilibili.com/video/BV14K4y1v7xP?p=2&spm_id_from=pageDriver

# NumPy

* Numpy全称为`Numerical Python`，是一个开源的Python数据分析和科学计算库
* Numpy是我们后期学习Pandas（数据分析）、Scipy（科学计算）和Matplotlib（绘图库）的基础



## 1、Numpy的特点

* NumPy底层使用C语言实现的，速度快
* NumPy提供数据结构（即数组）比Python内置数据结构的访问效率更高
* 支持大量高维度数组与矩阵运算
* 提供大量的数学函数库



## 2、数组对象

> 数组对象特点：
>
> 	- 用于存放同类型元素的集合
> 	- 每个元素在内存中都有相同存储大小的区域

### 2.1 创建一维数组

* `numpy.array(object)`
  * object只能是元组或列表
  * 类型是`numpy.ndarray`
* `numpy.arange(start, stop, step, dtype)`
  * start：开始值，默认值为0，包含开始值
  * stop：结束值，不包含结束值
  * step：步长，默认值为1，该值可以为负数
  * dtype：数组元素类型
* `numpy.linspace(start, stop, num, endpoint=True, retstep=False, dtype)`
  * 创建等差数组
  * num：设置生成的元素个数
  * endpoint：设置是否包含结束值，False是不包含，True是包含，默认值是True
  * retstep：设置是否返回步长（即公差），False是不返回，默认是False，True是返回，当为True是，返回值是一个二元组，包含数组和步长
* `numpy.logspace(start, stop, num, endpoint, base, dtype)`
  * start：开始值，值为base**start
  * stop：结束值，值为base**stop
  * base：底数

### 2.2 数组的数据类型

* 使用dtype属性查看数组的数据类型

* 指定数组的数据类型

  * 使用数据类型

    * ```python
      arr = numpy.array([1, 2, 3], dtype=numpy.int64)
      ```

  * 使用类型代码

    * ```python
      arr = numpy.array([1, 2, 3], dtype='i8')
      ```

int类型

| 数据类型 | 类型代码 | 描述                                     |
| -------- | -------- | ---------------------------------------- |
| int_     |          | 默认的整数类型，是int32或int64           |
| intc     |          | 与C语言的int类型一样，一般是int32或int64 |
| int8     | i1       | 字节（-128 to 127）                      |
| int16    | i2       | 有符号的两个字节                         |
| int32    | i4       | 有符号的四个字节                         |
| int64    | i8       | 有符号的八个字节                         |
| uint8    | u1       | 无符号的字节（0 to 255）                 |
| uint16   | u2       | 无符号的两个字节                         |
| uint32   | u4       | 无符号的四个字节                         |
| uint64   | u8       | 无符号的八个字节                         |

float类型

| 数据类型 | 类型代码 | 描述              |
| -------- | -------- | ----------------- |
| float_   |          | float64类型的简写 |
| float16  | f2       | 半精度浮点数      |
| float32  | f4或f    | 单精度浮点数      |
| float64  | f8或d    | 双精度浮点数      |

complex类型

| 数据类型   | 类型代码 | 描述                                         |
| ---------- | -------- | -------------------------------------------- |
| complex_   |          | complex128的简写                             |
| complex64  | c8       | 复数，表示双32位浮点数（实数部分和虚数部分） |
| complex128 | c16      | 复数，表示双64位浮点数                       |

string类型

| 数据类型 | 类型代码 | 描述                                                |
| -------- | -------- | --------------------------------------------------- |
| string_  | S        | ASCII字符串，例如S7表示长度为7的字符串              |
| unicode_ | U        | Unicode字符串，例如U5表示长度为5的Unicode编码字符串 |

bool类型

| 数据类型 | 类型代码 | 描述                  |
| -------- | -------- | --------------------- |
| bool_    |          | 布尔型，即True或False |

### 2.3 创建二维数组

* `numpy.array(object)`

  * object只能是元组或列表
  * 类型是`numpy.ndarray`
  * `arr = numpy.array([1, 2, 3], [4, 5, 6])`

* `numpy.ones(shape,dtype)`

  * 根据指定的形状和数据生成全为1的数组

  * shape：是数组的形状，类型可以是列表或元组

    ```python
    import numpy as np
    # 生成一个2行3列的数组，元素全为1
    arr = np.ones([2, 3])
    ```

* `numpy.zeros(shape, dtype)`

  * 根据指定的形状和数据生成全为0的数组

* `numpy.full(shape, fill_value, dtype)`

  * 根据指定的形状和数据类型生成数组，并用指定数据填充
  * fill_value：指定填充的数据

* `numpy.identity(n, dtype)`

  * 创建n阶单位矩阵

### 2.4 数组的轴

对于二维数组来说，不同的行是0轴，不同的列是1轴

### 2.5 数组的属性

* dtype
  * 数组的类型
* shape
  * 数组的形状（几行几列）
* ndim
  * 维度
* size
  * 数组的个数

### 2.6 数组索引访问

* 一维数组索引访问与Python内置序列类型索引访问一样

  * 正序索引从0开始
  * 逆序索引从1开始

* 二维数组索引访问

  * `ndarray([所在0轴索引][所在1轴索引])`

  * `ndarray[所在0轴索引，所在1轴索引]`

    ```python
    import numpy
    arr = numpy.array([[1, 2, 3],
                      [4, 5, 6]])
    print(arr[1, 2])  #6
    print(arr[1][2])  #6
    ```

### 2.7 切片访问

* 一维数组切片访问与Python内置序列类型切片访问一样
  * `ndarray[start:end]`
  * `ndarray[start:end:step]`
  * 注意：切片包括start位置，但不包括end位置元素
* 二维数组切片访问
  * `ndarray[所在0轴切片，所在1轴切片]`
  * 如果两个索引处都是切片，则结果是二维数组
  * 如果有一个索引是标量，则结果是一维数组

### 2.8 布尔索引

我们可以传递布尔索引，从数组中过滤出需要的元素

注意1：布尔索引必须要与索引的数组形状相同，否则会引发IndexError错误

注意2：布尔索引返回的新数组是原数组的副本，与原数组不共享相同的数据空间，即新数组的修改不会影响原数组，这是所谓的***深层复制(深拷贝)***

```python
# 一维数组的布尔索引
arr1 = numpy.array([1, 2, 3])
arr2 = numpy.array([True, False, True])
print(arr1[arr2])  # [1, 3]
```

注意3：布尔索引的结果只能是一维数组

### 2.9 花式索引

* 索引为整数列表
* 索引为一维整数数组
* 索引为二维整数数组
* 注意：
  1. 花式索引返回的新数组与花式索引数组形状相同
  2. 花式索引返回的新数组与布尔索引类似，属于`深层复制（深拷贝）`
  3. 二维数组上每一个轴的索引数组形状相同

```python
# 一维数组的花式索引
a1 = numpy.array([1, 3, 5, 7, 9])
b = [1, 3]
print(a1[b])  # a1[b] = [a1[1], a1[3]] = [3, 7]

c = numpy.array([[1, 2],
                [3, 4]])
# c = [[a1[1], a1[2]],
# 		[a1[3], a1[4]]]
```

```python
# 二维数组的花式索引
a2 = numpy.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])
m = [1,2]
n = [0,1]
print(a2[m,n])  # a2[m,n] = [a2[1,0], a2[2,1]] = [4, 8]
# 注意a2[m][n]这种形式：先计算出a2[m]得到第二行和第三行，中间结果假设为res，再计算res[n]得到第一行和第二行，最后结果是a2的第二行和第三行

m = numpy.array([[1, 2],
                [2, 0]])
n = numpy.array([[1, 0],
                [2, 2]])
print(a2[m, n])
# a2[m,n] = [
#	 [a2[1,1],a2[2,0]],
#    [a2[2,2], a2[0,2]]
# ]

# 利用数组的广播
n = numpy.array([1, 0])
print(a2[m, n])
# a2[m,n] = [
#	 [a2[1,1],a2[2,0]],
#    [a2[2,1], a2[0,0]]
# ]
```



## 3、数组运算

### 3.1 连接数组

* `numpy.concatenate((a1, a2, ...), axis=0)`
  * 第一个参数是元组，元组的元素是需要连接的数组
  * 注意：除了指定轴外，其他轴的元素个数必须相同
  * axis是沿指定轴的索引，默认为0轴
* `numpy.vstack((a1, a2))`
  * 沿垂直堆叠多个数组，相当于concatenate()函数axis=0的情况
  * 注意：1轴元素个数相同
* `numpy.hstack((a1, a2))`
  * 沿水平堆叠多个数组，相当于concatenate()函数axis=1情况
  * 注意：0轴上元素个数相同
* `numpy.tile(v, (2, 1))`
  * 将v在行上变成原来的两份，在列上变成原来的一份

### 3.2 分割数组

* `numpy.split(array, indices_or_sections, axis=0)`
  * 沿指定的轴分割多个数组
  * arr：要被分割的数组
  * indices_or_sections：是一个整数或数组
    * 如果是整数，则平均分割该数组，每个数组的长度为该值(注意：该整数必须是原来数组长度的因数)
    * 如果是数组，则为沿指定轴的切片操作（切出来的部分为一个子数组，切片的前后分别为两个数组）
    * axis指轴的分割方向，默认为0轴
* `numpy.vsplit(array, indices_or_sections)`
  * 沿垂直方向分割数组，相当于split()函数axis=0情况
* `numpy.hsplit(array, indices_or_sections)`
  * 沿水平方向分割数组，相当于split()函数axis=1情况

### 3.3 算术运算

算术运算就是对应位置的元素进行运算

### 3.4 数组的广播

> 数组与标量或者不同形状的数组进行算术运算的时候，就会进行数组的广播

* 数组与标量
  * 数组与标量进行算术运算，相当于先将标量广播成相同形状的数组，然后再进行算术运算
* 数组与数组
  * 数组与不同形状的数组进行算术运算时，会发生广播，须遵守以下广播原则
    * 先比较形状，再比较维度，最后比较轴长度
    * 如果两个数组维度不相等，会在维度较低数组的形状左侧填充1，直到维度与高维数组相同
    * 如果两个数组的维度相等时，要么轴长度相同，要么轴长度为1，则兼容的数组可以广播，长度为1的轴会被扩展

## 4 、数组的常用函数

### 4.1 随机数函数

* `numpy.random.seed(num)`
  
* 将num指定为随机数的种子
  
* `numpy.random.rand(d0, d1, d2, ..., dn)`

  * 返回[0.0, 1.0)的随机浮点数，即大于等于0.0，且小于1.0的随机浮点数
  * d0，d1，...表示数组的形状

* `numpy.random.randomint(low, high, size, dtype)`

  * 返回[low, high)的随机整数，如果high省略，则返回[0,low)的随机整数，并且此时size是关键字参数

  * size表示数组的形状

  * ```python
    a1 = numpy.random.randint(3, 7, (5,))
    ```

* `numpy.random.normal(loc, scale, size)`

  * 返回正态分布随机数
  * loc：平均值
  * scale：标准差

* `numpy.random.randn(d0, d1, ...)`

  * 返回标准正态分布随机数，平均数为0，标准差为1的正态分布随机数

### 4.2 排序函数

* `numpy.sort(a, axis=-1, kind='quicksort', order=None)`	
  * 按照轴对数组进行排序，即轴排序	
  * a：要排序的数组
  * axis：排序的轴索引，默认是-1，即最后一个轴（二维是1轴）
  * kind表示排序类型
    * quicksort：快速排序，默认值
    * mergesort：归并排序
    * heapsort：堆排序
  * order：排序字段
* `numpy.sort(a, axis=-1, kind='quicksort', order=None)`
  * 按照轴对数组进行排序索引，即轴排序索引（实质是按轴排序之后，将元素换为该轴的索引）
* `numpy.random.shuffle(a)`
  * 将a乱序处理

### 4.3 聚合函数

* 求和
  * 使用NumPy中sum()函数
    * `numpy.sum(a, axis=None)`
    * 如果不设置axis，默认求所有的元素的和
    * 注意：任何数和`numpy.nan`计算都是NaN
  * 使用NumPy中nansum()函数，该函数会忽略NaN
    * `numpy.nansum(a, axis=None)`
  * 使用数组对象的sum()方法
    * `numpy.ndarray.sum(axis=None)`
* 求最大值
  * 使用NumPy中amax()函数
    * `numpy.amax(a, axis=None)`
  * 使用NumPy中nanmax()函数，该函数会忽略NaN
    * `numpy.nanmax(a, axis=None)`
  * 使用数组对象的max()方法
    * `numpy.ndarray.max(axis=None)`
* 求最小值
  * 使用NumPy中amin()函数
    * `numpy.amin(a, axis=None)`
  * 使用NumPy中nanmin()函数，该函数会忽略NaN
    * `numpy.nanmin(a, axis=None)`
  * 使用数组对象的min()方法
    * `numpy.ndarray.min(axis=None)`
* 求平均值
  * 使用NumPy中mean()函数
    * `numpy.mean(a, axis=None)`
  * 使用NumPy中nanmean()函数，该函数会忽略NaN
    * `numpy.nanmean(a, axis=None)`
  * 使用数组对象的mean()方法
    * `numpy.ndarray.mean(axis=None)`
* 求加权平均值
  * 使用NumPy中average()函数
    * `numpy.average(a, axis=None, weights=None)`
  * weights：权重，形状与数组a相同，且元素和为1
* 求中位数
  * `numpy.median(X)`
* 求百分位点
  * `numpy.percentile(arr, q=40)`
    * 返回arr中由小到大第40%个数
* 求方差
  * `numpy.var(arr)`
* 求标准差
  * `numpy.std(arr)`
* `numpy.all(x == 0)`
  * 是否x的所有元素都等于0
* `numpy.any(x == 0)`
  * 是否x有非零元素

### 4.4 索引运算

* 求最小值的索引
  * `numpy.argmin(arr)`
* 求排序后的索引位置
  * `numpy.argsort(arr)`

### 4.5 矩阵相关函数

* `A.dot(B)`
  * 矩阵A点乘矩阵B
* `npumpy.linalg.inv(A)`
  * 求矩阵A的逆矩阵
  * A必须是方阵
* `numpy.linalg.pinv(A)`
  * 求矩阵A的伪逆矩阵
  * 对A的形状没有要求
  * 矩阵A和伪逆矩阵点乘结果也是单位矩阵
* `numpy.transpose(A)`
  * 返回A的转置矩阵
  * 也可以使用`A.T`



## 5、数组的保存和读取

### 5.1 数组的保存

* `numpy.save(file, arr, allow_pickle=True, fix_imports=True)`
  * 将一个数组保存至后缀名为“.npy”的二进制文件
  * file：文件名/文件路径
  * arr：要存储的数组
  * allow_pickle：布尔值，是否允许使用pickle来保存数组对象
  * fix_imports：布尔值，是否允许在Python2中读取Python3保存的数据

* `numpy.savez(file)`

  * 将多个数组保存到未压缩的后缀名为".npz"的二进制文件中

    ```python
    a1 = numpy.arange(10)
    a2 = numpy.random.rand(0, 10, (3, 3))
    np.savez("array_savez", array_1=a1, array_2=a2)
    ```

* `numpy.savez_compressed(file)`

  * 将多个数组保存到压缩的后缀名为".npz"的二进制文件中

### 5.2 数组的读取

* `numpy.load(file, mmap_mode, allow_pickle, fix_imports, encoding)`

  * mmap_mode：表示内存的映射模式，即在读取较大NumPy数组时的模式，默认情况是None

    ```python
    arr = numpy.load('array_savez')  # arr是NpzFile类型的
    a1 = arr['array_1']
    a2 = arr['array_2']
    ```

    