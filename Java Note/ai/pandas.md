> 视频网址：https://www.bilibili.com/video/BV1UJ411A7Fs
>
> 练习代码位置：D:\pycharm\code\study\pandas\

## 1、pandas

* 一个开源的python类库：用于数据分析、数据处理、数据可视化
* 高性能
* 易使用
* 方便与其他类库一起使用
  * numpy：用于数学计算
  * scikit-learn：用于机器学习



## 2、数据读取

> pandas需要先读取**表格类型**的数据，然后进行分析

| 数据类型                                         | 说明                            | Pandas读取方法    |
| ------------------------------------------------ | ------------------------------- | ----------------- |
| csv(用逗号分隔)、tsv(用tab分隔)、txt(任意分隔符) | 用逗号分隔、tab分隔的纯文本文件 | pandas.read_csv   |
| excel                                            | 微软xls或xlsx文件               | pandas.read_excel |
| mysql                                            | 关系型数据库                    | pandas.read_sql   |

### 2.1 读取纯文本文件

#### 2.1.1读取csv文件

```python
import pandas as pd

fpath = './ratings.csv'
# 使用pd.read_csv读取数据
ratings = pd.read_csv(fpath)

# 查看前几行数据
print(ratings.head())

# 查看数据的形状，返回(行数, 列数）
print(ratings.shape)

# 查看列名列表
print(ratings.columns)

# 查看索引列
print(ratings.index)

# 查看每列的数据类型
print(ratings.dtypes)
```

#### 2.1.2 读取txt文件

```python
import pandas as pd

fpath = "./access_pvuv.txt"
# 读取txt文件
# 关键字参数sep: separator 数据之间的分隔符
# 关键字参数header: 数据的标题列
# 关键字参数names: 指定数据的标题列
pvuv = pd.read_csv(fpath, sep='\t', header=None, names=['pdate', 'pv', 'uv'])
print(pvuv)
```

### 2.2 读取excel文件

注意：需要由`xlrd`模块

注意：新版`2.0.1`的`xlrd`模块只支持读取`xls`文件，不支持读取`xlsx`文件，需要下载`1.2.0`版本

```python
import pandas as pd
filepath = './access_pvuv.xlsx'
pvuv = pd.read_excel(filepath)
print(pvuv)
```

### 2.3 读取mysql数据库

```python
import pandas as pd
import pymysql
conn = pymysql.connect(
    host="127.0.0.1",
    user='root',
    password='123456',
    database='test',
    charset='utf8'
)
mysql_page = pd.read_sql('select * from hero', con=conn)  # sql语句，数据库连接
print(mysql_page)
conn.close()
```



## 3、Pandas—数据结构

`2、数据读取`中所有读取的数据的结果的类型是`DataFrame`

`DataFrame`是一个二维的数据，多行多列

	- df.index  行
	- df.columns  列

`Series`是一维数据，一行一列

### 3.1 Series

```
Series是一种类似于一维数组的对象，它由一组数据（不同数据类型）以及一组与之相关的数据标签（即索引）组成
```

1. 仅有数据列表即可产生最简单的Series

   ```python
   s1 = pd.Series([1, 'a', 5.2, 7])
   # 左侧是索引  右侧是列表数据
   print(s1)
   # 获取索引
   print(s1.index)
   # 获取数据
   print(s1.values)
   ```

2. 创建一个具有指定标签索引的Series

   ```python
   # 创建具有指定标签索引的Series
   s2 = pd.Series([1, 'a', 5.2, 7], index=['d', 'b', 'a', 'c'])
   print(s2)
   print(s2.index)
   ```

3. 使用Python字典创建Series

   ```python
   # 索引是key，数据是value
   sdata = {'Ohio':35000, 'Texas':72000, 'Oregon':16000, 'Utah':5000}
   s3 = pd.Series(sdata)
   print(s3)
   ```

4. 根据标签索引查询数据

   类似Python的dict

   ```python
   print(s2)
   print(s2['a'])
   print(type(s2['a']))
   print(s2[['b', 'a']])  # 参数是index的列表，会返回这些index的数据
   print(type(s2[['b', 'a']]))  # 类型仍旧是Series
   ```

### 3.2 DataFrame

```
DataFrame是一个表格型的数据结构
- 每列可以是不同的值类型
- 既有行索引index，也有列索引columns
- 可以被看作由Series组成的字典
```

