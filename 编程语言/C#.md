# C#

## .Net Framework

分为

* CLR （公共语言运行时）
* .Net类库

C#可以开发基于.net平台的应用

### 应用

* 桌面应用程序
  * Winform应用程序
* Internet应用程序
  * ASP.NET
* 手机开发
  * Wp8
* Unity3D游戏开发

### 两种交互模式

* C/S 客户机/服务器模式
* B/S 浏览器/服务器模式

## cs文件结构

![](D:\大学\笔记\编程语言\c#_pic\1.png)

### 基本语法

> * C#是大小写敏感
> * 所有语句表达式以分号结尾
> * 与Java不同的是，文件名可以不同于类的名称

#### 控制台输入输出

输入：

```c#
string variable = Console.ReadLine();
```

输出

```c#
Console.WriteLine();
```

#### 注释

* 单行注释
  * `//`
* 多行注释
  * `/* 多行注释的内容  */`
* 文档注释
  * `///`

### 数据类型

* 整数类型 int
* 浮点数类型 double
  * 15到16位
* 高精度小数  decimal
  * 28到29位
  * 如果需要指定实数是decimal类型，需要添加后缀m或M
* 字符串 string
  * 在字符串之前添加一个@符号
    * 可以取消`\`符号的转义作用
    * 字符串按照编辑原格式输出
* 字符  char
* 布尔类型  bool
  * True和False

注：

1. 互相兼容的类型之间的转换使用隐式类型转换或者显式类型转换

2. 互相不兼容的类型使用Convert转换工厂转换

   * 但是如果数据不符合，会抛出运行时异常

   ```c#
   string s = "123";
   int num = Convert.ToInt32(s);
   ```

   * 本质上是调用int.Parse(string s), char.Parse(string s)
   
3. 还可以使用int.TryParse(string s)等TryParse函数

   ```c#
   int number = 0;
   // 如果正常转换，b为True
   // 如果错误转换，b为Flase，但不会抛出异常
   // 转换的结果在out后面的参数中
   bool b = int.TryParse("123", out number);
   ```

将数字num保留两位小数，`num.ToString("0.00");`

`GetType()`方法可以获取变量的类型

注2：

数据类型可以使用`var`关键字进行声明，它会根据值来推断变量的数据类型

但是`var`需要在声明变量的时候，就对变量进行初始化

### 变量

#### 命名规则

必须是字母或下划线或@符号开头，不可以用数字开头

* 给变量或字段命名时，使用Camel驼峰命名规则
* 给类或方法命名时，使用Pascal命名规则
  * 每个单词的首字母大写

#### 占位符

```c#
string name = "张三";
char gender = '男';
int age = 18;
string tel = "18916063629";
Console.WriteLine("我叫{0}，我今年{1}岁了，我是{2}，我的电话是{3}", name, age, gender, tel);
```

占位符的顺序按照数字的顺序

{1:0.00}  表示第二个占位符保留两位小数

#### 三元表达式

关系表达式 ? val_1 : val_2

注意：val_1与val_2的结果类型必须一致

### 常量const

```c#
const 变量类型 变量名 = 变量值;
```

### 枚举enum

```c#
[public] enum 枚举名 {
    值1=num,
    值2,
    值3,
    ...
}
```

枚举类型的值可以与int类型的值相互转换（强制类型转换）

默认的转换后的值是从0开始，每个枚举值可以设置它转换后的值

int类型如果不能转换为枚举类型，则保留其原值

枚举与字符串的转换

枚举类型调用其ToString方法转换为字符串

字符串转为枚举使用`Enum.Parse(typeof(enum), string value)`

```c#
QQState state = (QQState)Enum.Parse(typeof(QQState), input);
```

```c#
public enum Gender {
    male,
    female
}

Gender gender = Gender.male;
```

### 结构struct

```c#
[public] struct Person {
    public string _name;  // 字段
    public int _age;
    public Gender _gender; // 枚举
}

Person zsPerson;
zsPerson._name = "张三";
```

### 数组

数组类型[] 数组名 = new 数组类型[数组长度];

```c#
int[] arr = new int[10]; 

int[] arr_2 = {1, 2, 3, 4, 5};

int[] arr_3 = new int[] {1, 2, 3};
```

int类型的数组初值为0，string类型的数组初值为null，bool类型的数组是False

**排序**

```c#
Array.Sort(nums);  // 将数组升序排列
Array.Reverse(nums);  // 将数组反转
```

### 条件结构

if else-if结构

```c#
if (表达式1) {
    pass
} else if (表达式2) {
    pass
} else {
    pass
}
```

switch-case结构

```c#
switch (变量或表达式的值) {
    case val_1:
        执行的代码;
        break;
    case val_2:
        执行的代码;
        break;
    ...
    default:
    	要执行的代码;    	
}
```

变量或表达式的值需要与val的类型匹配

### 循环结构

break

continue

#### while循环

```c#
while (循环条件) {
    循环体;
}
```

#### do while循环

```c#
do {
    循环体;
} while (循环条件);
```

#### for循环

```c#
for (int i = 0; i < 10; i++) {
    循环体
}
```

#### foreach循环

```c#
foreach (var item in collection) {
    
}
```

### 异常捕获

异常都是语法没有错误，但是运行时产生的异常

```c#
try {
    
} catch {
    捕获到异常时的行为
}
```

### 函数

#### out参数

```c#
public static void Test(nums, out int max, out int min) {
    max = nums[0];
    min = nums[0];
}

Test(nums, out max, out min);
```

out参数必须在方法内赋值

#### ref参数

用法与out参数基本一致

ref参数必须在方法外赋值

#### params可变参数

params参数必须是形式参数列表中的最后一个

```c#
public static void Test(name, params int[] score) {
    
}

Test("zhangsan", 98, 88, 76);
```

#### 方法重载

方法名称相同，但参数不同（类型或者个数）

与返回值无关

## 面向对象

### 类

* 字段 fields
  * 类中字段的默认是private
* 属性 properties
  * 属性的作用就是保护字段，对字段的赋值和获取进行限定
  * 属性的本质就是两个方法，一个是get()，另一个是set()
  * 当要获取属性的值，会自动调用get()方法
  * 当要设置属性的值，会自动调用set()方法
* 方法 methods

```c#
public class Person {
    public string _name;  // 字段
    
    public string Name {
        get { return _name;}
        set { this._name = value;}  // value是set方法的参数
    } // 属性
}
```

静态类只能有静态成员，且不能创建对象

静态类存储在静态存储区，在整个项目中被资源共享。只有在程序结束之后，静态类才会释放资源

#### ToString

每一个类都有一个ToString方法，默认是打印类的命名空间

### this关键字

1. 指向本类对象
2. 显式地调用构造函数

```c#
public Person(string name, int age, char gender) {
    
}

public Person(string name):this(name, 0, '男') {
    
}
```

### 析构函数

在程序结束的时候才运行，帮助释放资源

由于GC不能立即释放资源，有些时候会将释放资源的代码放入析构函数

析构函数名为`~类名`

```c#
~Student() {
    
}
```

### 命名空间

`using 命名空间;`

**在一个项目中引用另一个项目**

1. 添加引用
2. 引用命名空间

### 值类型与引用类型

值类型：int、double、decimal、bool、char、struct、enum

引用类型：string、自定义类、数组

值类型的值是存储在内存的**栈**中

引用类型是存储在内存的**堆**中

### 字符串

字符串的不可变性

可以看作一个char类型的只读数组(通过索引进行访问)

```c#
string s = "abcd";
char ch = s[0];
```

#### 属性

* Length
  * 获取字符串的长度

#### 方法

* char[] ToCharArray()

  * 转换为char类型的数组

* new string(char[] chs)

* ToUpper()

  * 将字符串转换为大写

* ToLower()

  * 将字符串转换为小写

* Equals(string s, StringComparison comparisonType)

  * 默认比较两个字符串是否相同，可以指定comparisonType指定如何比较字符串
  * StringComparison.OrdinalIgnoreCase  忽略大小写

* string[] Split(params char[] separator)

  * 将字符串按照指定的分隔符分割为字符串数组，分隔符会变成""

* string[] Split(char[] separator, StringSplitOptions options)

  * 当options取RemoveEmptyEntries的时候移除结果中的空白字符串

* string Replace(string oldValue, string newValue)

  * 将字符串中oldValue的地方替换为newValue的地方

* string Substring(int startIndex)

  * 取从startIndex位置开始到最后的子字符串

* string Substring(int startIndex, int length)

* bool Contains(string value)

  * 判断字符串中是否包含子串value

* bool StartsWith(string value)

  * 判断字符串中是否以子串value开始

* bool EndsWith(string value)

* int IndexOf(string value)

  * 取子串value在字符串中第一次出现的位置，找不到，则返回-1

* int IndexOf(string value, int startIndex)

  * 取子串value在字符串的startIndex索引后第一次出现的位置	

* LastIndexOf(string value)

  * 取子串value在字符串中最后一次出现的位置

* Trim()

  * 移除字符串两端所有的空格

* TrimEnd()

* TrimStart()

* static IsNullOrEmpty()

  * 判断字符串是否为空或""

* static Join(string s, params object[] objs)

  * 按照连接符s将object的字符串形式连接起来

  ```c#
  string[] names = {"张三", "里斯"};
  string str_1 = string.Join("|", names);
  string str_2 = string.Join("|", "张三", "里斯");
  ```

  

### StringBuilder

```c#
using System.Text;

StringBuilder sb = new StringBuilder();
sb.append("i");
string s = sb.ToString();
```

### 继承

```c#
public class Person {
	private string _name;
    public string Name {
        get {return _name;}
        set {_name = value;}
    }
}

public class Student : Person {
    
}
```

* 子类可以继承父类的公有属性和方法

* 一个类只有一个父类（单继承）

* 继承的传递性（子类可以继承父类以及父类的父类的属性和方法）

* 子类没有继承父类的构造函数，但是，子类默认会调用父类无参数的构造函数创建父类对象，让子类可以使用父类中的成员

* 显式地调用父类的构造函数`:base();`

  ```c#
  public Student(string name, int age, char gender):base(name, age) {
  	this.Gender = gender;    
  }
  ```

* object默认是所有类的基类

#### 方法覆盖

子类的方法默认会覆盖父类的同名方法（这种做法会产生警告，最佳方式是在子类的同名方法上添加new关键字）

```c#
public class Person() {
    public void sayHello() {
        Console.WriteLine("你好，我是人");
    }
}

public class Student() {
    public new void sayHello() {
        Console.WriteLine("你好，我是学生");
    }
}
```

#### 里氏替换

* 子类对象可以赋值给父类变量

* 如果父类变量指向的是子类对象，可以将这个对象强转为子类对象

* is

  * 类型转换，如果可以转换，返回True

  ```c#
  Person p = new Student();
  if (p is Teacher) {
      Console.WriteLine("转换成功");
  } else {
      Console.WriteLine("转换失败");
  }
  ```

* as

  * 类型转换，如果转换成功，返回转换的对象，如果失败，返回Null

  ```c#
  Teacher t = p as Teacher;
  ```

### 多态

**实现多态的三种手段**

* 虚方法
* 抽象类
* 接口

#### 虚方法

步骤：

1. 将父类的方法标记为虚方法，使用关键字virtual
2. 子类的对应方法添加override关键字

## 装箱和拆箱

装箱：把值类型转换为引用类型

拆箱：把引用类型转换为值类型

如果两种类型之间存在继承关系，则相互转换可能发生装箱或者拆箱

## 模块

### Random

```c#
int low = 1;
int high = 10;
Random r = new Random();
int random_num = r.Next(low, high);  // [low, high)
```

### 计时器 Stopwatch

```c#
using System.Diagnostics;

Stopwatch sw = new Stopwatch();  // 创建一个计时器
sw.start(); // 开始计时
sw.stop(); // 结束计时
Console.WriteLine(sw.Elapsed);  // 输出计时时间
```

### File

是一个静态类

```c#
using System.IO;

string path = @"C:\1.txt";
string lines = File.ReadAllLines(path);
```

### 集合

#### ArrayList

```c#
using System.Collections;

ArrayList list = new ArrayList();
```

##### 属性

* Count
  * ArrayList中的元素个数
* Capacity
  * ArrayList中可以包含的元素个数（每次申请当前容量的一倍）

##### 方法

* Add(object value)
  * 添加
* AddRange(ICollection c)
  * 将集合c中的元素依次添加到ArrayList的末尾
* Insert(int index, object value)
  * 在指定的index处插入value
* insertRange(int index, ICollection c)
  * 在指定的index处插入一个集合
* clear()
  * 移除所有元素
* Remove(object value)
  * 移除value的第一个匹配项
* RemoveAt(int index)
  * 移除索引为index的数据项
* RemoveRange(int index, int count)
  * 移除索引为index开始的count个元素
* Reverse()
  * 反转元素
* Contains(object value)
  * 是否包含某个指定的元素

访问元素与数组相同，直接使用索引值进行访问

#### 不使用ArrayList的原因

因为ArrayList存储的是object类型，会发生装箱与拆箱的操作，导致性能的下降

#### Hashtable

数据以“键值对”的形式进行存储

根据key获取value   `ht['key']`

##### 属性

* Count
  * Hashtable中键值对的数目
* Keys
  * 键的集合
* Values
  * 值的集合

##### 方法

* Add(object key, object value)
  * 添加一组键值对，重复添加相同的key会抛出`ArgumentException`
  * 还可以通过`ht[6] = "新来的"`这种方式添加数据，对一个key重复赋值会覆盖
* Contains(object key)
* ContainsKey(object key)
* ContainsValue(object value)
* Clear()
  * 移除所有元素
* Remove(object key)
  * 移除键为key的键值对

```c#
foreach (var item in ht.Keys) {
    Console.WriteLine(ht[item])
}
```

### Path

静态类

```c#
using System.IO;
```

* static GetFileName(string pathname)
  * 获取pathname中的文件名
* static GetFileNameWithoutxtension(string pathname)
  * 获取不带扩展名的文件名
* static GetExtension(string pathname)
  * 获取扩展名
* static GetDirectoryName(string pathname)
  * 获取文件所在文件夹的名称
* static GetFullPath(string pathname)
  * 获取文件的绝对路径
* static Combine(str1, str2)
  * 连接两个字符串作为路径

### File

静态类

一般用于读小文件

```c#
using System.IO;
```

* static Create(string file_path)
  
  * 通过指定的路径（相对/绝对）来创建文件
  
* static Delete(string file_path)
  
  * 通过相对路径或者绝对路径删除文件
  
* static Copy(string source_file, string dest_file)
  
  * 复制文件
  
* static byte[] ReadAllBytes(string path)

  * 打开一个文件，将文件的内容以字节的形式读入一个字符串，然后关闭文件

  ```c#
  byte[] buf = File.ReadAllBytes(@"C:\1.txt");
  string s = Encoding.GetEncoding("GB2312").GetString(buffer);
  ```

* static byte[] WriteAllBytes(string path, byte[] contents)
  
  * 将字节写入一个文件。如果文件没有，则创建
* static string[] ReadAllLines(string path, Encoding encoding)
  
  * 将文件的每一行读入字符串数组
* static WriteAllLines(string path, string[] contents)
  
  * 将contents字符串数组写入path文件，每个元素占一行
* static string ReadAllText(string path)
  
  * 把文件的内容读入一个字符串
* static WriteAllText(string path, string contents)
  
  * 将contents写入path文件
* static AppendAllText(string path, string content)
  
  * 将字符串content追加到path文件中

#### 字符转字节

```c#
string s = "123";
byte[] buf = Encoding.Default.GetBytes(s);
```

#### 字节转字符

```c#
byte[] buf = new byte[20];
string s = Encoding.Default.GetString(buf);
```



### FileStream

操作字节

```c#
using System.IO;

FileStream fsStream = new FileStream(string path, FileMode fileMode, FileAccess fileAccess)
```

**FileMode**

* Append
* Create
* Open
* OpenOrCreate
* CreateNew
* Truncate

**FileAccess**

* Read
* ReadWrite
* Write

**方法**

* int Read(byte[] array, int offset, int count)
  * 从流中读取字节块并将该数写入给定缓冲区的offset位置开始的count个元素中，返回本次读到实际有效的字节数
* void Write(byte[] array, int offset, int count)
  * 将字节块的offset位置开始的count个元素写入文件流
* Close()
  * 关闭流
* Dispose()
  * 释放流所占用的资源

注：将创建文件流的对象的过程写在using语句中，会自动帮助释放流所占用的资源

#### 读取

```c#
using System.IO;
```

#### 写入

```c#
using System.IO;

// 使用using语句自动释放流所占用的资源
using (FileStream fsWriter = new FileStream(@"C:\1.txt", FileMode.OpenOrCreate, FileAccess.Write)) {
    
}
```

#### 读与写

```c#
using (FileStream fRead = new FileStream("1.jpg", FileMode.Open, FileAccess.Read),
            fWrite = new FileStream("copy_1.jpg", FileMode.OpenOrCreate, FileAccess.Write))
            {
                int length = -1;
                byte[] buf = new byte[1024]; //1k
                while ((length = fRead.Read(buf, 0, buf.Length)) > 0)
                {
                    fWrite.Write(buf, 0, length);
                } 
            }
```

### StreamReader和StreamWriter

操作字符

**StreamReader**

```c#
using System.IO;
StreamReader sr = new StreamReader(string path, Encoding encoding);
```

**属性**

* EndOfStream
  * 是否读到文件结尾。如果是，返回true

**方法**

* string ReadLine()
  * 读取当前一行字符串

**StreamWriter**

```c#
StreamWriter sw = new StreamWriter(string path, Encoding encoding, bool append);
```

append表示是否追加，默认是false

**方法**

* Write(object value)

### List泛型集合

```c#
using System.Collections.Generic;
List<int> list = new List<int>();
```

#### 属性

* Count
  * 实际存在的元素个数

#### 方法

* Add(`<T>` value)
* AddRange(`<T>[]`  values)
* Remove()
* RemoveAt()
* RemoveRange(int index, int count)
* Reverse()
* Sort()
* Insert(int index, object value)
  * 在指定的index处插入value
* insertRange(int index, ICollection c)
  * 在指定的index处插入一个集合
* `<T>[]` ToArray()
  * 将集合转换为数组

### Dictionary字典集合

```c#
using System.Collections.Generic;
Dictionary<int, string> dic = new Dictionary<int, string>();
dic.Add(1, "张三");
dic.Add(2, "里斯");

foreach (KeyValuePair<int, string> kv in dic) {
    Console.WriteLine("{0}:{1}", kv.Key, kv.Value);
}
```



#### 属性

* Keys
  * 键

#### 方法

