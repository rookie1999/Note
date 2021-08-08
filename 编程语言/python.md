# Python

>项目代码地址：D:\pycharm\code\study

## 1、输出函数print

* 默认输出在控制台，并且输出完毕自动生成一个换行

* 也可以输出到文件中

  ```python
  # 打印到文件中
  # a+ 表示模式是追加的方式（如果文件不存在就创建文件）
  file = open("./demo1.txt", "a+")
  print('Hello world', file=file)
  file.close()
  ```

* 在一行输出, 默认会添加空格

  ```python
  print("Hello", "world.", "I love", "Python")
  ```

* 不换行输出

  ```python
  print("Hello", end='')  #结束符号为“”，即不换行输出
  print("Hello", end='\t')
  ```

  

## 2、转义字符

* `\t \n \r \b \\ \' \" `

* 如果想要转义字符不起作用，可以在字符串之前加上r或R

  * 但是字符串的最后一个字符不能是`\`

  ```python
  print("Hello\nworld") # 输出Hello 换行 world
  print(r"Hello\nworld")  # 输出Hello\nworld
  ```



## 3、保留字

* 在python中查看有哪些word已经被定义为保留字
* 保留字不能作为标识符

```python
import keyword
print(keyword.kwlist)
```



## 4、变量

* 变量由三部分组成

  * 标识
    * 表示对象所存储的内存地址，使用内置函数id(obj)获取
  * 类型
    * 表示对象的数据类型，使用内置函数type(obj)获取
  * 表示对象所存储的具体数据，使用print(obj)打印

  ```python
  name = "zhangsan"
  print("标识", id(name))
  print("类型", type(name))
  print("值", name)
  ```
  
* 全局变量	

  * 用global声明的变量是全局变量

* 局部变量（在函数内部的变量）

* 变量对大小写敏感



## 5、数据类型

* type(变量)

  * 返回该变量的类型

* isinstance(变量, 类型名)

  * 返回布尔值

* 常用的数据类型

  * 整数类型：int

    * 十进制：默认
    * 二进制：0b开头
    * 八进制：0o开头
    * 十六进制：0x开头

  * 浮点数类型：float

    * 使用浮点数计算可能会出现小数位数不确定的情况
    * 解决方案：导入`decimal`模块

  * 复数类型：complex

    ```python
    c = 1+2j
    ```
  
  * 布尔类型：bool

    * True
    * False （0、空字符串、None）
    * 可以转化为整数
      * True -> 1
      * False -> 0
    
  * 字符串类型：str
  
    * 可以使用单引号、双引号、三引号
    * 使用三引号进行输出，字符串可以写在多行，并且会保留字符串格式
  
    ```python
    str1 = "Life is short, I use Python"
    str2 = 'Life is short, I use Python'
    str3 = """Life is short,
        I use Python
    """
    str4 = '''Life is short,
  I use Python'''
    print(str1)
    print(str2)
    print(str3)
    print(str4)
    ```
  
  * 空值None

## 6、类型转换

* str()
  * 将其他类型转换为字符串
  * 注意：Python中字符串与其他类型进行“+”运算时，需要强制类型转换，否则报错
* int()
  * 将其他数据类型转换为整数
  * **对于字符串，只能转换原本是整数串的字符串**
  * 对于浮点数，只保留整数部分
* float()
* bool()
  * 非0整数是True
  * 0是False
* complex()
* ord()
  * 返回字符的Unicode编码
  * 通常用于字符转ASCII码



## 7、注释

* 单行注释

  * 以`#`开头

* 多行注释

  * 使用三引号字符串

* 使用注释切换字符编码

  ```python
  #config:gbk
  ```



## 8、input输入函数

* str input(提示文本)
  * 返回的是用户输入的字符串



## 9、运算符

