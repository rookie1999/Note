> 视频网址:https://www.bilibili.com/video/BV1Kk4y1U7UV

# Matplotlib

## 第一章 数据分析绪论

数据分析是指用适当的统计分析方法对收集来的大量数据进行分析，提取有用信息和形成结论而对数据加以详细研究和概括总结的过程

1. 收集数据
2. 提取信息
3. 形成结论

### 1.1 数据分析的可视化流程

1. 定义分析目标
2. 数据采集及预处理
3. 数据分析挖掘
4. 数据可视化

数据清洗

​	是指发现并纠正数据文件中可识别的错误的最后一道程序，包括检查数据一致性，处理无效值和缺失值等

### 1.2 常见的可视化形式

* 可视化形式

  * 统计图（直方图，折线图，饼图）
  * 分布图（热力图，散点图，气泡图）

* 常用工具

  * 分析工具
    * pandas
    * SciPy
    * numpy
    * sklearn
  * 绘图工具
    * matplotlib
    * Pychart
    * reportlab
  * 平台工具
    * Jupyter Notebook
    * PyCharm

  ```
  Matplotlib是Python的绘图库，它可与NumPy一起使用，提供了一种有效的Matlab开源替代方案，也可以和图形工具包一起使用，如PyQt和wxPython
  ```

### 1.3 配置matplotlib

Matplotlib是一个Python的2D绘图库，它以各种硬拷贝格式和跨平台的交互式环境生成出版质量级别的图形

1. 启动conda环境

2. 安装matplotlib

   ```
   conda install matplotlib
   ```

3. 使用matplotlib

   ```python
   >>> import matplotlib
   >>> import matplotlib.pyplot as plt   # 绘图库
   >>> x=[1,2]
   >>> y=[3,4]
   >>> plt.bar(x,y)
   <BarContainer object of 2 artists>
   >>> plt.show()
   ```

   如果默认不显示图片（如jupyter），需要使用

   ```
   %matplotlib inline
   ```



## 第二章 matplotlib的使用

### 2.1 matplotlib的基本配置

* `matplotlib.rcParams`

  * 是matplotlib存放设置的字典，修改字典键值以改变matplotlib绘图的相关设置

  ```python
  import matplotlib.pyplot as plt
  plt.rcParams['font.sans-serif'] = ['SimHei']  # 支持中文
  plt.rcParams['axes.unicode_minus'] = False	  # 正常显示负号
  
  plt.rcParams['lines.linewidth'] = 5			   # 设置线条宽度
  plt.rcParams['lines.color'] = 'red'			   # 设置线条颜色
  plt.rcParams['lines.linestyle'] = '-'		   # 设置线条样式
  ```

* `plt.figure(figsize=(length, width))`
  
  * 调整画布大小

### 2.2 直方图

直方图，又称质量分布图，是一种统计报告图，由一系列高度不等的纵向条纹或线段表示数据分布的情况

绘制直方图：`plt.hist(data)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

height = [168, 155, 182, 170, 173, 161, 155, 173, 176, 181, 166, 172, 170]  # 数据
bins = range(150, 191, 5)  # x轴显示的区间

plt.hist(height, bins=bins)
plt.show()
```



### 2.3 条形图

条形图，是用宽度相同的条形的高度或长短来表示数据多少的图形

绘制条形图：`plt.bar(X, Y)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

classes = ['class A', 'class B', 'class C']  # x轴显示的标签
scores = [70, 80, 60]  # 每个标签对应的数据

plt.bar(classes, scores)
plt.show()
```



### 2.4 折线图

折线图，通常显示随时间而变化的连续数据，因此非常适用于体现在相等时间间隔下数据的趋势

绘制折线图：`plt.plot(X, Y)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
% matplotlib inline  # 在jupyter notebook中使用，用来显示图像

year = range(2005, 2020) # x轴数据 [2005, 2020)
height = [157, 160, 162, 163, 167, 170, 173, 175, 174, 179, 182, 182, 182, 182.182, 183]  # 每一个x对应的数据

plt.plot(year, height)
plt.show()
```



