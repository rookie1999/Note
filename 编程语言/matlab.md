特点：

1. 如果在一行语句的结尾加上“；”分号，语句的返回值不会被打印或显示
2. `clc`命令清空command line

## 1  Matlab variable

Variable name

* Start with a letter
* contain only letters, numbers and underscores
* are case-sensitive

如果没有使用=进行赋值，结果会自动赋值给ans变量



当前所有变量都会显示在workspace中，如果要将其保存到文件里，可以使用命令`save filename`，filename以`.mat`结尾

如果只是想保存workspace中的几个变量，使用`save filename a b c`，a，b，c是变量名

如果想要保存**文本格式**，使用命令`save hello.txt a b -ascii`

如果想要加载某个`.mat`文件，使用命令`load filename`

如果想要加载文件里指定的变量，使用命令`load filename a b c`

`clear`用来清空workspace

`clear a`用来删除workspace中的变量a

`who`用来显示所有在workspace中的变量

`whos`命令用来显示所有在workspace中的变量及其size，bytes，class和attributes

`which a`用来检查变量a是否被使用



matlab里面的变量都是array

一个标量其实是一个1x1的array

## 2  运算和运算符

==判断是否相等  ~=判断是否不等

&&		||		~ Not

xor(a, b) 返回a与b异或后的值

^ 次方运算

显示的时候只会显示小数点后四位（默认是format short），但是内部存储很精确  

通过使用命令`format long`显示小数点后更多位数

sqrt(a)  返回根号a

## 3  函数

sin(x) cos(x)

abs(x)

log10(x)

fun(x)

exp(x)     e^x指数函数

eig(x)    计算x的特征值

min(x)  如果是一个变量接收是，X矩阵每一列的最小值

​			如果是两个变量接收，X矩阵每一列的最小值及其相应的索引

plot(x, y)将x，y绘制成figure  (x, y需要有相同数量的数)

plot(x, y, format)  按照format绘制x-y图形

​		format: (color line marker)

​			m:s    magenta, dotted line with square markers

​			g--*	green, dashed line with star markers

​			r-		red, solid line with no markers

plot(y, "LineWidth", 5)  用粗的线画y

plot(x, y, "ro-", "LineWidth", 5)

xlabel(str)   将str作为x轴的名称

ylabel(str)   将str作为y轴的名称

title(str)  将str作为figure的title

legend(str)  将str放在函数图像上

bar(x, y)  绘制x-y的柱状图

***hold on***  当前绘制的figure不会在下一次绘制的时候消失

`print -dpng 'figure1.png'`将当前figure以'figure1.png'的名称保存为png文件

subplot(a, b, x)	将显示窗口划分为axb的grid，然后当前操作的是第x个

axis(x1, x2, y1, y2)	修改figure的x轴范围为x1到x2，y轴范围为y1到y2

`clf`清除figure

imagesc(A)	将A显示到figure上，不同的颜色代表不同的值

该方法一般与以下两个command一起使用

colorbar	显示色彩条，并与数值进行映射

colormap gray	切换颜色



如果一个函数有多个值返回

[output1, output2, ...] = f(x)



### Comma chain of function calls

a = 1, b = 2, c = 3  将命令写在一行，用逗号隔开，会打印每一个command的输出

如 `imagesc(A), colorbar, colormap gray`



## 4  Vector

行向量  x = [1, 2, 3]  或  x = [1 2 3]

列向量  y = [1; 2; 3]

find(v > 3)		返回向量v中大于3的元素的索引

sum(v)			返回向量v的和

prod(v)			返回向量v的乘积

ceil(v)			对向量v中的每个浮点数实现进一法

### create a uniformly spaced vector