创建`DataFrame`最常用的方法，见`2、数据读取`

1. 根据多个字典序列创建dataframe

   ```python
   data = {
       'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
       'year': [2000, 2001, 2002, 2001, 2002],
       'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
   }
   # 每一个字典作为一列（也可以看作是一个Series）
   # 字典的key是列索引
   df = pd.DataFrame(data)
   print(df)
   print(df.dtypes)
   # 第二列:year
   print(df.columns[1])
   print(df.index)
   ```

### 3.3 从DataFrame查询出Series

* 如果只查询一行或一列，返回pd.Series
* 如果查询多行、多列，返回pd.DataFrame

1. 查询一列，结果是pd.Series

   ```python
   print(df['year'])  # 查询year所在列, 结果的index是行索引
   print(type(df['year']))  # 类型是Series
   ```

2. 查询一行，结果是pd.Series

   ```python
   print(df.loc[0])  # 使用loc属性， 结果的index是列名
   print(type(df.loc[1]))  # 类型是Series
   ```

3. 查询多列，结果是pd.DataFrame

   ```python
   print(df[['year', 'pop']])  # 索引是一个含多个索引的列表时，返回多列
   print(type(df[['year', 'pop']]))  # 类型是DataFrame
   ```

4. 查询多行，结果是pd.DataFrame

   ```python
   print(df.loc[1:3])  # 与切片不同，pandas这里的切片索引包含尾部
   print(type(df.loc[1:3])  # 类型是DataFrame
   ```



## 4、Pandas查询数据

> Pandas 查询数据的几种方法
>
> 	1. df.loc方法，根据行、列的标签值查询
>  	2. df.iloc方法，根据行、列的数字位置查询
>  	3. df.where方法
>  	4. df.quert方法
>
> .loc既能查询，又能覆盖写入，强烈推荐

### 4.1 Pandas使用df.loc查询数据的方法

1. 使用单个label值查询数据
2. 使用值列表批量查询
3. 使用数值区间进行范围查询
4. 使用条件表达式查询
5. 调用函数查询

```
注意：
	- 以上方法，既适用于行，也适用于列
	- 注意观察降维  DataFrame > Series > 值
```

数据读取

​	数据为北京2018年全年天气预报

```python
filepath = './beijing_tianqi.csv'
# 读取csv文件
df = pd.read_csv(filepath)
# 打印数据的前几行
print(df.head())
# 设置索引为文件中的日期ymd，方便按照日期进行筛选
# 关键字参数inplace：当为True时，不生成新的DataFrame对象，修改原DataFrame
df.set_index('ymd', inplace=True)
print(df.head())
print('-' * 30)
# 替换掉最高温度和最低温度的后缀℃，并修改类型
# 注意：这里的str是DataFrame和Series的内置方法，里面重写了原python的str的方法，新的方法对行列操作的效率更高
df['bWendu'] = df['bWendu'].str.replace('℃', '').astype("int32")
df['yWendu'] = df['yWendu'].str.replace('℃', '').astype("int32")
print(df.dtypes)
```

### 4.2 单个Label值查询

```python
# 得到2018年1月3号的最高温度   单个值
print(df.loc['2018-01-03', 'bWendu'])
# 得到2018年1月3号的最高和最低温度  Series
print(df.loc['2018-01-03', ['bWendu', 'yWendu']])
```

### 4.3 使用值列表批量查询

```python
# 得到Series  2018/1/3,2018/1/4,2018/1/5三日的最高温度
print(df.loc[['2018-01-03', '2018-01-04', '2018-01-05'], 'bWendu'])
# 得到DataFrame  2018/1/3,2018/1/4,2018/1/5三日的最高温度和最低温度
print(df.loc[['2018-01-03', '2018-01-04', '2018-01-05'], ['bWendu', 'yWendu']])
```

### 4.4 使用数值区间进行范围查询

注意：与Python的切片索引不同，此处的区间既包含开始，也包含结束

```python
# 行index是区间
print(df.loc['2018-01-03':'2018-01-05', 'bWendu'])
# 列是区间
print(df.loc['2018-01-03', 'bWendu':'fengxiang'])
# 行和列都是区间
print(df.loc['2018-01-03':'2018-01-05', 'bWendu':'fengxiang'])
```

### 4.5 使用条件表达式进行查询