### 2.5 饼图

饼图，常用于显示一个数据系列中各项的大小与各项综合的比例

绘制饼图：`plt.pie(data)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']  # 整除显示中文

labels = ['房贷', '饮食', '出行', '教育']  # 每一部分的标签
data = [8000, 2000, 2000, 3000]  # 每个标签对应的数据

# 使用labels关键字参数指定每个部分的label
# 使用autopct关键字参数指定显示的百分数的格式（%%表示%，前一个%是转义字符）
plt.pie(data, labels=labels, autopct="%1.1f%%")
```



### 2.6 散点图

散点图是指在回归分析中，数据点在直角坐标系平面上的分布图，散点图表示因变量而变化的大致趋势，据此可以选择合适的函数对数据点进行拟合

绘制散点图：`plt.scatter(X, Y)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']

data = [[18.9, 10.4], [21.3, 8.7], [19.5, 11.6], [20.5, 9.7], [19.9, 9.4], [22.3, 11], [21.4, 10.6], [9, 9.4], [10.4, 9], [9.3, 11.3], [11.6, 8.5], [11.8, 10.4], [10.3, 10], [8.7, 9.5], [14.3, 17.2], [14.1, 15.5], [14, 16.5], [16.5, 17.7], [15.1, 17.3], [16.4, 15], [15.7, 18]]  # 每个元素的第一个是x轴数据，第二个是y轴数据
datax = [item[0] for item in data]  # x轴
datay = [item[1] for item in data]  # y轴
plt.scatter(datax, datay)

plt.title('超市商品价位与销量散点图')  # 添加图像标题
plt.xlabel('价格（元）')  # 添加x轴说明
plt.ylabel('销量（件）')  # 添加y轴说明
plt.text(16, 16, '牙膏')  # 给固定（x,y）位置添加文字
plt.text(10, 12, '纸巾')
plt.text(20, 10, '洗衣液') 
```

* 关键字参数alpha
  * 透明度 0-1

### 2.7 箱线图

箱线图，又称盒须图、盒式图，是一种用作显示一组数据分散情况的统计图，主要用于反映原始数据分布的特征

绘制箱线图：`plt.boxplot(X, Y)`

![箱线图说明](F:\Java Note\ai\mpl_箱线图.png)



```python
import matplotlib as mpl
import matplotlib.pyplot as plt

data = [77, 70, 72, 89, 89, 70, 90, 87, 94, 63, 81, 99, 80, 95,67, 65, 88, 60, 67, 85, 88, 87, 75, 62, 65, 95, 62, 61, 93, 30]

plt.boxplot(data)
```



### 2.9 极线图

极线图用于表示极坐标下的数据分布情况，多用于显示具有一定周期性的数据

绘制极线图

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
r = [1, 2, 3, 4, 5]  # 极径
theta = [0.0, 1.5707963267948966, 3.141592653589793, 4.71238898038469, 6.283185307179586]  # 角度

# 指定图为极线图
ax = plt.subplot(111, projection='polar')  
# 111是一行一列的第一个子图，projection表示坐标轴是极坐标
ax.plot(theta, r)
plt.show()
```



### 2.10 阶梯图

阶梯图，是一种以无规律、间歇型阶跃的方式表达数值变化的方法。

它不仅可以像折线图那样反映数据发展的趋势，还可以反应数据状态的持续时间

绘制阶梯图：`plt.step(X, Y)`

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']

year = range(2005, 2020)  # x轴坐标
height = [157, 160, 162, 163, 167, 170, 173, 175, 174, 179, 182, 182, 182, 182.182, 183]  # 数据
plt.step(year, height)
```



## 第三章 Matplotlib高阶图形绘制

### 3.1 图标参数设置

import matplotib.pyplot as plt

* `plt.title("图表名称")`

* `plt.xlabel("x轴标签")`

