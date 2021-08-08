# Matlab教程

## 1、简介

## 2、Matlab计算机与矩阵操作

### 2.1 Matalb as A Calculator

Operators: + - * / ^

Result is computed, and displayed as `ans`

如果***不想要Matlab显示运算结果***可以在命令的最后增加一个分号`;`

### 2.2如何查看Matlab函数的帮助信息

1. google搜索Matlab sin
2. 在Matlab软件自带的search documentation上搜索
3. 在command line上输入 help sin

### 2.3 特殊符号和function

| 特殊符号             | Matlab形式 |
| -------------------- | ---------- |
| π                    | pi         |
| e<sup>a</sup>        | exp(a)     |
| ∞                    | Inf        |
| 2.2204e-016   无穷小 | eps        |
| not a number         | NaN        |

| 函数     |                         |
| -------- | ----------------------- |
| sin      | sin                     |
| cos      | cos                     |
| tan      | tan                     |
| ln       | log                     |
| log10    | log10                   |
| rem(a,b) | 返回a/b的余数           |
| prod(A)  | 返回A向量所有数字的连乘 |

### 2.3 变量

变量不需要声明类型，默认是double

变量区分大小写

变量不能以数字开头

clear nameOfVariable：	在当前的工作区消除指定name的变量

clear：	清空工作区

### 2.4 Matlab 调用优先级

1. 变量variable
2. 内建函数built-in function
3. 子函数subfunction
4. 私有函数 private function
   1. MEX-file
   2. P-file
   3. M-file

综上，不要使用keyword当作variable的name

### 2.5 显示格式

| 格式   | 位数                      |
| ------ | ------------------------- |
| short  | 小数点后四位   3.1416     |
| long   | 小数点后15位              |
| shortE | 小数点后四位+科学计数法   |
| longE  | 小数点后十五位+科学计数法 |
| bank   | 小数点后两位              |
| hex    | 十六进制                  |
| rat    | 分数形式                  |

默认的显示格式是short

切换显示格式使用format指令		`format long`切换成long形式

### 2.6 输入矩阵

* Row vector

  * a = [1 2 3 4]

* Column vector

  * b = [1; 2; 3; 4]

* 输入一个矩阵

  * 3X3矩阵
  * A = [1 5 8; 6 11 17; 21 9 3]

* Array Indexing

  * Select a certain subset of elements inside a matrix

  * 对于row vector： a(2) = 2

  * 对于column vector： b(4) = 4

  * 对于矩阵：

    * A(2,3) = 17   2代表row  3代表column

    * 如果把矩阵当作是一维数组，编号从上到下然后再对第二列进行从上到下A(3)=8;  A(5)=11

    * ```matlab
      A([1 3 5]) = [1 31 17]
      // row[1 3] 代表选取第一行和第三行
      // column[1 3] 代表选取第一列和第三列
      A([1 3], [1 3]) = [1  6]
      				  [31 7]
      ```

  * 修改矩阵的元素的值

    * A(row,col)=新值  		// 新值可以是数值、向量也可以是矩阵

### 2.7 Colon Operator

>Want to create a long array: A = [1 2 3 ... 100]

* Creates vectors or arrays, and specify for iterations
* Syntax
  * j:k  =>  j  j+1  j+2  ...  k
  * j:i:k   =>   j   j+i   j+2i ... 

```
str='a':2:'z'
结果：
'acegikmoqsuwy'
```

* A(3,:) = [31 2 7]
  * 这里colon operator是作为全部的index
  * 另外，如果要删除A矩阵的行 A(3,:)=[]

### 2.7 Array Concatenation

有一个矩阵A和B

F=[A B] 表示一个由A和B组成的新矩阵，A和B在同一行（类似增广矩阵）

F=[A;B]表示一个由A和B组成的新矩阵，A和B在同一列

### 2.8 ArrayManipulation

Operators on array: 	+  -  *  /  ^  .  '

矩阵加上实数就是每一个元素加上这个实数

A.*B指的是A的a<sub>mn</sub>和b<sub>mn</sub>对应相乘

A/B 就是A乘以B的inverse矩阵

A./B就是A的a<sub>mn</sub>和b<sub>mn</sub>对应相除

A^a就是a个A矩阵相乘

A.^a就是A矩阵的每一个元素变成原来的a次方