#### 4.5.1 简单查询

```python
# 查询最低温度低于-10度的列表（选择出bool表达式为True的行）
print(df.loc[df['yWendu'] < -10, :])
```

#### 4.5.2 复杂查询

```python
# 复杂查询，多个条件表达式用'&'连接，每一个条件需要加上()
# 查询最高温度小于等于30度，最低温度大于等于15度，并且是晴天，且天气为优
print(df.loc[(df['bWendu'] <= 30) & (df['yWendu'] >= 15) & (df['tianqi'] == '晴') & (df['aqiLevel'] == 1), :])
```

### 4.6 使用函数查询

符合函数式编程

```python
# 使用lambda表达式
print(df.loc[lambda row: (row['bWendu'] <= 30) & (row['yWendu'] >= 15), :])
# 编写自定义的函数
def query_data(df):
    # 2018年9月空气质量为优的天气
    return df.index.str.startswith('2018-09') & df['aqiLevel'] == 1


print(df.loc[query_data, :])
```



## 5、新增数据列

在进行数据分析时，经常需要按照一定条件创建新的数据列，然后进一步分析

新增数据列的几种方法

1. 直接赋值
2. `df.apply`方法
3. `df.assign`方法
4. 按条件选择分组分别赋值

### 5.1 直接赋值

```python
# 新增温差列
# 注意： 其实df['bWendu']是一个Series，两个Series相减得到的还是Series（对应位置元素相减）
df.loc[:, 'wencha'] = df['bWendu'] - df['yWendu']
print(df.head())
```

### 5.2 df.apply方法

> Apply a function along the axis of the DataFrame
>
> Objects passed to the function are Series objects whose index is either the DataFrame's index(axis = 0，将function应用到每一列) or the DataFrame's columns(axis = 1，将function应用到每一行)

添加一列温度类型，如果最高温度大于33是高温，低于-10是低温，否则是常温

```python
def get_temp_type(df):
    if df['bWendu'] > 33:
        return '高温'
    elif df['yWendu'] < -10:
        return '低温'
    else:
        return '常温'

# 将df的每一列传入传入get_temp_type
df.loc[:, 'wendu_type'] = df.apply(get_temp_type, axis=1)
# value_counts()对wendu_type的值进行计数
print(df['wendu_type'].value_counts())
```

### 5.3 df.assign方法

> Assign new columns to a DataFrame
>
> Returns a new object with all original columns in addition to new ones
>
> 即不会修改原对象，而是将修改后的作为新对象返回

```python
# 将摄氏度转换为华氏度
# assign可以同时新增多个列
new_df = df.assign(
    # 关键字参数作为新增的列名
    bWendu_huashi = lambda row:row['bWendu'] * 1.8 + 32,
    yWendu_huashi = lambda row:row['yWendu'] * 1.8 + 32
)
```

### 5.4 按条件选择分组分别赋值

按条件先选择数据，然后对这部分数据赋值新列



## 6、数据统计函数

1. 汇总类统计
2. 唯一去重和按值计数
3. 相关系数和协方差

### 6.1 汇总类统计

```python
# 1. 汇总类统计
# describe 提取所有数字列的统计结果(计数，平均值，标准差，最大值，最小值。。。)
print(df.describe())
# 查看单个Series的数据
# 平均值
print(df['bWendu'].mean())
# 最大值
print(df['bWendu'].max())
# 最小值
print(df['bWendu'].min())
```

### 6.2 唯一去重

unique列出某一列不重复的元素

```python
print(df['fengxiang'].unique())
print(df['tianqi'].unique())
print(df['aqiLevel'].unique())
```

### 6.3 按值计数

value_counts()统计所有不重复的元素的出现次数

```python
print(df['fengxiang'].value_counts())
print(df['tianqi'].value_counts())
```

### 6.4 相关系数和协方差

1. 协方差
   * 衡量同向反向程度
   * 如果协方差为正，说明X，Y同向变化，协方差越大说明同向程度越高
   * 如果协方差为负，说明X，Y反向运动，协方差越小说明反向程度越高
2. 相关系数
   * 衡量相似度程度
   * 当他们的相关系数为1时，说明两个变量变化时的正向相似度最大，当相关系数为-1时，说明两个变量变化的反向相似度最大