1. 使用":"分号——`[lowerbound:spacing:upperbound]

   spacing不写 默认是1

   ```matlab
   % a是上界，b是下界
   a = 0
   b = 100
   x = [a:0.5:b]  % 从a开始，每隔0.5取一个数，一直到最后一个数的下一个超过b为止  得到一个行向量
   ```

2. 使用linspace函数

   linspace(a, b)  指定上下界，返回上下界在内的100个数，每个间隔相同

   linspace(a, b, numPoints)  指定上下界，返回上下界在内的numPoints个数，每个数间隔相同

### 使用逻辑向量来访问满足条件的元素

1. 提取满足条件的元素

   ```matlab
   % 提取v向量中大于0.1的元素
   v = [0.01;0.02;0.01;0.27;0.39;0.01;0.65;0.5]
   logical = v > 0.1  % 返回一个与v向量同型的向量，元素的值为v对应位置与0.1比较的值，如果大于0.1，则为1；如果不大于，则为0
   ans = v(logical)  % 如果v向量的元素在logical中对应位置为1，则提取出来，若为0，则忽略
   
   activity =
   
       0.2700
       0.3900
       0.6500
       0.5000
   ```

2. 修改满足条件的元素

   ```matlab
   % 将v向量中小于等于0.1的元素修改为0
   logical = v <= 0.1
   v(logical) = 0
   ```

### 向量的运算

element-wise：对每个元素进行.之后的运算符操作

* .*
* ./
* .^

### 向量的转置

1. 使用transpose函数

2. 在需要被转置的向量后添加单引号

   ```matlab
   v = [1, 2, 3];
   c = v'
   ```

## 5  Matrix

ones(a, b)  生成一个a行b列的矩阵，其元素均为1

zeros(a ,b) 生成一个a行b列的矩阵，其元素均为0

rand(a, b)  生成一个a行b列的矩阵，其元素为0-1的随机数

randn(a, b)  生成高斯随机变量，normally distributed random numbers

randi(a, b)  uniformly distributed random integers

diag(v, k)  生成第k条对角线的对角矩阵（k=0是主对角线，k>0是主对角线上面的对角线）

diag(A)  返回矩阵A的主对角线元素的列向量形式

eye(n)  生成一个n阶的单位矩阵

magic(a)  生成一个n阶方阵，该方阵的行列对角线的元素和都是相同的



size(A)  通过1x2的行向量返回矩阵A的行数和列数

size(A, 1)  返回矩阵A的行数

size(A, 2)  返回矩阵A的列数

length(A)  返回矩阵A的最大维数（即行数与列数中较大的一个）     一般用在vector

sum(A, 1)	求矩阵A的每一列的和

sum(A, 2)	求矩阵A的每一行的和

inv(A)			求矩阵A的逆矩阵

transpose(A)		求矩阵A的转置矩阵



[r, c] = find(A > 7)	返回矩阵A中所有大于7的元素的索引，r是行标，c是列标

### 访问Matrix的元素

A(a, b)  访问矩阵A的第a行，第b列元素

A(a, :)    返回矩阵A的第a行

A(:, b)   返回矩阵A的第b列

A([1 5], 2:4)   返回矩阵A的第1和5行中的第2到第4列

A(end, 1)	返回矩阵A的第一列的最后一个元素

如果只对矩阵使用一个index，它会按列进行遍历（或者可以理解为先使用A(:)变成vector再访问对应索引元素）

### Element-wise operation

* Matrices需要是同型矩阵
* +，-
* .*
* ./
* .^

### Matrix concatenation

* concatenate vertically

  * use semicolon

  ```matlab
  % a and b must have the same number of columns
  a = [1 1];
  b = [2 2];
  res = [a; b]
  ```

* concatenate horizontally

* use comma

  ```matlab
  % a and b must have the same number of rows
  a = [1, 2, 3; 4, 5, 6]
  b = [7; 7]
  res = [a, b]
  ```

### Reshape array

reshape(arr, row, col)  将arr转换为row x col维的矩阵，按列填充（row和col的乘积为arr的元素数量）

reshape(arr, row, [])  将arr转换为row行的矩阵

### Statistical function

mean(A)  计算矩阵A的每一**列**的平均值

mean(A, 2)  计算矩阵A的每一**行**的平均值

mean(A(:))  先将A转换为n*1的列向量，再计算该列向量的平均值



max(A)    计算矩阵A的每一列的最大值

max(v)   返回向量v的最大值

min(A)     计算矩阵A每一列的最小值

min(v)	返回向量v的最小值

std(scores, 0, 2)  计算标准偏差

## 6  Matlab Programming

### if-else statement

```matlab
if expression
	code block;
else
	code block;
end
```

```matlab
if expression
	code block;
elseif expression
	code block;
else
	code block;
end
```

### for loop

```matlab
for k = 1:99
	code block;
end

v = [1:10]
for i = v,
	disp(i)
end
```

* break
* continue

### while loop

```matlab
while balance < 25000
	balance = (1+r)*balance;
end
```

### function

including two parts:

* declaration
* body

function的方法注释一般写在declaration和body中间

```matlab
function [out1, out2, ...] = functionName(in1, in2, ...)
statement 1;
statement 2;
statement 3;
statement 4;
...
end
```

例子

```matlab
function s = geoSum(r, n)
% geoSum sums the first n terms of a geometric series with common ratio r
if r == 1
	s = n;
else
	s = (1 - r^n) / (1 - r);
end
```



* 如果将函数的定义以及使用都放在一个脚本中，该函数仅作为local function，只允许该脚本使用

* 如果想要让该函数作为global function，需要将函数定义单独写在一个脚本中，该脚本的名称与函数名一致，并且该脚本在current folder或者search path上

### 将函数作为输入

第一种方式：使用匿名函数（仅适合那些简单的函数）

```
匿名函数变量名 = @(输入变量)  返回含有输入变量的表达式的值
```

```matlab
% f的类型是  <function_handle>
f = @(x) x^3-x*2+1
```

第二种函数：使用`@`符号传入存在或者自定义的函数

```matlab
fminsearch(@mean, x1)
```





## 7 Function Precedence Order

对于同名的变量

1. Variable
2. File in current folder
3. File on search path