B'  就是B的转置矩阵

### 2.9 Some special matrix

* linspace():	线性增长的向量
  * eg.  linspace(0, 13, 6):	第一个元素是0，最后一个元素是13，一共有六个，每个之间的差值相同符合线性
* eye(n):   n*n的identity matrix（对角线为1，其余为0）
* zeros(n1,n2):   n1*n2的矩阵，元素均为0
* ones(n1,n2):   n1*n2的矩阵，元素均为1
* diag([a1,...,an]): n*n的矩阵，对角线为a1到an，其余为0
* rand(n):   n*n的矩阵，元素的值在0到1随机

### 2.10 Some Matrix Function

* max(A)
  * 返回每一列的最大值
* max(max(A))
  * 返回矩阵最大值
* min(A)
  * 返回每一列的最小值
* min(min(A))
  * 返回矩阵最小值
* sum(A)
  * 返回每一列的和
* sum(sum(A))
  * 返回矩阵的和
* mean(A)
  * 返回每一列的平均值
* mean(mean(A))
  * 返回矩阵的平均值
* sort(A)
  * 对矩阵A的每一列进行排序
* sortrows()

## 3、结构化程式与自定函数

### 3.1 Matlab指令

* who
  * 查看当前工作区有哪些变量
* whos
  * 查看当前工作区的变量以及其详细信息，e.g. size和类型
* iskeyword
  * 列出Matlab中所有已经被定义的关键字
* clc
  * 清空command window

### 3.2 Matlab Script脚本

> A file containing a series of matlab commands
>
> scripts need to be saved to a <file>.m file before they can be run

small tips：

1. 使用`%%`分隔一段一段代码，如果想要执行某一段，使用`Run Section`
2. `Ctrl+I`格式化代码

### 3.3 流程控制

| if, else if, else       | 条件分支                       |
| ----------------------- | ------------------------------ |
| for                     | 循环分支                       |
| switch, case, otherwise | 选择分支                       |
| try, catch              | 异常机制                       |
| while                   | 循环分支                       |
| break                   | 跳出循环                       |
| continue                | 跳过当前循环进入下一次迭代次数 |
| end                     | 标记代码块的结束               |
| pause                   | 暂时停止执行代码               |
| return                  | 返回调用函数处                 |

关系运算：

* matlab的不等于符号是`~=`

#### 3.3.1 条件分支

```matlab
if condition1
	statement1
elseif condition2
	statement2
else
	statement3
end
```

```matlab
switch expression
case value1
	statement1
case value2
	statement2
...
otherwise
	statement
end



eg.

input_num=-1;
switch input_num
    case -1
        disp('negative 1');
    case 0
        disp('zero');
    case 1
        disp('postive 1');
    otherwise
        disp('other value');
end
```

#### 3.3.2 循环分支

##### while

```matlab
while expression
	statement
end
```

```matlab
%计算1到999的和
sum=0;n=1;
while (n<=999)
    sum=sum+n;
    n=n+1;
end
```

##### for

```matlab
for variable=start:increment:end
	commands
end
```

```matlab
i=1;
for n=1:2:10
    a(i)=2^n;   %变量a此时是向量，而不是指代某个值
    i=i+1;
end
disp(a)
```

注：上面的例子a是动态扩充的，即每次迭代都会增大a，需要花费额外的时间

如果知道a的具体大小，对a进行preallocate，会减少程序运行的时间

```matlab
tic
A = zeros(2000, 2000);
for ii = 1:size(A,1)
	for jj = 1:size(A,2)
		A(ii,jj) = ii + jj;
	end
end
toc
```

### 3.4 Tips for Script Writing

* 在脚本的一开始
  * `clear all` 删除所有的变量
  * `close all` 关闭所有的figure
* 使用semicolon`;`在命令的最后去隐藏不需要的输出
* 使用`Ctrl+C`强行结束正在运行的脚本
* 使用省略号ellipsis`...` 进行换行

## 4、变数与档案存取

## 5、初阶绘图

## 6、进阶绘图

## 7、图形界面GUI程式设计

## 8、影像处理初阶（一）

## 9、影像处理初阶（二）

## 10、数值微积分

## 11、方程式求根

## 12、线性方程式与线性系统

## 13、统计

## 14、回归与内插