```python
# 协方差矩阵
print(df.cov())
# 相关系数矩阵
print(df.corr())
# 单独查看两个列的相关性
print(df['aqi'].corr(df['bWendu']))  # 0.08
print(df['aqi'].corr(df['yWendu']))  # 0.027
print(df['aqi'].corr(df['bWendu'] - df['yWendu']))  # 0.216  和温差相关性更大
```



## 7、缺失值处理

* isnull和notnull
  * 检测是否是空值，可用于DataFrame和Series
* dropna
  * 丢弃、删除缺失值
  * axis：删除行（0 or 'index'}或列{1 or 'columns'}，默认值为0
  * how：值为any则有一个值为空就满足条件，值为all则所有值为空才满足条件
  * inplace：如果为True则修改当前DataFrame，否则返回新的DataFrame
* fillna
  * 填充空值
  * value：用于填充的值，可以是单个值，或者字典（key是列名，value是值）
  * method：
    * 值为ffill使用前一个不为空的值填充（forward fill）
    * 值为bfill使用后一个不为空的值填充（backword fill）
  * axis：按行{0, 'index'}或列{1, 'columns'}
  * inplace：如果为True返回当前修改的DataFrame，否则返回新的DataFrame

```python
import pandas as pd
filepath = './student_excel.xlsx'
# 读取数据
# 使用skiprows关键字跳过前面两行空行
df = pd.read_excel(filepath, skiprows=2)
print(df)
print('-' * 30)

# 1. 检测空值
print(df.isnull())  # 返回每个位置上是否是空
print(df['分数'].isnull())  # 返回分数列是否为空
print(df['分数'].notnull())  # 返回分数列是否非空
# 筛选出分数不为空的行
print(df.loc[df['分数'].notnull(), :])
print('-' * 30)

# 2. 删除全是空值的列
df.dropna(axis=1, how='all', inplace=True)
print(df)
print('-' * 30)

# 3. 删除全是空值的行
df.dropna(axis=0, how='all', inplace=True)
print(df)
print('-' * 30)

# 4. 将分数为空的填充为0分
df.loc[:, '分数'] = df['分数'].fillna(0)
# 或者 df.fillna({'分数':0})
print(df)

# 5. 将姓名的缺失值进行填充
df.loc[:, '姓名'] = df['姓名'].fillna(method='ffill')
print(df)

# 6. 将清洗好的excel进行保存
# 关键字index为False，表明不需要添加索引
df.to_excel('./student_clean.xlsx', index=False)
```



## 8、SettingWithCopyWarning

```python
import pandas as pd
# 读取数据
filepath = './beijing_tianqi.csv'
df = pd.read_csv(filepath)
df.loc[:, 'bWendu'] = df['bWendu'].str.replace('℃', '').astype('int32')
df.loc[:, 'yWendu'] = df['yWendu'].str.replace('℃', '').astype('int32')
print(df.head(3))

# 1. 复现SettingWithCopyWarning
# 选出三月份的数据
condition = df['ymd'].str.startswith('2018-03')
# 设置温差
df[condition]['wencha'] = df['bWendu'] - df['yWendu']
print(df[condition].head())  # 修改失败
```

```
发出警告的代码：df[condition]['wencha'] = df['bWendu'] - df['yWendu']
原因：df['condition']首先是获取 然后再设置其上的温差
但是：这里的df['condition']可能是copy或者是subview
```

*  **核心要诀**：pandas的dataframe修改写操作，只允许在源dataframe上一步到位

* 解决方法1

  ```python
  df.loc[condition, 'wencha'] = df['bWendu'] - df['yWendu']
  print(df[condition])
  ```

* 解决方法2

  ```python
  # 先得到三月数据的copy
  df_month3 = df[condition].copy()
  df_month3['wencha'] = df_month3['bWendu'] - df_month3['yWendu']
  print(df[condition])
  ```

总结：pandas不允许先筛选出dataframe，再进行修改写入

* 使用loc实现一步修改
* 先复制sub dataframe再修改



## 9、数据排序

* Series的排序
  * `Series.sort_values(ascending=True, inplace=False)`
  * ascending：默认为True升序排序，False是降序排序
  * inplace：是否修改原始Series
* DataFrame的排序
  * `DataFrame.sort_values(by, ascending=True, inplace=False)`
  * by：字符串或者List<字符串>，单列排序还是多列排序
  * ascending：bool或者List<布尔>，升序还是排序，如果是List对应by的多列