* 算术运算符

  * 加`+`  减`-`   乘`*`  除`\`  整除`\\` (round(num, digit)进行小数点后位数的保留)
  * 取余`%`
    * 取余运算符需要和操作数之间加上空格
  * 幂运算`**`
  * 对于一正一负的情况使用整除和取余

  ```python
  # 对于一正一负的情况使用整除和取余
  
  # 向下取整
  print(23 // -7)  # -4
  print(-23 // 7)  # -4
  
  # 余数 = 被除数 - 除数 * 商
  print(23 % -7)  # -5 = 23 - (-7) * (-4)
  print(-23 % 7)  # 5 = (-23) - 7 * (-4)
  # 所以被除数为负，余数为正
  # 另一种情况正好相反
  ```

* 赋值运算符

  * 运算顺序：从右到左

  * 链式赋值

    * 通过链式赋值的变量共用赋值运算符右边的对象

    ```python
    # 链式赋值
    a = b = c = 10
    print(a, id(a))
    print(b, id(b))
    print(c, id(c))
    ```

  * 参数赋值

    * `+=  -=  *=  /=  //=  %= `
    * 采用这种的赋值方式会自动提升数据类型（即运算过程中有隐式类型转换）

  * 解包赋值
  
    ```python
    a, b, c = 10, 20, 30
    # a=10, b=20, c=30
    
    # 使用解包赋值交换两个数的值
    num1 = 10
    num2 = 20
    num1, num2 = num2, num1
    ```

* 比较运算符

  * `> >= <= < == !=`
  * `==`比较的是值
  * 比较对象的标识使用  `is`或者`is not`

  ```python
  a = 10
  b = 10
  print(a == b)
  print(a is b)   # 因为对于整数对象来说，如果已经存在一份对象的值为当前整数，直接重复使用该对象
  
  list1 = [11, 22, 33, 44]
  list2 = [11, 22, 33, 44]
  print(list1 == list2)
  print(list1 is list2)  # 对于数组来说，由于是复合类型，所以会重新创建对象
  ```

* 布尔运算符

  * `and   or   not   in   not in`

  ```python
  st = "Helloworld"
  print("s" in st)
  print("l" in st)
  print("s" not in st)
  ```

  * 布尔运算符都是短路运算符
  * 比如计算`a and b`，如果a是False，结果必定为False，直接返回a；如果a是True，结果必定取决于b，直接返回b

* 位运算符
  
  * 位与`&`
  * 位或`|`
  * 左移位运算符`<<`:高位舍弃，低位补0
  * 右移位运算符`>>`：低位舍弃，高位补0
  
* 运算符的优先级
  
  * 幂运算>正负号>算术运算 > 位运算 > 比较运算 > 逻辑运算



## 10、对象的布尔值

* Python一切皆对象，所有对象都有一个布尔值
  * 获取对象的布尔值，使用内置函数bool()
* 以下对象的布尔值为False
  * False
  * 数值0
  * None
  * 空字符串
  * 空列表
  * 空元组
  * 空字典
  * 空集合



## 11、选择结构

* if,elif,else

  ```python
  score = int(input("请输入您的成绩：\n"))
  grade = score // 10
  if grade >= 9:
      print("您的等第是：A")
  elif grade >= 8:
      print("您的等第是：B")
  elif grade >= 7:
      print("您的等第是：C")
  elif grade >= 6:
      print("您的等第是：D")
  else:
      print("无法查询您的成绩")
  ```
  * 另外一种编写形式

  ```python
  score = int(input("请输入您的分数\n"))
  if 90 <= score <= 100:
      print("A")
  elif 80 <= score < 90:
      print("B")
  elif 70 <= score < 80:
      print("C")
  elif 60 <= score < 70:
      print("D")
  else:
      print("无法查询您的分数")
  ```

* python不能使用`{}`来表示代码段，而是使用**缩进**

* 条件表达式

  ```python
  x = 10
  y = 20
  # 如果x>y，res = x;反之，结果为y
  res = x if x > y else y
  ```



## 12、pass语句

* 语句什么都不做，只是一个占位符，用在语法上需要语句的地方

```python
x = 10
y = 9
if x > y:
    pass
else:
    pass
```



## 13、range函数

* 内置函数
* 用于创建一个整数序列
* 创建range对象的三种方式
  * range(stop)
    * 创建一个[0, stop)之间的整数序列，步长为1
  * range(start, stop)
    * 创建一个[start, stop)之间的整数序列，步长为1
  * range(start, stop, step)
    * 创建一个[start, stop)之间的整数序列，步长为step
* 返回值是一个迭代器对象
  * 如果需要用列表形式返回，可以使用list()函数
* range类型的优点：不管range对象表示的序列有多长，所有range对象占用的内存空间是相同的，因为仅仅需要存储start，stop和step，只有当用到range对象时，才会去计算序列中的相关元素
* in与not in判断整数序列中是否存在（不存在）指定的整数



## 13、循环结构

### 13.1 while

```
while 布尔表达式:
	循环体
	# 需要对布尔表达式的变量进行修改，否则会死循环
```

### 13.2 for in 循环

```python
# in 后面的对象需要是可迭代对象
for s in "Hello":
    print(s)
print("-------------")
for i in range(11):
    print(i)
print("------------")
# 如果不需要使用句柄，可以使用“_”代替
for _ in range(5):
    print("Life is short, I use Python")
```

### 13.3 break和continue

### 13.4 与循环搭配使用else

* 如果循环正常执行完毕之后，会执行else语句
* 如果循环中遇到break语句，则else语句不会执行

```python
for i in range(3):
    pwd = input("请输入您的密码：\n")
    if pwd == "8888":
        print("密码正确，欢迎您~")
        break
    else:
        print("密码错误")
else:
    print("三次机会已用完，账户锁定一小时")
```



## 14、列表list

### 14.1 列表的创建

```python
# 第一种创建方式：使用中括号
list1 = ["I am", "a handsome", "boy", 111]
# 第二种创建方式：使用list()内置函数
list2 = list(["woshi", "zhazhahui", "!"])
```

### 14.2 列表的特点

* 列表元素按顺序有序排序
* 索引映射唯一一个数据
  * 列表有两个索引
  * 第一个索引从前往后，即第一个元素索引为0，第二个为1，以此类推[0, N-1]
  * 第二个索引从后往前，即最后一个元素索引为-1，倒数第二个索引为-2，以此类推[-N, -1]
  * 根据索引查找元素，如果索引对应的元素不存在，则产生IndexError
* 列表可以存储重复数据
* 任意数据类型可以混合存储
* 根据需要动态分配和回收内存

### 14.3 index函数获取指定元素索引

```python
list1 = [1, 4, 6, 3, 2, 7, 3]
print(list1.index(3))  # 返回指定元素在列表中第一个出现的索引
# print(list1.index(-1)) # ValueError,由于-1不在列表中
print(list1.index(3, 4, 7))  #可以指定start和stop，从[start, stop)查找是否存在指定元素，如果存在，返回其索引 
```

### 14.4 切片——获取列表中的多个元素

* 语法格式： `列表名[start:stop:step]`

* 切片的结果：是原列表片段的拷贝，生成新对象

* 切片的范围：[start, stop)

* step:默认为1，简写[start:stop]或者[start:stop:]

* 当step为正数，从start开始从前往后计算切片

  * [:stop:step]
    * 切片的第一个元素是列表的第一个元素

  * [start::step]
    * 切片的最后一个元素是列表的最后一个元素

* 当step为负数，从start开始从后往前计算切片

  * [:stop:step]
    * 切片的第一个元素默认是列表最后一个元素
  * [start::step]
    * 切片的最后一个元素默认是列表的第一个元素

```python
# 语法格式：列表名[start:stop:step]
# 从列表start开始，到stop-1处结束，步长为step截取列表

lst = [1, 2, 3, 4, 5, 6, 7, 8]
print(id(lst))
lst2 = lst[1:7:1]
print(id(lst2))   # 是原列表片段的拷贝，生成新列表
print(lst2)
# 默认步长为1
print(lst[1:7])
print(lst[1:7:])
# 如果start不写，默认是列表开始
print(lst[:7:1])
# 如果stop不写，默认是列表末尾
print(lst[1::1])
# start:2  stop:7  step:2
print(lst[2:7:2])

print("----------如果step是负数-------")
# 从start开始从后往前数
# 默认start为列表最后一个元素即-1，stop为-N-1即列表开始-1
print(lst[::-1])  # reverse list
print(lst[-1:-6:-2])
```

### 14.5 列表的遍历

* 判断元素是否在列表中
  * in
  * not in
* 使用for in循环对列表进行遍历

```python
lst = [30, "NB", "xswl", 87.1]
print("NB" in lst)
print("***" in lst)

for item in lst:
    print(item)
```

### 14.6 列表的增加

* append(element)
  
  * 向列表的最后添加element，不会创建新列表
  
* extend(iterable_obj)

  * 向列表的末尾一次性添加多个元素
  * 将iterable中的所有元素添加到列表最后

* insert(index, element)

  * 在指定索引index的位置上添加element

* 使用切片替换多个元素

  * 将切片切出来的列表替换为指定列表的所有元素

  ```python
  lst = [1, 2, 3, 4, 5, 6]
  lst3 = ["I", "love", "Python"]
  lst[1:6:2] = lst3
  print(lst)
  ```

### 14.7 列表的删除

* remove(element)

  * 删除element第一次出现的位置
  * 如果列表不存在element，会产生ValueError

* pop(index)

  * 删除指定索引上的元素，并返回
  * 如果索引不存在，则产生IndexError
  * 如果不指定参数，则删除最后一个

* 切片

  * 将切片列表赋值为空列表

  ```python
  lst = [10, 20, 30, 40, 50]
  lst[1:4:] = []
  print(lst)
  ```

* clear()

  * 清空列表

* del

  * 删除列表对象

### 14.8 列表的修改

* `lst[1] = 10`
* 使用切片进行修改

### 14.9 列表的排序

* sort
  * 使用关键字参数`reverse=True`，进行降序排序
  * 使用关键字参数`reverse=False`，进行升序排序
  * 如果不使用参数，默认进行升序排序
  * 不会产生新列表
* 使用内置函数sorted
  * 对原列表不做修改，产生一个排序的新列表
  * 使用关键字参数`reverse=True`，进行降序排序
  * 使用关键字参数`reverse=False`，进行升序排序
  * 如果不使用参数，默认进行升序排序

```python
print("-----使用sort进行排序-----")
lst = [10, 69, 54, 78, 32, 14, 24]
lst.sort()
print(lst)
print("-----------使用sorted------")
lst = [10, 69, 54, 78, 32, 14, 24]
new_lst = sorted(lst)
print(new_lst)
```

### 14.10 列表的生成式

```python
lst = [i for i in range(1,10)]
print(lst)
lst = [i * i for i in range(1, 10, 1)]
print(lst)
```

### 14.11 列表的操作符

* 比较操作符
  * 如`list1 > list2`
  * 返回值是True,False
  * 从第一个元素开始比较，如果有不同，直接返回False
* +运算符（连接两个列表）
  * 拼接两个列表，生成一个新列表
* 重复操作符*
  * `list1 * 3`将list1拼接三次并返回



## 15、字典dict

### 15.1 什么是字典

* Python内置的数据结构之一，与列表一样是一个可变序列
* 以键值对的方式存储数据，字典是一个无序序列
* 字典的实现原理
  * 根据key的hash值查找value

### 15.2 字典的创建

* 用花括号的形式

* 用内置函数dict()

  ```python
  dict1 = {"name": "zhangsan", "age": 18}
  print(dict1)
  
  dict2 = dict(name="lisi", age=19)
  print(dict2)
  ```

* fromkeys(key[, v])
  * key是元组类型，元组中的每一个值都是一个key
  * v是可选的，默认是None，如果有值，则会赋值给所有的key

### 15.3 字典元素的获取

* 使用`字典名['键']`
* 使用get(key)
* 二者的区别：当查找的键不存在时
  * 使用第一种方式会产生KeyError
  * 使用第二种方式会得到`None`
  * 第二种方式还可以设置不存在的时候的默认值，通过第二个参数

```python
dict1 = {"name": "zhangsan", "age": 11}
print(dict1['name'])
print(dict1.get("age"))

# print(dict1['nihao'])  # 会产生KeyError
print(dict1.get("nihao"))
print(dict1.get("nihao", 12))
```

### 15.4 字典键key的判断

* `key in dict`
  * 返回key是否在字典dict
* `key not in dict`
  * 返回key是否不在字典dict

### 15.5 删除键值对

* del dict['key']

### 15.6 清空字典

* clear()

### 15.7 字典的新增或修改

* `dict[key] = value`
* update(A)    将字典A中所有与调用对象相同的键的值都改为字典A中的值

### 15.8 获取字典试图

* keys()
  * 获取所有的键，类型`dict_keys`
* values()
  * 获取所有的值，类型`dict_values`
* items()
  * 获取所有的键值对，类型`dict_items`
* 以上三种视图都可以转化为列表

* items转换为列表后的元素为**元组**

### 15.9 字典的遍历

* `for item in dict:`
  * item是dict字典中的键

### 15.10 字典的特点

* 所有元素都是k-v对，且key不允许重复
* 元素是无序的
* 字典的key必须是不可变对象
  * 不可变对象有整数，字符串
  * 列表是可变对象
* 字典会根据需要动态伸缩
* 字典会浪费较大的内存，是用空间换时间

### 15.11 字典的生成式

* 内置函数zip()
  * 用于将可迭代对象作为参数，将对象的元素打包成一个元组，然后返回这些元组的列表
* `{item : price for item, price in zip(items, prices)}`

```python
items = ['fruit', 'book', 'cat']
prices = [10, 15, 31]
dict1 = {item.upper(): price for item, price in zip(items, prices)}
print(dict1)
```



## 16、元组tuple

### 16.1 什么是元组

* 元组
  * Python内置的数据结构之一，是一个不可变序列
* 不可变序列和可变序列
  * 不可变序列：字符串，元组
    * 没有增删改操作
  * 可变序列：列表，字典
    * 可以对序列执行增删改，对象地址不会发生改变

### 16.2 元组的创建方式

* 直接使用小括号，元素之间用逗号隔开
* 使用内置函数tuple
  * 内置函数的参数需要添加小括号
* 如果元组只包含一个元素，需要添加逗号

### 16.3 为什么元组是不可变序列

* 在多任务环境下，同时操作对象不需要加锁
* 因此，在程序中尽量使用不可变序列
* 注意事项：元组中存储的是对象的引用
  * 如果元组中对象本身是不可变对象，则不能再引用其他对象
  * 如果元组中对象是可变对象，则引用不能改变，该对象的数据可以改变

### 16.4 元组的遍历

* `for item in t:`
  * t是元组
  * item是元组中的每一个元素

### 16.5 元组的修改

* 与字符串的修改一样，通过新对象的生成去修改

```python
>>> temp = temp[:2] + (3,) + temp[2:]
>>> temp
(1, 2, 3, 4, 5)
```

### 16.6 操作符

* 重复操作符*
* 拼接操作符+

### 16.7 tuple的方法

* count(num)
  * 计算num在元组中出现的次数，如果不存在，返回0
* index(num)
  * 返回num在元组中第一次出现的下标，当元素不存在时，会报错



## 17、集合set

### 17.1 什么是集合

* 集合
  * Python语言内置的数据结构
  * 与列表、字典一样都属于可变序列
  * 集合是没有value的字典
  * `{dataA, dataB, dataC}`
  * 集合中的元素不能重复

### 17.2 集合的创建

* 直接使用花括号
* 使用内置函数set()
  * 也可以用来将其他数据结构（列表，元组，字符串）转换为集合
* 注意：
  * 当定义空集合时，不可以直接使用`k = {}`，这会产生一个空字典
  * 如果要创建空集合，`k = set()`

### 17.3 集合的判断

* `key in set`

### 17.4 集合的新增

* add(element)
  * 向集合添加一个元素element
* update(set)     参数也可以是list
  * 向集合添加set集合的所有元素

### 17.5 集合的删除

* remove(element)
  * 删除element
  * 如果element不存在，会产生KeyError
* discard(element)
  * 删除element
  * 如果element不存在，不会报错
* pop()
  * 删除集合在内存中的第一个元素
* clear()
  * 清空集合

### 17.6 集合的关系

* 集合相等，即集合的元素相同
  * =
  * !=
* 集合是否是子集
  * issubset()
* 集合是否是超集
  * issuperset()
* 集合之间是否**没有**交集
  * isdisjoint()

```python
'''集合和子集和超集'''
s1 = {1, 3, 6, 9}
s2 = {1, 3}
s3 = {1, 6, 7}
print(s2.issubset(s1))
print(s3.issubset(s1))
print(s1.issuperset(s2))
print(s1.issuperset(s3))
'''是否没有交集'''
print(s2.isdisjoint(s3))
```

### 17.7 集合的数学操作-交并差

* 交集
  * intersection()
  * &
* 并集
  * union()
  * |
* 差集
  * difference()
  * -
* 对称差
  * 两个集合的并集去掉它们的交集
  * symmetric_difference()
  * ^

```python
s1 = {3, 5, 7}
s2 = {4, 5, 6}

'''交集'''
print(s1.intersection(s2))
print(s2.intersection(s1))
print(s1 & s2)
'''并集'''
print(s1.union(s2))
print(s1 | s2)
'''差集'''
print(s1.difference((s2)))
print(s1 - s2)
'''对称差'''
print(s1.symmetric_difference(s2))
print(s1 ^ s2)
```

### 17.8 集合的生成式

* `i*i for i in range(6)`

### 17.9 Frozenset不可变集合

* s = frozenset(1, 2, 3)
  * 这以后s不能删除修改增加元素



## 18、列表，字典，元组，集合总结

| 数据结构  | 是否可变 | 是否重复    | 是否有序 | 定义符号    |
| --------- | -------- | ----------- | -------- | ----------- |
| 列表list  | 可变     | 可重复      | 有序     | []          |
| 元组tuple | 不可变   | 可重复      | 有序     | {}          |
| 字典dict  | 可变     | key不能重复 | 无序     | {key:value} |
| 集合set   | 可变     | 不可重复    | 无序     | {}          |



## 19、字符串

字符串与列表的索引访问机制相同，可以使用`str1[1]`访问str1的第二个字符

### 19.1 字符串的驻留机制

* 仅保存一份相同且不可变字符串的方法

* 不同的值被存放在字符串的驻留池中

* Python的驻留机制对相同的字符串只保留一份拷贝，后续创建相同字符串时，不会开辟新空间，而是把该字符串的地址赋给新创建的变量

* 驻留机制的几种情况

  * 字符串的长度为0或1
  * 符合标识符的字符串
  * 字符串只在编译的时候驻留，而不是运行的时候

  ```python
  s1 = "abc"
  s2 = "ab" + "c"
  s3 = "".join(["ab", "c"])
  print(s1 is s2)  # True，驻留机制，s2在编译时就完成了连接
  print(s1 is s3)  # False，不驻留，s3在运行时才确定为abc
  ```

  * [-5, 256]之间的整数

* 注：pycharm对字符串进行了优化处理，基本上对所有的字符串都采用了驻留机制

* sys中的intern方法强制让两个字符串指向同一个对象、

* 字符串驻留机制的优点

  * 当需要值相同的字符串，可以直接从字符串池拿出使用，避免频繁的创建和销毁，提升效率节约内存，因此拼接字符串和修改字符串是会比较影响性能的
  * 在需要进行字符串拼接时，建议使用字符串的join方法，而不是+，因为join方法会先计算出所有的字符串的长度，然后再拷贝，只new一次对象，效率比“+”要高

### 19.2 字符串的查询操作

* index(str)
  * 查找子串str第一次出现的位置，如果不存在，抛出ValueError
* rindex(str)
  * 查找子串str最后一次出现的位置，如果不存在，抛出ValueError
* find(str)
  * 查找子串str第一次出现的位置，如果不存在返回-1
* rfind(str)
  * 查找子串str最后一次出现的位置，如果不存在，返回-1

### 19.3 字符串的大小写操作

* upper()
  * 将字符串转为大写
* lower()
  * 将字符串转为小写
* swapcase()
  * 将大写字母转为小写字母，小写字母转为大写字母
* capitalize()
  * 将第一个字符转为大写，其他变成小写
* title()
  * 将每个单词的第一个字符转为大写，其他变成小写

### 19.4 字符串的对齐操作

* center(length, filler)
  * 居中对齐，第一个参数指定宽度，第二个参数指定填充符，默认是空格，如果设置宽度小于实际宽度则返回原字符串
* ljust(length, filler)
  * 左对齐，第一个参数指定宽度，第二个参数指定填充符，默认是空格，如果设置宽度小于实际宽度则返回原字符串
* rjust(length, filler)
  * 右对齐，第一个参数指定宽度，第二个参数指定填充符，默认是空格，如果设置宽度小于实际宽度则返回原字符串
* zfill(length)
  * 右对齐，左边用0填充，第一个参数指定宽度，如果设置宽度小于实际宽度则返回原字符串

```python
string = "Hello Python"
print(string.center(20, "*"))  # ****Hello Python****
print(string.ljust(20, "*"))  # Hello Python********
print(string.rjust(20, "*"))  # ********Hello Python
print(string.zfill(20))  # 00000000Hello Python
```

### 19.5 字符串的劈分操作

* split()
  * 从字符串的左边开始劈分，默认按照空格，返回值是列表
  * 通过参数sep指定分隔符
  * 通过maxsplit指定劈分的最大次数，一旦达到次数，剩余的子串会单独作为一部分
* splitlines()
  * 按换行符进行划分
* rsplit()
  * 与split唯一的区别就是从字符串右边开始劈分

### 19.6 字符串的判断

* isidentifier()
  * 判断指定字符串是否是合法的标识符
* issapce()
  * 判断指定的字符串是否是全部由空白字符组成（回车，换行，水平制表符）
* isalpha()
  * 判断指定的字符串是否全部由字母（包括汉字）组成
* isdecimal()
  * 判断指定的字符串是否全部由十进制数组成
* isnumeric()
  * 判断指定的字符串是否全部由数字（包括中文数字，罗马数字）组成
* isalnum()
  * 判断指定字符串是否全部由数字或字母组成

### 19.7 字符串的替换和合并

* replace(replacedStr, newStr, times)

  * 第一个参数指定被替换的子串
  * 第二个参数指定替换子串的字符串
  * 第三个参数指定最大替换次数

* 分隔符.join()

  * 参数是列表或元组
  * 将列表或元组的元素通过分隔符连接成一个字符串

  ```python
  print("".join(["Hello", "Java"]))  # HelloJava
  print("*".join(["Hello", "Java"]))  # Hello*Java
  ```

### 19.8 字符串的比较

* 运算符
  * ` > >= < <= == !=  `
* 比较规则
  * 首先比较两个字符串的第一个字符，如果相等则继续比下去，直到两个字符串中的字符不相等时，其比较结果就是两个字符串的比较结果

* 比较原理
  * 两个字符进行比较时，比较的是其ordinal value(原始值)，调用内置函数ord可以得到字符的原始值。与内置函数ord对应的是内置函数chr，调用内置函数chr时，指定ordinal value可以得到其对应的字符
* == 与 is的区别
  * ==比较的是字符串的值
  * is比较字符串是否是同一个对象

### 19.9 字符串的切片

* 原理与列表的切片相同

```python
string = "Hello Python"
print(string[:5])  # 从字符串的开始，到索引为5的地方（不包括5）
print(string[6:])  # 从索引为6的地方开始，到字符串末尾
```

### 19.10 字符串的格式化

* 使用格式化字符串

  * %d表示数字，%s表示字符串，%f表示浮点数
  * %c表示字符  `'%c %c %c' % (97, 98, 99)`
  * %o表示八进制，%x是十六进制
  * %e表示科学计数法表示数
  * %G表示根据值的大小使用%f或%e
  * m.n表示m是显示的最小总宽度，n是小数点后的位数
  * -表示左对齐
  * +表示在正数前面显示加号（+）
  * #o表示在八进制前添加'0o'，#x或#X表示在十六进制前添加'0x'或'0X'
  * 0表示用0替代填充的空格

  ```python
  name = "zhangsan"
  age = 18
  print("我是%s，我今年%d岁。" % (name, age))
  ```

* 使用索引和format函数

  ```python
  print("我是{0}，我今年{1}岁。".format(name, age))
  
  # 指定{}的名字w,c,b,i
  template = 'Hello {w}, Hello {c}, Hello {b}, Hello {i}.'
  world = 'World'
  china = 'China'
  beijing = 'Beijing'
  imooc = 'imooc'
  # 指定名字对应的模板数据内容
  result = template.format(w = world, c = china, b = beijing, i = imooc)
  print(result) # ==> Hello World, Hello China, Hello Beijing, Hello imooc.
  ```

* python3之后可以使用f-string

  ```python
  print(f"我叫{name}，我今年{age}岁。")
  ```

* 精度
  * 格式化字符串%10.3f
    * 表示10位数字，如果不足用空格填充
    * 小数点后占3位
  * format函数{0:.3}
    * 第0个索引的长度为3
  * format函数{0:10.3f}
    * 第0个索引长度为10，小数点后3位

### 19.11 字符串的编码转换

* python3默认使用UTF-8 Unicode进行编码

* 编码
  * 将字符串转化为二进制数据
  * encode(encoding='GBK')
  * 参数用来指定编码的字符集
  * 返回的结果是bytes类型的
* 解码
  * 将二进制数据转为字符串
  * decode(encoding='GBK')
  * 调用的对象的类型是bytes



## 20、函数

### 20.1 什么是函数

* 函数就是执行特定任务并完成特定功能的一段代码
* 函数的作用
  * 复用代码
  * 隐藏实现细节
  * 提高可维护性
  * 提高可读性便于调试

### 20.2 函数的创建

```python
def calc(a, b):
    return a + b
```

### 20.3 函数的参数传递

* 位置实参

  * 根据形参对应的位置进行传递

  ```python
  calc(10, 20)  # a=10, b=20
  ```

* 关键字实参

  ```python
  calc(b=20, a=10)  # a=10, b=20
  ```

* 在函数调用过程中，进行参数的传递
  * 如果是不可变对象，在函数体内的修改不会影响实参的值
  * 如果是可变对象，在函数体内的修改会影响实参的值

### 20.4 函数的返回值

* 如果没有return语句，默认返回None

* 当函数的返回值有多个时，返回的结果呈现在元组中

```python
def fun1(lst):
    odd = []
    even = []
    for i in lst:
        if i%2:
            odd.append(i)
        else:
            even.append(i)
    return odd, even


lst1 = [13, 24, 38, 47, 52, 79, 81]
res = fun1(lst1)  # 结果是元组
print(res[0])
print(res[1])
```

### 20.5 函数的参数默认值

* 在函数的定义时，给形参设置默认值，只有与默认值不符合的情况下需要传递实参

  ```python
  def fun(a, b=10):
      print(a, b)
  ```

* 默认参数需要放在必需参数的后面

### 20.6 函数的可变参数

* 个数可变的位置参数

  ```python
  def fun(*args):
  ```

  * args是一个元组

* 个数可变的关键字参数

  * 关键字参数需要出现在位置参数之后

  ```python
  def fun(**args):
  ```

  * args是一个字典

* 当一个函数的定义既出现可变位置参数，又出现可变关键字参数，要求可变位置参数出现在可变关键字参数之前

```python
def fun_tuple(*args):
    print(args)  # 元组


def fun_dict(**args):
    print(args)


fun_tuple(10, 20, 30)
fun_dict(a=10, b=20, c=30)

def fun_hybrid(*args1, **args2):
    pass        # 这种形式是允许的
```

### 20.7 函数的文档

```python
def fun1():
    '这里写函数的文档，执行函数时文档不会显示也不会运行'
    code block
```

* 可以使用`fun1.__doc__`访问编写的文档
* 可以使用`help(fun1)`，返回函数文档

### 20.8 变量作用域

* 局部变量的范围是在当前函数体或循环体内
* 全局变量的范围是整个模块
* 在函数内修改全局变量在函数外不会生效，因为python会在函数体内修改的时候，使用一个同名的局部变量然后进行修改，所以不会影响全局变量
* 如果想要在局部作用域使用全局作用域的变量， 需要使用`global`关键字，比如`global num`
* 如果想要在内部作用域使用外部作用域的变量，需要使用`nonlocal`关键字，使用方法与global相同

### 20.9 lambda表达式

形式：`lambda 参数列表: 返回值`

```python
g = lambda x : x * 2 + 1
g(5)
```



### 20.10 函数的参数总结

| 序号 | 参数的类型                           | 函数的定义 | 函数的调用 | 备注                           |
| ---- | ------------------------------------ | ---------- | ---------- | ------------------------------ |
| 1    | 位置参数                             |            | √          |                                |
| 2    | 将序列中的每个元素都转换为位置参数   |            | √          | 使用*                          |
| 3    | 关键字实参                           |            | √          |                                |
| 4    | 将字典中的每个键值对都转为关键字实参 |            | √          | 使用**                         |
| 5    | 默认值形参                           | √          |            |                                |
| 6    | 关键字形参                           | √          |            | 使用\*，\*号后的形参使用关键字 |
| 7    | 个数可变的位置形参                   | √          |            | 使用*                          |
| 8    | 个数可变的关键字形参                 | √          |            | 使用**                         |

```python
def fun(a, b, c):
    return a + b + c


lst = [10, 20, 30]
print(fun(*lst))  # 会将列表的元素作为参数
dic = {"a": 10, "b": 20, "c": 30}
print(fun(**dic))  # 将字典的元素最为参数
```



## 21、 异常处理机制

### 21.1 异常结构

* try-多except结构

  ```python
  try:
      num1 = int(input("Please enter the first number:"))
      num2 = int(input("Please enter the second number:"))
      res = num1 / num2
      print(res)
  except ZeroDivisionError: # 当除数为0时，产生此异常
      print("除数不能为0")
  except ValueError:
      print("请输入整数")
  except BaseException:   # 异常的基类
      print("程序有bug")
  print("程序结束")
  ```

* try-except-else结构
  
  * 当try的部分捕捉到异常后会直接执行except，如果没有捕捉到，执行else部分
* try-except-else-finally结构
  
  * finally的部分不管是否出现异常，都会被执行

```python
try:
    a = int(input("请输入被除数:"))
    b = int(input("请输入除数:"))
    res = a / b
except BaseException as e:
    print("出错了", e)
else:
    print(res)
finally:
    print("感谢使用")
```

### 21.2 常见的异常类型

| 序号 | 异常类型          | 描述                          |
| ---- | ----------------- | ----------------------------- |
| 1    | ZeroDivisionError | 除（或取模）零                |
| 2    | IndexError        | 序列中没有此索引              |
| 3    | KeyError          | 映射中没有这个键              |
| 4    | NameError         | 未声明/初始化对象（没有属性） |
| 5    | SyntaxError       | Python语法错误                |
| 6    | ValueError        | 传入无效的参数                |

### 21.3 traceback模块

* 使用traceback的print_exc()方法可以打印异常的信息

```python
import traceback

try:
    res = 1 / 0
except BaseException as e:
    traceback.print_exc()
```

### 21.4 assert

* assert 布尔表达式
  * 如果布尔表达式为假，程序会自动崩溃并抛出AssertionError异常

### 21.5 raise语句

raise ZeroDivisionError

与Java的throw语句作业相同



## 22、类和对象

### 22.1 类的定义

* 类属性

  * 直接写在类的内部，方法之外的变量，所有的对象共享

* 构造方法

  * 不允许返回除了None以外的对象

  ```python
  def __init__(self):
  ```

* 实例方法
  
  * 参数第一个默认是self，如果不传入，会自动放入一个self
  
* 类方法
  * 需要添加注解`@classmethod`
  * 参数第一个是cls，如果不传入，会自动放入一个cls
  
* 静态方法
  
  * 需要添加注解`@staticmethod`
  
* 对象创建
  
  * `实例对象名 = 类对象(参数)`
  * 类对象是工厂函数

```python
class Student:
    native_attribute = "类属性"  # 直接写在class定义里面的是类属性

    # 构造方法
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # 实例方法
    def study(self):  # 实例方法的参数列表第一个为self，如果不显式指定，解释器会自动放入一个self
        print(self.name + "在学习")

    # 静态方法
    @staticmethod
    def eat():
        print("干饭")

    # 类方法
    @classmethod
    def drink(cls):  # 类方法的参数列表第一个为cls，如果不显式指定，解释器会自动放入一个cls
        print("喝水")


stu = Student("zhangsan", 18)
```

* 实例对象的内部会持有一个对类对象的指针

### 22.2 类属性，类方法，静态方法的使用

* 类属性的使用
  * `类对象.类属性`
* 类方法的使用
  * `类对象.类方法`
* 静态方法
  * `类对象.静态方法`

### 22.3 实例对象动态绑定属性和方法

* 动态绑定属性
  * `实例对象.新属性 = 属性值 `
* 动态绑定方法
  * `实例对象.新方法 = 函数`



## 23、面向对象的三大特征

### 23.1 封装

* 提高代码安全性，隐藏代码复杂度
* Python没有定义私有的修饰符，如果不想让外界使用，变量名的前面加两个 `_`（这时直接对象访问会报错）
* 使用dir(实例对象)查看实例对象的所有属性

```python
class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.__age = age

obj = Person("zhangsan", 18)
print(obj.name)
# print(obj.age)  # 无age属性
print(dir(obj))  # 显示obj的所有属性
print(obj._Person__age)  # _Person__age是类中定义的__age
```

### 23.2 继承

* 提高代码的复用性
* 如果一个类没有继承任何类，默认继承object
* Python支持多继承
* 定义子类时，必须在构造方法中调用父类的构造方法

```python
class XiaoMing(Person, Student):  # XiaoMing类继承了Person类和Student类
```

```python
class A(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def info(self):
        print(self.name, self.age)

class B(A):
    def __init__(self, name, age, gender):
        super().__init__(name, age)
        self.gender = gender

b = B("zhangsan", 18, "male")
b.info()
```

多继承

```python
class A(object):
    pass

class B(object):
    pass

class C(A, B):
    pass
```

* 方法重写
  * 用相同的函数签名覆盖掉父类中方法的定义
  * 调用父类的函数`super().xxx()`
* object类
  * 是对象默认的父类
  * 内置函数dir(实例对象)可以查看对象的属性和方法
  * 打印对象，默认调用`__str__()`方法，一般会重写此方法

### 23.3 多态

* 提高代码的可扩展性
* 动态语言的多态对于类型没有要求，只要求对象具有相同的方法（即方法名和参数列表）

```python
class Animal(object):
    def eat(self):
        print("动物吃")
class Cat(Animal):
    def eat(self):
        print("猫吃鱼")
class Dog(Animal):
    def eat(self):
        print("狗吃屎")
class Person:
    def eat(self):
        print("人吃饭")

def eat(animal):
    animal.eat()

eat(Animal())
eat(Cat())
eat(Dog())
eat(Person())  # 即使Person不是Animal类型的，但是它也有eat方法，就可以被调用
```

### 23.4 变量与方法

* 公共变量作为静态变量，如果修改，所有的对象中的都会修改，因为所有对象中的静态变量是引用的类对象的静态变量
* 但如果对象对公共静态变量进行了修改，那么python会在该对象的空间中创建一个同名变量，覆盖静态变量，并赋值为修改后的值，此时对公共静态变量修改不会影响到该对象（因为该对象的作用域内读取的是同名局部变量）
* 如果方法的参数中没有self，则该方法是静态方法（因为python的实例对象在调用方法时会自动将本对象作为参数传入）
* 总结来说，实例化对象调用方法需要方法有参数self

### 23.5 特殊属性

* `__dict__`
  * 是以字典的形式
  * 实例对象的属性
  * 类对象的属性和方法
* `__class__`
  * 对象所属的类
* `__bases__`
  * 父类类型的元组
* `__base__`
  * 父类元组的第一个

* `__mro__`
  * 类的层次结构

### 23.6 特殊方法

* issubclass(class, classinfo)
  * 判断一个类是否是classinfo的子类
  * 对于该方法来说，允许一个类是其自身的子类
  * classinfo可以是类对象组成的元组，只要class是其中任何一个候选类的子类，返回True
  * 如果第二个参数不是类对象，则抛出TypeError

* `__subclasses__()`
  
  * 子类的列表
  
* isinstance(obj, classinfo)

  * 判断一个对象的类型是否是类对象或类对象元组的类型
  * 如果第二个参数不是类对象，则抛出TypeError

* hasattr(obj, name)

  * 判断一个对象是否有指定的name，name为属性

* getattr(obj, name[, default])

  * 获取obj的属性，如果属性不存在，则返回default参数，如果default也不存在，则抛出AttributeError

* `__getattr__(self, name)`

  * 定义当用户试图获取一个不存在的属性时的行为

* `__getattribute(self, name)`

  * 定义该类的属性被访问时的行为

* setattr(obj, name, value)

  * 将obj的属性设置为value，如果属性不存在，则创建

* `__setattr(self, name, value)`

  * 当设置对象的属性的时候被触发

* delattr(obj, name)

  * 删除obj的指定属性
  * 如果要删除的属性不存在，则抛出AttributeError

* property（fget=None, fset=None, fdel = None, doc=None)

  ```python
  class A:
  	def __init__(self, size):
          self.size = size
      def getSize(self):
          return self.size
      def setSize(self, size):
          self.size = size
      def delSize(self):
          del self.size
          
      x = property(getSize, setSize, delSize)
  
  a = A()
  a.x = 1 # a.size = 1
  print(a.x)  # print(a.size)
  del a.x  # del a.size
  ```

* `__add__(self, other)`
  
  * 魔法方法
  * 对象的加法运算（+底层调用此方法，`a+b`实际上是`a.__add(a, b)`）
  
* `__radd__(self, other)`
  
  * 反运算
  * 对于`a+b`，如果a没有定义`__add__(self, other)`方法，而b定义了`__radd__(self, other)`，则执行此方法
  
* `__isadd__(self, other)`

  * 增量运算
  * 当执行`+=`，底层调用此方法

* 一元运算符

  * `__neg__(self)`
    * `+x`
  * `__pos__(self)`
    * `-x`
  * `__abs__(self)`
    * 当`abs`方法被调用的时候
  * `__invert__(self)`
    * `~x`

* `__len__()`
  
  * len(对象)的底层调用此方法

```python
class Str:
    def __init__(self, name):
        self.name = name

    def __add__(self, other):
        return self.name + other.name


s1 = Str("zhangsan")
s2 = Str("lisi")
res = s1 + s2
print(res)  # zhangsanlisi
```

### 23.7 魔法方法（包括构造函数）

* `__new__(cls, args)`和`__init__`(self, args)

  * new方法返回一个cls的类的实例对象
  * args会传递给init方法
  * `__init__`不需要返回值

* ```python
  stud = Student()
  # 该语句创建了一个Student的实例对象stud
  # 首先解释器调用Student类对象的__new__()方法，该方法会调用object的__new__()方法，返回一个对象
  # 新创建的对象传入__init__()，进行初始化
  ```

* `__del__`
  * 析构函数
  * 对象被删除（被释放内存）时会被调用，即垃圾回收器会调用

### 23.8 类的浅拷贝与深拷贝

* 变量的赋值操作
  * 只是形成两个变量，实际上还是指向同一个对象
* 浅拷贝
  * Python拷贝一般都是浅拷贝，拷贝时，对象包含的子对象内容不拷贝，因此源对象与拷贝对象会引用同一个子对象
  * 使用copy模块的copy函数
* 深拷贝
  * 使用copy模块的deepcopy函数，递归拷贝对象中包含的子对象，源对象和拷贝对象所有的子对象不相同

## 24、模块Modules

### 24.1 模块

* 模块包含
  * 函数
  * 类
  * 语句
* 使用模块的好处
  * 方便其他程序和脚本的导入并使用
  * 避免函数名和变量名冲突
  * 提高代码的可维护性
  * 提高代码的可重用性

### 24.2 模块的创建和导入

* 一个py文件就是一个模块
* 自定义的模块尽量不要和系统自带的相同
* 导入模块
  * `import 模块名称 [as 别名]`     推荐
  * `from 模块名称 import 函数/变量/类`
    * 只有`__all__`属性中的内容会被导入
    * 但没有`__all__`属性，则会导入模块的所有

```python
import 模块A_calc
res = 模块A_calc.add(1, 3)
print(res)
```

```python
import module_a.module1 as m1
t = m1.Test()

from module_a.module1 import * 
t = Test()
```

### 24.3 作为主程序才运行

在每个模块的定义中都包括一个记录模块名称的变量`__name__`,程序检查该变量，以确定它们在哪个模块中运行。

如果一个模块不是被导入到其他程序中执行，它有可能在解释器的顶级模块中执行。顶级模块的`__name__`变量是`__main__`

```python
if __name__ == "__main__":
    print("当前在主程序中运行")
```

### 24.4 Python中的包

* 包是一个分层级的目录结构，它将一组功能相近的模块组织在一个目录下
* 作用
  * 代码规范
  * 避免模块名称冲突
* 包与目录的区别
  * 包中含有`__init__.py`文件的目录称为包
  * 目录里通过不含`__init__.py`这个文件
* 包的导入
  * `import 包名.模块名  `

### 24.5 Python中常用的模块

* sys
  * 与Python解释器及其环境操作相关的标准库
  * getsizeof(obj)  :获取对象的内存大小
* time
  * 提供了与时间相关的各种函数的标准库
  * time()  :返回当前的事件的总秒数
  * localtime
* os
  * 提供了访问操作系统服务的标准库
* calendar
  * 提供与日期操作相关的各种函数的标准库
* urllib
  * 用于读取来自网上（服务器）的数据标准库
  * urllib.request
    * urlopen(url)
* json
  * 用于使用JSON序列化和反序列化对象
* re
  * 用于在字符串中执行正则表达式匹配和替换
* math
  * 提供标准算术运算函数的标准库

* decimal
  * 用于进行精确控制运算精度、有效数位、和四舍五入操作的十进制运算
* logging
  * 提供了灵活的记录事件、错误、警告和调试信息等日志信息的功能

### 24.6 第三方模块的安装——在线安装

* 在线安装

  * `pip install 模块名 `

  ```python
  # 先安装schedule模块
  # pip install schedule
  import schedule
  
  def job():
      print("哈哈哈哈")
  
  schedule.every(3).seconds.do(job)  # 每3秒执行一次job函数
  while True:
      schedule.run_pending()
  ```



## 25、文件

### 25.1 编码格式

* Python解释器使用Unicode（内存）
* .py文件使用UTF-8（外存）

* 修改python文件的编码格式

  * 在文件的第一行加上以下注释

  ```python
  #encoding=gbk
  ```

* 操作的文件默认编码格式是GBK

### 25.2 文件读写

* 操作
  * 打开或新建文件
  * 读写文件
  * 关闭资源
* open(filename[, mode, encoding])
  * 打开或创建文件
  * 返回一个文件对象
* 常见的mode
  * r
    * read，只读
  * w
    * write，只写，指针在文件开头，文件不存在会创建
  * a
    * append，追加，指针在文件末尾，文件不存在会创建
  * b
    * 以二进制方式打开文件
  * +
    * 读写方式，不能单独使用，与其他模式一起使用

### 25.3 文件对象的常用方法

* read([size])
  * 从文件中读取size个字节或字符的内容返回。如果size不指定，默认读到文件末尾
* readline()
  * 从文本文件中读取一行内容
* readlines()
  * 把文本文件中每一行都作为一个字符串对象，并将这些对象放入列表返回
* write(str)
  * 将str写入文件
* writelines(s_list)
  * 将字符串列表s_lists写入文件，不添加换行符
* seek(offset[, whence])
  * 把文件指针移动到新的位置上，offset表示相对于whence的位置
  * offset
    * 正：往结束方向移动
    * 负：往开始方向移动
  * whence不同的值代表不同的含义
    * 0：文件开始（默认值）
    * 1当前位置
    * 2文件末尾

* tell()
  * 返回文件指针的当前位置
* flush()
  * 把缓冲区内容写入文件，但不关闭文件
* close()
  * 关闭文件同时将缓冲区内容写入文件

### 25.4 With语句

```python
with open("a.txt", "w", encoding="UTF-8") as src_file:
    src_file.write("你好")
```

* with语句的好处：
  * 其语句体内可以自动管理上下文资源，不论什么原因跳出with块，都能确保文件正确的关闭，以此来达到释放资源的目的

* `open("a.txt", "w", encoding="UTF-8)`
  * 此为上下文表达式
  * 会返回一个上下文管理器
  * 该上下文管理器实现了`__enter()__`和`__exit__`方法，这两个方法会自动被系统调用
  * 离开运行时上下文时，自动调用的上下文管理器的`__exit__`

* `as src_file`
  * 可选项
  * 将调用的`__enter__`的返回值自动赋值给src_file

```python
with open("a.txt", "w", encoding="UTF-8") as src_file:
    src_file.write("你好")
```

自定义的上下文管理器

```python
class MyContextMG(object):

    def __enter__(self):
        print("enter is invoked")

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("exit is invoked")


with MyContextMG() as src_source:
    pass

# 运行结果
# enter is invoked
# exit is invoked
```

### 25.5 OS模块相关用法

* 打开并执行可执行文件

```python
import os
# 对于直接在环境变量中的程序，可以直接通过os.system(exe名称)进行调用
# os.system("notepad.exe")  # 打开记事本
# os.system("calc.exe")  # 打开计算器

# 打开非环境变量下的可执行文件
os.startfile(r"C:\Program Files (x86)\Tencent\QQ\Bin\QQ.exe")
```

* 常用函数

  * getcwd()
    * 获取程序运行时当前的工作目录
  * listdir(path)
    * 返回指定目录下的文件和目录信息
  * mkdir(path [, mode])
    * 创建目录
  * makedirs(path1/path2/path3/...)
    * 创建多级目录
  * rmdir(path)
    * 删除目录
  * removedirs(path1/path2/path3/...)
    * 删除多级目录
  * chdir(path)
    * 修改程序当前运行工作目录为path

  ```python
  import os
  
  print(os.getcwd())
  lst = os.listdir(os.getcwd())
  print(lst)
  
  # os.mkdir("new_dir")
  # os.rmdir("new_dir")
  # os.makedirs("a/b/c")
  # os.removedirs("a/b/c")
  os.chdir("../day06")
  print(os.getcwd())
  ```

  * walk(path)
    * 返回一个迭代器对象
    * 遍历top参数指定路径下的所有子目录，并将结果返回一个三元组（路径，[目录]，[文件]）

  ```python
  import os.path
  
  obj = os.walk("D:\\pycharm\\code\\study\\day07")
  print(obj)  # 迭代器对象（里面有该路径下的所有目录及文件）
  # 多个元组对象
  # 第一个是路径名di'r'path
  # 第二个是路径名下的子目录
  # 第三个是路径名下的文件
  for item in obj:
      print(item)
  # ('D:\\pycharm\\code\\study\\day07', ['sub'], ['a.txt', 'demo1_with.py', 'demo2_with2.py', 'demo3_os模块1.py', 'demo4_os方法.py', 'demo5_os方法2.py', 'demo6_列出指定目录下的所有python文件.py', 'demo7_walk.py'])
  # ('D:\\pycharm\\code\\study\\day07\\sub', [], ['b.txt'])
  ```

* 常量
  * curdir
    * 当前目录
  * pardir
    * 上一级目录
  * sep
    * 输出操作系统特定的路径分隔符（Win是'\\\\\',Linux是'/'）
  * linesep
    * 当前平台使用的行终止符（Win是'\r\n',Linux是'\n'）
  * name
    * 指代当前的操作系统
    * 'posix': Unix
    * 'nt': Windows
    * 'mac'
    * 'os2'
    * 'ce'
    * 'java'

### 25.5 os.path模块相关用法

* abspath(path)
  * 用于获取文件或目录的绝对路径
* exists(path)
  * 判断文件或目录是否存在，存在返回True
* join(path, name)
  * 将目录与文件名拼接起来，以符合当前操作系统的方式
* split()
  * 分离路径和文件名
* splitext()
  * 分离文件名和扩展名
* basename(path)
  * 从一个目录中提取文件名
* dirname(path)
  * 从一个路径中提取文件路径，不包括文件名
* isdir(path)
  * 判断是否为路径
  * 如果直接判断会有bug，最好判断绝对路径
* getsize(file)
  * 返回文件大小，单位是byte

### 25.6 图片的复制

```python
import shutil
shutil(old_path, new_path)
```



## 26、序列

序列包括列表list，元组tuple以及字符串

都可以通过索引进行访问

* max：求最大值（要求序列中的元素是相同类型）
* min
* sum(iterable[, start]):求start和iterable所有元素的和
* sorted(iterable)
* reversed(iterable)
* enumerate(iterable)：生成一个列表，元素是元组类型，元组的第一个是索引，第二个是该索引对应的元素值
* zip(iterable_a, iterable_b)：生成一个列表，元素是元组类型，第一个是a的元素，第二个是对应索引的b的元素（长度等于a和b中小的那个）



## 27、filter

filter(function, iterable)	将iterable的每一个元素作为参数依次调用function，如果返回值为false，则过滤掉

## 28、map

map(function, iterable)	将iterable的每一个元素都调用一次function，然后将所有的返回值返回

## 29、pickle模块

```python
import pickle
my_list = [123, 3.14, '小甲鱼', ['another list']]
# 创建一个文件，模式为二进制写入（必需）
pickle_file = open('my_list.pkl', 'wb')
# 使用dump方法将数据保存到文件中
pickle.dump(my_list, pickle_file)
pickle_file.close()

pickle_file = open('my_list.pkl', 'rb')
# 使用load方法将数据读取
my_list2 = pickle.load(pickle_file)
pickle_file.close()
```

## 30、time模块

* localtime

  * 返回当前时区的时间的struct_time格式

    | 索引值 | 属性                    | 值                       |
    | ------ | ----------------------- | ------------------------ |
    | 0      | tm_year                 | eg. 2015                 |
    | 1      | tm_mon                  | 1~12                     |
    | 2      | tm_day                  | 1~31                     |
    | 3      | tm_hour                 | 0~23                     |
    | 4      | tm_min                  | 0~59                     |
    | 5      | tm_sec                  | 0~59                     |
    | 6      | tm_wday(星期几)         | 0~6（0是星期一）         |
    | 7      | tm_yday(一年当中第几天) | 1~366                    |
    | 8      | tm_isdst(是否是夏令时)  | 0，1，-1（-1代表夏令时） |

* perf_counter()
  
  * 返回计时器的精准时间（系统的运行时间）
* process_time()
  
  * 返回当前进程执行CPU的时间总和

### 30.1 获取时间戳

```python
import datetime
dt = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
```



## 31、timeit模块

* `import timeit`
* timeit.timeit(语句, number=1000000)
  * 返回该语句执行number次的时间
* t1 = Timer(语句, 执行一次的安装语句)
  * t1.timeit(number=10000)



## 32、描述符Descriptor

* 描述符就是将某种特殊类型的类的实例指派给另一个类的属性
* 注：描述符需要定义在类层面上，而不能定义在实力层面上
* 这种特殊类型的类需要实现以下三个方法
  * `__get__(self, instance, owner)`
    * 用于访问属性，返回属性的值
    * self是该特殊类型的类（即描述符）
    * instance是描述符描述的类的对象
    * owner是描述符描述的类
  * `__set__(self, instance, value)`
    * 将在属性分配操作中调用，不返回任何内容
  * `__delete__(self, instance)`
    * 控制删除操作，不返回任何内容

```python
class MyDescriptor:
    # self是描述符自身的实例；instance是这个描述符的拥有着所在类的实例，在这里也就是Test类的实例；owner是这个描述符的拥有者所在的类本身
    def __get__(self, instance, owner):
        print("getting...", self, instance, owner)
    # 参数value是等号右边的值，就是下面的'X-man'
    def __set__(self, instance, value):
        print("setting...", self, instance, value)
    def __delete__(self, instance):
        print("delete...", self, instance)

        
class Test:
    # x是一个描述符，对x进行修改会触发MyDescriptor的描述符方法
    # 该描述符定义在类层面上
    x = MyDescriptor()

    
>>> test = Test()
>>> test.x
getting... <__main__.MyDescriptor object at 0x000001A2EA6D86D8> <__main__.Test object at 0x000001A2EA668278> <class '__main__.Test'>
>>> test
<__main__.Test object at 0x000001A2EA668278>
>>> Test
<class '__main__.Test'>
>>> test.x = 'X-man'
setting... <__main__.MyDescriptor object at 0x000001A2EA6D86D8> <__main__.Test object at 0x000001A2EA668278> X-man
>>> del test.x
delete... <__main__.MyDescriptor object at 0x000001A2EA6D86D8> <__main__.Test object at 0x000001A2EA668278>
>>>
```





## 33、定制容器

* 不可变容器
  * 实现`__len__(self)`和`__getitem__(self, key)`
  * `__len__(self)`
    * 定义当被len()方法调用时的行为
  * `__getitem__(self, key)`
    * 定义获取容器中指定元素的行为，相当于`self[key]`

* 可变容器
  * 在不可变容器的基础上，实现`__setitem__(self, key, value)`和c
  * `__setitem__(self, key, value)`
    * 定义设置容器中指定元素的行为，相当于self[key] = value
  * `__setitem__(self, key, value)`
    * 定义删除容器中指定元素的行为，相当于`del self[key]`
* 容器还可以实现的方法
  * `__iter__(self)`
    * 定义当迭代容器中的元素的行为
  * `__reversed__(self)`
    * 定义当被reversed()调用时的行为
  * `__contains__(self, item)`
    * 定义当使用成员测试运算符(in或not in)的行为



## 34、迭代器

* iter()
  * 返回容器的一个迭代器
* next()
  * 返回迭代器的下一个元素，如果到最后，抛出异常StopIteration
* `__iter__`
  * 定义当iter()方法被触发时的行为
* `__next__`
  * 定义当next方法被触发时的行为

```python
n = 0
while n < 5:
    print(n)
    n += 1
# 或者用迭代器的方式
it = iter(range(0, 5))
while True:
    try:
        print(next(it))
    except StopIteration:
        break
```



## 35、生成器generator

* 生成器的本质是一个协同程序，就是可以运行的独立函数调用，函数可以暂停或者挂起，并在需要的时候从程序离开的地方继续或者重新开始

* 其实也可以理解为生成器函数调用返回一个特殊的迭代器

```python
>>> def my_gen():
	for i in range(10):
		yield i

		
>>> r = my_gen()
>>> next(r)
0
>>> next(r)
1
>>> next(r)
2
>>> 
```

### 35.1 生成器推导式

`(i for i in range(100) if i % 2)`



## 36、datetime模块

* datetime.date
  * 是一个对象
* datetime.date.today()
  * 是一个日期元组
  * 返回今天的年月日
  * 例如`datetime.date(2021, 3, 17)`
* datetime.date.today().year
  * 返回今天所在的年份



## 37、sys模块

* `models`
  * 字典数据结构，包含了从Python开始运行起，被导入的所有模块。键就是模块名，值就是模块对象
  * `sys.models[__name__]`

### 37.1 搜索路径

```python
import sys
sys.path  # 系统路径
sys.path.append(filePath)  # 添加新路径到系统路径
```



## 38、爬虫

### 38.1 第一个爬虫

```python
import urllib.request
response = urllib.request.urlopen("https://www.baidu.com")
# 获取网页的二进制文件
html = response.read()
# 获取网页的文本内容
htmlcontent = html.decode("utf-8")

---------------------
---------------------

import urllib.request

req = urllib.request.Request("http://placekitten.com/g/200/300")
resp = urllib.request.urlopen(req)
htmlbinary = resp.read()
with open("C:\\Users\\zhanguozhi\\Documents\\cat_200_300.jpg", "wb") as f:
    f.write(htmlbinary)
```





## 39、如何学习使用一个模块

1. 首先是IDLE的菜单栏的Help中的Python Docs
2. 使用dir()函数查询该模块定义了哪些函数和变量
3. 有些模块有`__all__`属性，指出该模块有哪些可以使用的变量或函数
4. `__file__`指明了该模块的源代码位置
5. `help()`函数



## 40 python操作Mysql数据库

### 查询数据

```python
from pymysql import *

def main():
    # 创建connection连接
    conn = connect(host='localhost', port=3306, user='root', password='mysql', database='jingdong', charset='utf8')
    # 获取Cursor对象
    cursor = conn.cursor()
    # 执行select语句，并返回受影响的行数，查询一条数据
    count = cursor.execute('select id, name from goods where id >= 4')
    # 打印受影响的行数
    print("查询到的%d条数据：" % count)
    
    for i in range(count):
        # 获取查询的结果(类型为元组)
        res = cursor.fetchone()
        print(res)
    # 关闭Cursor对象
    cursor.close()
    # 关闭connection
    conn.close()
```

* cursor.fetchone()
  * 返回结果集的一行数据
* cursor.fetchmany(num)
  * 返回结果集的num行数据
* cursor.fetchall()
  * 返回结果集的所有数据

### 增删改数据

```python
from pymysql import *

def main():
    # 创建connection连接
    conn = connect(host='localhost', port=3306, user='root', password='mysql', database='jingdong', charset='utf8')
    # 获取Cursor对象
    cursor = conn.cursor()
    # 执行insert语句，并返回受影响行数；添加一条数据
    count = cursor.execute('insert into goods_cates(name) values ('硬盘')')
    # 打印受影响行数
    print(count)
    
    # 更新
    # count = cursor.execute('update goods_cates set name="机械硬盘" where name="硬盘"')
    # 删除
    # count = cursor.execute('delete from goods_cates where id=6')
    
    # 提交之前的操作
    # 如果之前进行了多次execute，那么都进行提交
    conn.commit()
    
    # 关闭Cursor对象
    cursor.close()
    # 关闭connection
    conn.close()
```

### SQL注入

```python
# 安全的方式,将数据通过列表进行传入可以避免sql注入
params = [good_name, good_age]
count = cursor.execute("select * from goods where name=%s and age=%d", params)
```



## 41 Json

数组 -> json

```
import json
a = [1, 2, 3, 4, 5]
a_json = json.dumps(a, ensure_ascii=False)
```

json -> 数组

```python
import json
a_json = '[1, 2, 3, 4]'
a = json.loads(a_json)
```



## 42 big-o

`pip install big-O`

估算一个函数的大O数量级

```python
big_o(
	func, # 需要估计大0数量级的算法函数，1个输入参数
	data_generator, #能产生输入参数的函数，以N为参数
    min_n, max_n, n_measures,  # 最小N，最大N，取多少个N
    n_repeats,  # 重复执行多少次func，来计算执行时间
    n_timings,  # 重复测量多少次，保留最好的测量结果
)
返回值：(best_class, fitted)  最佳拟合的复杂度，以及其他拟合信息
```

**一些通用的data_generator**

* big_o.datagen.n_(N)  
  * 返回参数N
* big_o.datagen.integer(N, min, max) 
  * 返回N个随机整数list
* big_o.datagen.range_n(N)
  * 返回参数N的range(N)列表list

## 43 函数值缓存 lru_cache装饰器

* 来自functools模块的lru_cache装饰器
* 对产生大量重复计算的递归函数自动提供函数值装饰器

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n == 1:
        return 0
    elif n == 2:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

## 44 散列函数库hashlib

* Python 自带MD5和SHA系列的散列函数库：hashlib
* 包括了md5、sha1、sha224、sha256、sha384、sha512等6种散列函数

```python
import hashlib
hashlib.md5('hello world!').hexdigest()
hashlib.sha1('hello world!').hexdigest()
```

* 除了对单个字符串进行散列计算之外，还可以使用update方法对任意长的数据进行分部分计算，避免内存不足

```python
import hashlib
m = hashlib.md5()
m.update('hello world!')
m.update('this is part #2')
m.update('this is part #3')
m.hexdigest()
```