* `plt.ylabel("y轴标签")`

* `plt.xticks(x, name)`

  * x：x轴数值数据

  * name：每一个数值数据对应的要显示的标签

    ```python
    x = [1, 2, 3]
    name = ['A班', 'B班', 'C班']
    plt.xticks(x, name)
    ```

* `plt.text(x, y, str)`
  * x,y：图表上的坐标
  * str：在该坐标位置显示的文字



### 3.2 堆积图

堆积图，常用于综合展示不同分类的指标趋势以及它们的总和的趋势。

比如说，我们想看一下5名同学期末的总分情况，同时，也想知道这5名同学的各科成绩以及它们各自的占比，选用堆积图往往会很清晰

绘制堆积图，使用的是条形图的方法

```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']
number = ['1', '2', '3', '4', '5']
ch = [72, 80, 66, 77, 92]
math = [62, 92, 72, 75, 88]
eng = [76, 81, 73, 75, 80]
chmath = [math[i] + ch[i] for i in range(5)]
x = range(1, 6)
plt.bar(x, ch, color='r', label='语文')  # 红色
plt.bar(x, math, bottom=ch, color='g', label='数学')  # 以语文成绩为底，绿色
plt.bar(x, eng, bottom=chmath, color='c', label='英语') # 以语文数学成绩为底
plt.title('成绩')
plt.legend(loc='upper right')  # 设置label说明在右上角
plt.grid(axis='y', color='gray', linestyle=':', linewidth=2)
plt.xticks(x, number)
plt.xlabel('学号')

plt.show()
```



### 3.3 分块图

分块图，可将不同数据集进行并列显示，通常可用于对同一方面的不同主体进行比较（例如用分块图来比较1班，2班，3班的各科平均分情况）

绘制分块图：本质还是条形图

```python
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams['font.sans-serif'] = ['SimHei']
name_list = ['语文', '数学', '英语']
c1 = [81.4, 83, 87.1]  # 1班的语数外成绩 
c2 = [85.6, 87.4, 90]  # 2班的语数外成绩
c3 = [78, 81.2, 86.1]  # 3班的语数外成绩
width = 0.4  # 每个分块的宽度为0.4
# 每块区域的间隔需要大于3*width，这里取2
x = np.array([1, 3, 5])  # 间隔是区域间隔
plt.bar(x, c1, fc='r', label='c1', width=width)
plt.bar(x + width, c2, fc='g', label='c2', width=width)
plt.bar(x + 2 * width, c3, fc='c', label='c3', width=width)
plt.xticks(x + width, name_list)
plt.legend(loc='upper right')
plt.title('三班级成绩图')
plt.xlabel('科目')
plt.ylabel('成绩')

plt.show()
```



### 3.4 绘制气泡图

![气泡图](F:\Java Note\ai\mpl_气泡图.png)

绘制气泡图：`plt.scatter(X, Y, s=Z)`  s是气泡大小，即size

```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']
x = [22, 22, 23, 24, 25, 25, 26, 27, 28, 29, 30, 30, 32, 32, 32, 33, 34, 34, 35, 36, 37, 38, 38, 39, 40, 42, 43, 43, 45, 45, 46, 48, 48, 48, 50, 52, 56, 57, 60, 62]
y = [176, 186, 164, 177, 183, 194, 180, 179, 190, 170, 168, 192, 173, 178, 181, 186, 177, 187, 180, 195, 179, 186, 187, 190, 182, 184, 176, 178, 164, 185, 181, 175, 173, 172, 172, 169, 168, 182, 188, 174]
z = [70, 220, 50, 170, 210, 270, 150, 150, 360, 150, 150, 200, 150, 170, 170, 160, 180, 460, 480, 480, 490, 300, 300, 250, 300, 250, 350, 180, 100, 250, 160, 170, 160, 180, 150, 150, 130, 180, 100, 160]
plt.scatter(x, y, s=z)
plt.show()
```