```python
import pandas as pd
# 0. 读取数据
filepath = '../beijing_tianqi.csv'
df = pd.read_csv(filepath)
df.loc[:, 'bWendu'] = df['bWendu'].str.replace('℃', '').astype('int32')
df.loc[:, 'yWendu'] = df['yWendu'].str.replace('℃', '').astype('int32')
print(df.head())

# 1. Series的排序
print(df['aqi'].sort_values())  # 显示排序后的Series
print(df['aqi'].sort_values(ascending=False))
print(df['tianqi'].sort_values())  # 对中文进行排序
# 2. DataFrame的排序
# 2.1 单列排序
print(df.sort_values(by='aqi'))
print(df.sort_values(by='aqi', ascending=False))
# 2.2 多列排序
print(df.sort_values(by=['aqi', 'bWendu']))  # 按空气质量，最高温度排序，默认是升序
print(df.sort_values(by=['aqi', 'bWendu'], ascending=False))  # 降序排序
print(df.sort_values(by=['aqi', 'bWendu'], ascending=[True, False]))  # 空气质量升序，最高温度降序
```



## 10、字符串处理

1. 使用方法：先获取Series的str属性，然后在属性上调用函数
2. 只能在字符串列上使用，不能再数字列上使用
3. DataFrame上没有str属性或方法
4. Series.str并不是Python原生字符串，而是自己的一套方法，不过大部分和原生str类似



## 11、理解axis参数

* axis=0或'index'
  * 如果是单行操作，就指的是某一行
  * 如果是聚合操作，指的是跨行对列进行操作
* axis=1或'columns'
  * 如果是单列操作，指的是某一列
  * 如果是聚合操作，指的是跨列对行进行操作

## 12、index的用途

把数据存储于普通的column列也能用于数据查询，那使用index有什么好处呢？

index用途总结：

1. 更方便的数据查询

2. 使用index可以获得性能提升

   ```
   如果index是唯一的，pandas会使用哈希表优化，查询性能为O(1)
   如果index不是唯一的，但是有序，pandas会使用二分查找法，查询性能为O(logN)
   如果index是完全随机的，那么每次查询都要扫描全表，查询性能为O(N)
   ```

3. 自动的数据对齐功能

   * 包括Series和DataFrame

4. 更多更强大的数据结构支持

   * 很多强大的索引数据结构
   * CategoricalIndex，基于分类数据的Index，提升性能
   * multiIndex，多维索引，用于groupby多维聚合后结果等
   * DatetimeIndex，时间类型索引，强大的日期和时间的方法支持

```python
import pandas as pd
df = pd.read_csv('../ratings.csv')
print(df.head())
print(df.count())

# 1. 使用index查询数据
# inplace=True会把原来的userId列删除，使用drop=False保留该列
df.set_index('userId', inplace=True, drop=False)
print(df.head())
print(df.index)
# 使用index的查询方法，查询userId为500的前五行数据
print(df.loc[500].head(5))
# 还有一种比较笨的方法
print(df.loc[df['userId'] == 500].head())
print('-' * 30)

# 2. 使用index可以获得性能提升
# 实验一，完全随机的顺序查询
from sklearn.utils import shuffle
df_shuffle = shuffle(df)
print(df_shuffle.head())
# 判断索引是否递增
print(df_shuffle.index.is_monotonic_increasing)  # False
# 判断索引是否唯一
print(df_shuffle.index.is_unique)  # False
print('-' * 30)

# 实验二，将index排序后的查询
df_sorted = df_shuffle.sort_index()  # 索引排好序之后的df
print(df_sorted.head())
print(df_sorted.index.is_monotonic_increasing)  # True
print(df_sorted.index.is_unique)  # False
print('-' * 30)

# 3. 使用index的自动对齐功能
s1 = pd.Series([1, 2, 3], index=list('abc'))
print(s1)
s2 = pd.Series([2, 3, 4], index=list('bcd'))
print(s2)
print(s1 + s2)  # 相同的索引元素相加，其他为NaN
```



## 13、pandas的merge语法

> Pandas的Merge，相当于SQL的join，将不同的表按照key关联到一个表

merge的语法：

`pd.merge(left, right, how='inner', on=None, lef_on=None, right_on=None, left_index=False, right_index=False, sort=True, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)`

* left，right：要merge的DataFrame或者有name的Series
* how：join类型，'left', 'right', 'outer', 'inner'
* on：同时指定两个表join时的key，left和right都需要有这个key
* left_on：left的df或者series的key
* right_on：right的df或者series的key
* left_index，right_index：使用index而不是普通的column进行join
* suffixes：两个元素的后缀，如果列有重名，自动添加后缀，默认是`('_x', '_y')`

```python
import pandas as pd

# 读取数据
# sep是数据的分隔符（由于两个冒号在python中表示正则表达式，所以需要指定engine为python）
df_ratings = pd.read_csv("../ratings.dat", sep="::", engine="python",
                         names="UserID::Movie::Rating::Timestamp".split("::"))
print(df_ratings.head())
df_users = pd.read_csv("../users.dat", sep="::", engine="python",
                       names="UserID::Gender::Age::Occupation::Zip-code".split("::"))
print(df_users.head())
df_movies = pd.read_csv("../movies.dat", sep="::", engine='python',
                        names="MovieID::Title::Genres".split("::"))
print(df_movies.head())
print('-' * 30)

# 1. ratings和users的join
df_ratings_users = pd.merge(df_users, df_ratings, left_on='UserID', right_on='UserID', how='inner')
print(df_ratings_users.head())
print('-' * 30)

# 2. merge时的数据关系
# 2.1 一对一
left = pd.DataFrame({'sno': [11, 12, 13, 14], 'name': ['name_a', 'name_b', 'name_c', 'name_d']})
right = pd.DataFrame({'sno': [11, 12, 13, 14], 'age': ['21', '22', '23', '24']})
res = pd.merge(left, right, on='sno')
print(res)
print('-' * 30)

# 2.2 一对多
left = pd.DataFrame({'sno': [11, 12, 13, 14], 'name': ['name_a', 'name_b', 'name_c', 'name_d']})
right = pd.DataFrame({'sno': [11, 11, 11, 12, 12, 13], 'grade': ['语文88', '数学90', '英语75', '语文66', '数学55', '英语29']})
res = pd.merge(left, right, on='sno')
print(res)
print('-' * 30)

# 2.3 多对多
left = pd.DataFrame({'sno': [11, 11, 12, 12, 12], '爱好': ['篮球', '羽毛球', '乒乓球', '篮球', '足球']})
right = pd.DataFrame({'sno': [11, 11, 11, 12, 12, 13], 'grade': ['语文88', '数学90', '英语75', '语文66', '数学55', '英语29']})
res = pd.merge(left, right, on='sno')
print(res)
print('-' * 30)

# 3. 理解join的连接
# 3.1 inner join 默认
# 两方数据的交集
left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'], 'A': ['A0', 'A1', 'A2', 'A3'], 'B': ['B0', 'B1', 'B2', 'B3']})
right = pd.DataFrame({'key': ['K0', 'K1', 'K4', 'K5'], 'C': ['C0', 'C1', 'C4', 'C5'], 'D': ['D0', 'D1', 'D4', 'D5']})
res = pd.merge(left, right, on='key', how='inner')
print(res)
print('-' * 30)

# 3.2 left join
# 两方数据的交集与左边数据的并集
res = pd.merge(left, right, on='key', how='left')
print(res)
print('-' * 30)

# 3.3 right join
res = pd.merge(left, right, on='key', how='right')
print(res)
print()

# 3.4 outer join
# 两边数据的并集
res = pd.merge(left, right, on='key', how='outer')
print(res)
print('-' * 30)

# 4. merge的数据出现重名字段
left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'], 'A': ['A0', 'A1', 'A2', 'A3'], 'B': ['B0', 'B1', 'B2', 'B3']})
right = pd.DataFrame({'key': ['K0', 'K1', 'K4', 'K5'], 'A': ['A10', 'A11', 'A12', 'A13'], 'D': ['D0', 'D1', 'D2', 'D3']})
res = pd.merge(left, right, on='key')  # 一个是_x后缀，一个是_y后缀
print(res)
# 通过suffixes参数自己指定后缀
res = pd.merge(left, right, on='key', suffixes=('_left', '_right'))
print(res)
```

## 14、Pandas实现数据合并concat

>  使用场景：批量合并相同格式的Excel、给DataFrame添加行或列
>
> * 使用某种方式合并（inner/outer）
> * 沿着某个轴向（axis=0/1）
> * 把多个pandas对象（DataFrame或Series）合并成一个

**concat语法：**

`pandas.concat(objects, axis=0, join='outer', ignore_index=False)`

* objects: 一个列表，内容可以是DataFrame或者Series，可以混合
* axis: 默认是0，按行合并，如果等于1，表示按列合并
* join: 合并的时候索引对齐的方式，默认是outer join，也可以是inner join
* ignore_index: 是否忽略掉原来的数据索引

```python
import pandas as pd

df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3'],
                    'E': ['E0', 'E1', 'E2', 'E3']})
df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7'],
                    'F': ['F4', 'F5', 'F6', 'F7']})
# 使用默认的参数, axis=0, ignore_index=False, join=outer
print(pd.concat([df1, df2]))
# 忽略之前的索引
print(pd.concat([df1, df2], ignore_index=True))
# 过滤不匹配的列
print(pd.concat([df1, df2], ignore_index=True, join='inner'))
# 按列添加
s1 = pd.Series(list(range(4)), name='F')
print(pd.concat([df1, s1], axis=1))
# 添加多列
s2 = df1.apply(lambda x: x['A'] + '_GG', axis=1)
s2.name = 'G' # 指定索引列为G
print(pd.concat([s1, df1, s2], axis=1))
```

**append语法：**

`DataFrame.append(other, ignore_index=False)`

* other: 单个DataFrame，Series，dict，或者列表
* ignore_index: 是否忽略掉原来的数据索引

```python
import pandas as pd
df1 = pd.DataFrame([[1, 2], [3, 4]], columns=list('AB'))
df2 = pd.DataFrame([[5, 6], [7, 8]], columns=list('AB'))
print(df1.append(df2))
# 忽略原来索引
print(df1.append(df2, ignore_index=True))
```

concat与append有性能差距（append每次操作之后返回新的DataFrame）

```python
# 给DataFrame一行一行添加数据时
df = pd.DataFrame(columns=['A'])
# 低性能版本
for i in range(5):
    # 调用了append i 次
    df = df.append({'A': i}, ignore_index=True)
print(df)

# 高性能版本
# 调用了concat一次
df = pd.concat([pd.DataFrame({'A': i} for i in range(5))], ignore_index=True)
print(df)
```

## 15、批量合并和拆分

拆分

```python
import pandas as pd
import os

# 将一个大Excel拆分成多个小Excel
work_dir = '.'
split_dir = f'{work_dir}/split'
if not os.path.exists(split_dir):
    os.mkdir(split_dir)

df_source = pd.read_excel(f'{work_dir}/workload_source.xlsx')
total_row = df_source.shape[0]

user_names = ['蔡徐坤', '易建联', '吴亦凡', 'XIAOHU', '胡凯莉', '大司马']
# 每个人的任务数量
split_size = total_row // len(user_names)
if total_row % len(user_names) != 0:
    split_size += 1

df_subs = []
for idx, user_name in enumerate(user_names):
    begin = idx * split_size
    end = begin + split_size
    df_sub = df_source.iloc[begin:end]
    df_subs.append((idx, user_name, df_sub))
for idx, user_name, df_sub in df_subs:
    file_name = f"{split_dir}/{idx}_{user_name}.xlsx"
    df_sub.to_excel(file_name, index=False)
    print(idx)
```

合并

```python
import pandas as pd
import os

# 将多个小Excel合并为一个大Excel，并标记其来源
# 1. 遍历文件夹，得到要合并的Excel文件列表
# 2. 分别读取成DataFrame，给每一个df增加标记来源
# 3. 使用pandas.concat进行批量合并
# 4. 将合并后的df输出成Excel

work_dir = '.'
split_dir = f'{work_dir}/split'
excel_names = []
for excel_name in os.listdir(split_dir):
    excel_names.append(excel_name)
print(excel_names)

df_xlsxs = []
for excel_name in excel_names:
    df = pd.read_excel(f'{split_dir}/{excel_name}')
    user_name = excel_name.replace(".xlsx", '')[2:]
    print(user_name)
    df.loc[:, 'Name'] = user_name
    df_xlsxs.append(df)

merge_xlsx = pd.concat(df_xlsxs, ignore_index=True)
merge_xlsx.to_excel(f'{work_dir}/workload_merge.xlsx', index=False)
print('over')
```

### 16、

