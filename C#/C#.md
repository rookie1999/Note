# C#

## .Net Framework

> * 提供一个一致的面向对象的编程环境
> * 提供一个将软件部署和版本控制冲突最小化的代码执行环境
> * 提供一个可提高代码（包括由未知的或不完全受信任的第三方创建的代码）执行安全性的代码执行环境
> * 提供一个可消除脚本或解释环境的性能问题的代码执行环境
> * 使开发人员的经验在面对类型大不相同的应用程序时保持一致
> * 按照工业标准生成所有通信，以确保基于.Net Framework的代码可与任何其他代码集成

分为

* CLR （公共语言运行时）
  * 管理内存，线程执行，代码执行，代码安全验证，编译和其他系统服务
  * 安全性
  * 访问安全
  * CTS Common Type System
  * 消除常见软件问题
  * 提高开发效率（兼容性特别好，比如VB等语言开发的程序）
  * 增强性能（比如，JIT just-in-time及时编译）
  * 宿主应用
  * 自由的多线程处理
  * 结构化的异常处理
  * 自定义attribute支持
  * 垃圾回收
  * 使用委托取代了函数指针，从而增强了类型安全和安全性
  * 托管执行过程
    1. 选择编译器
    2. 将代码编译为MSIL
    3. 将MSIL编译为本机代码
    4. 运行代码
  * 自动内存管理
    * 分配内存
    * 释放内存
    * 级别和性能
      * 0-2级，一共三级
      * 新创建的对象在第0级
      * 当第0级内存满了的时候或是在合适的时候，会回收不再需要或继续使用的对象，而仍被使用的对象会升级到第1级
    * 为非托管资源释放内存
* .Net类库

C#可以开发基于.net平台的应用

1. 

### 应用

* 控制台应用程序
* 桌面应用程序
  * Winform应用程序
* Internet应用程序
  * ASP.NET
* 手机开发
  * Wp8
* Unity3D游戏开发
* WIndows Presentation Foundation (WPF)
* Windows服务

### 两种交互模式

* C/S 客户机/服务器模式
* B/S 浏览器/服务器模式

## cs文件结构

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



### 操作符operator

本质是函数

**example**

```c#
static void Main(string[] args)
{
    Person husband = new Person();
    husband.Name = "Tom";
    Person wife = new Person();
    wife.Name = "Tom's wife";
    List<Person> children = husband + wife;
    Console.WriteLine(children.Count);
    foreach (var person in children)
    {
        Console.WriteLine(person);
    }
}

public class Person
{
    public string Name;

    public static List<Person> operator +(Person s1, Person s2)
    {
        List<Person> children = new List<Person>();
        for (int i = 0; i < 11; i++)
        {
            Person child = new Person();
            child.Name = $"{s1.Name}_{s2.Name}'s child";
            children.Add(child);
        }
        return children;
    }

    public override string ToString()
    {
        return this.Name;
    }
}
```

**default操作符**

default(type)可以获取type的默认值，int是0，引用类型是null

枚举类型，返回索引为0的枚举值，如果不存在，则返回0



**自定义的显式转换**

```c#
Stone stone = new Stone();
stone.Age = 10000;
Monkey monkey = (Monkey)stone;
Console.WriteLine(monkey.Age);

public class Stone
{
    public int Age;

    public static explicit operator Monkey(Stone stone)
    {
        Monkey m = new Monkey();
        m.Age = stone.Age / 500;
        return m;
    }
}

public class Monkey
{
    public int Age;
}
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

ref的实参和形参在栈中是同一块地址

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

使用`using static`可以静态导入其他名称空间的静态字段或静态方法

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

### 抽象

abstract可以修饰class或者方法或者属性

抽象方法没有方法体

抽象成员在抽象类中

抽象类无法实例化

抽象属性可以被重写

### 访问修饰符

* public
* internal
  * 只能在当前程序集中访问（暂时理解为是项目内）
* protected
  * 只能在当前类以及该类的子类中访问
* protected internal
* private

可以修饰class的修饰符是public和internal，默认是internal

#### 可访问性

父类的访问级别必须高于子类的访问级别（否则会暴露不该被访问的成员）

### 部分类

在类上用关键字partial修饰，可以在一个项目中重复定义该类，并且在这些类中的字段、属性和方法可以通用（方便多人协作开发）

所有的部分类组合成一个完整的类

### 密封类

在类上用关键字sealed修饰，该类不可以被继承

### 接口interface

接口可以有方法、自动属性以及索引器

```c#
[public] interface  I...able {
    
}
```

* 接口中的成员不允许添加访问修饰符，默认是public
* 接口的方法不允许有定义
* 接口中不可以包含字段
* 接口与接口之间可以多继承
* 如果一个类继承了一个类A，又实现了接口B，则A需要写在B的前面

#### 接口的显式声明

```c#
interface A 
{
    void Method();
}

class B : A 
{
    void A.Method() {}
}
```

当接口的实现定义为显式声明的时候，只有`A a = new B();`这样声明并创建的对象实例才可以调用接口A中的Method方法

### 自动属性

不需要创建私有字段，直接声明属性（程序会自动动态创建一个与之对应的字段）

```c#
public class Person {
    public int Age {
        get;
        set;
    }
}
```



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
* WriteLine(object value)

### Directory

```c#
using System.IO;
```

#### 方法

* CreateDirectory(string path)
  * 在指定路径下创建文件夹
  * 可以创建多级目录
* Delete(string path, bool recursive)
  * 删除目录
  * recursive指示是否级联删除，默认是false只能删除空文件夹
* Move(string oldDir, string newDir)
  * 将旧文件夹移动到新位置
* string[] GetFiles(string path， string searchPattern)
  * 返回指定目录中文件的名称（包括其路径）
  * searchPattern指定搜索模式
    * 比如  "*.jpg"
* string[] GetDirectories(string path, string searchPattern)
  * 获取指定目录下的文件夹的全路径
* bool Exists(string path)
  * 判断路径是否存在

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

> 线程不安全
>
> 安全的可以使用ConcurrentDictionary

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



### HashSet

HashSet<T>

默认去重

方法

* Add(T element)
  * 添加一个元素
* void IntersectWith(HashSet<T> set2)
  * 获取当前set和set2的交集
* void  UnionWith(HashSet<T> set2)
  * 获取当前set和set2的并集
* void  ExceptWith(HashSet<T> set2)
  * 获取set中含有但是set2没有的元素（差集）
* void  IntersectWith(HashSet<T> set2)
  * 获取set中没有但是set2含有的元素（补集）

适合

* 二次好友
* 间接关注
* 粉丝合集



### SortedSet

去重+排序

构造方法可以传入一个`IComparer`对象，指定排序规则



### HashTable

键值对

如果不同的key映射到同一位置，则第二个key的索引加一

调用`Synchronized(HashTable table)`会返回一个同步的table



### Guid

是一个不会重复的编号

```c#
string id = Guid.NewGuid().ToString();
```

### MD5加密

```c#
// 1. 创建MD5对象
MD5 md5 = MD5.create();
// 2. 加密得到字节数组
byte[] cipher = md5.ComputeHash(Encoding.Default.GetBytes(string s));
// 3. 转换为字符串
string cipherStr = "";
for (var ch in cipher) {
    // 将每一位转为两位16进制
    cipherStr += ch.ToString("x2");  
}
```

### DateTime

#### 属性

* Now
  * 得到一个值为当前时间的DateTime对象
* Now.Hour
  * 获取当前小时数（int）
* Now.Minute
* Now.Second

### SoundPlayer

只能播放wav格式

```c#
SoundPlayer sp = new SoundPlayer();
sp.SoundLocation = wavFilePath;
sp.play();
```



## 设计模式

### 简单工厂模式

根据不同的输入获取不同的对象（通过条件结构以及多态实现）

## 序列化与反序列化

### 序列化

将对象转换为二进制

Step：

1. 给需要被序列化的class添加Attribute `Serializable`

2. 对不想被序列化的属性可以添加`[NonSerialized]`

3. 使用BinaryFormatter将对象转换为byte数组

   ```c#
   using System.Runtime.Serialization.Formatters.Binary;
   ```

```c#
Person p = new Person();
            p.Name = "zhangsan";

            using (FileStream fsWrite = new FileStream(@"C:\Users\bzhan\OneDrive - Infor\Desktop\person.obj", FileMode.OpenOrCreate, FileAccess.Write))
            {
                BinaryFormatter binaryFormatter = new BinaryFormatter();
                binaryFormatter.Serialize(fsWrite, p);  // 序列化并write
            }
```

注：目前显示BinaryFormatter已经被弃用了，可以使用`XmlSerializer`或者是`JsonSerializer`

### 反序列化

将二进制转为文件

```c#
Person p = null;
using (FileStream fsRead = new FileStream(@"C:\Users\bzhan\OneDrive - Infor\Desktop\person.obj", FileMode.OpenOrCreate, FileAccess.Read)) {
    BinaryFormatter bf = new BinaryFormatter();
    p = (Person) bf.Deserialize(fsRead);
}
```



### XmlSerializer

#### 序列化

```c#
using System.IO;
using System.Xml.Serialization;

/*
     * XmlRootAttribute设置xml文档根节点的名称，namespace以及是否显示xsi当xsi为null
     */
    [XmlRootAttribute("Person", Namespace="http://www.cpandl.com", IsNullable=false)]
    public class Person
    {
        private string _name;

        // 将age作为xml的属性而不是element
        [XmlAttribute]
        public int age;

        public string Name
        {
            get { return _name; }
            set { _name = value; }
        }
    }


// 序列化代码
Person p = new Person();
p.Name = "zhangsan";
p.age = 18;

using (FileStream fsWrite = new FileStream(@"C:\Users\bzhan\OneDrive - Infor\Desktop\person.xml", FileMode.OpenOrCreate, FileAccess.Write))
{
    XmlSerializer serializer = new XmlSerializer(typeof(Person));
    serializer.Serialize(fsWrite, p);
}
```

#### 反序列化

```c#
XmlSerializer serializer = new XmlSerializer(typeof(Person));
            using (var fsRead = new FileStream(@"C:\Users\bzhan\OneDrive - Infor\Desktop\person.xml", FileMode.Open, FileAccess.Read))
            {
                Person p = (Person)serializer.Deserialize(fsRead);
            }
```



### json序列化

dll是`System.Web.Extensions.`

命名空间是`System.Web.Script.Serialization.JavascriptSerializer`



或者是

`Newtonsoft.Json`



## Winform应用程序

> 是一种智能客户端技术，使用winform应用程序帮助获取信息或者传输信息等

### 注意事项

1. 在Main函数中创建的窗体对象，称之为这个窗体应用程序的主窗体（当主窗体关闭后，整个应用程序都关闭了）
2. // 取消对跨线程操作控件的检查
               Control.CheckForIllegalCrossThreadCalls = false;

### 控件

#### Button

#### TextBox

* 文本框
* WordWrap  是否自动换行
  * true是不换行
* ScrollBars	 滚动条
  * Horizontal
  * Vertical
  * Both
  * None
* PasswordChar   显示为密码状态
  * *
* TextChanged
  * 当文本框的内容改变时触发
* Focus()
  * 设置焦点

#### Label

#### Timer

* 在指定的时间间隔内做一件指定的事情
* 默认Enabled是false，需要修改为true
* Interval
  * 事件执行的频率
* Tick
  * 执行的事件

#### WebBrowser

* 在窗体中添加一个网页
* Url

```c#
// ulrText是一个TextBox
string text = urlText.Text;
if (!text.StartsWith("http://") && !text.StartsWith("https://"))
{
    text = "http://" + text;
}
Uri uri = new Uri(text);
// webBrowser是WebBrowser类型的
webBrowser.Url = uri;
```

#### ComboBox

* 下拉框

```c#
// 给下拉框增加item
comboBox.Items.Add(string data);

// 清空items
comboBox.Items.Clear();
```

* DropDownStyle
  * 下拉菜单的样式
  * Simple
  * DropDown
  * DropDownList
* SelectedText
  * 选中项对应的文本
* SelectedIndex
  * 选中项的索引
* SelectedItem
  * 选中项
* SelectedValue
  * 选中项的值
* SelectIndexChanged
  * 当下拉菜单的选中项修改的时候会触发

#### ListBox

* 与ComboBox使用方法类似

#### PictureBox

* 显示图片

* PictureBox.Image

  ```c#
  PictureBox pictureBox = new PictureBox();
  pictureBox.Image = Image.FromFile(string path);
  ```

* SizeMode
  * 设置模式  值为PictureBoxSizeMode类型的枚举值

#### OpenFileDialog

* 打开文件对话框

```c#
OpenFileDialog odf = new OpenFileDialog();
odf.ShowDialog();
```

* Title

  * 对话框的标题

* MultiSelect

  * 对话框是否可以多选，默认是false

* InitialDirectory

  * 对话框的初始目录

* Filter

  * 设置对话框的文件类型

  ```
  odf.Filter = "文本文件|*.txt|媒体文件|*.wmv|图片文件|*.jpg";
  odf.Filter="所有文件|*.*";
  ```

* FileName
  * 在单选模式下选中的文件
  * 如果打开对话框之后没有选中文件，则返回""
* FileNames
  * 在多选模式下选中的文件

#### SaveFileDialog

* 保存文件对话框
* FileName
  * 在单选模式下选中的文件

#### FontDialog

字体对话框

* Font
  * 通过字体对话框设置后的字体的相关属性会保存在FontDialog对象的Font字体中
  * 然后可以将某个控件的Font属性赋值为该对话框的属性

#### ColorDialog

颜色对话框

* Color
  * 设置完的颜色相关设置会保存在ColorDialog的Color属性
  * 再将该属性赋值给某个控件的ForeColor属性

#### Panel

容器类

#### CheckBox

复选框

* Checked
  * 是否被选中

#### RadioButton

单选按钮

默认页面上的所有单选按钮都是一个group

如果要显式指定分组，可以使用GroupBox

* checked
  * 是否被选中

#### DataGridView

显示数据的控件

对属性`DataSource=list`绑定一个Collection集合对象，内部会自动遍历该集合，并将对象的属性显示到控件上

内部的实现原理是反射，使用`GetProperties()`方法

可以设置选中方式为行选中，也可以设置是否可以选中多行

获取当前选中的行  `DataGridViewRow currentRow = this.dataGridView.Rows[e.RowIndex]`

获取当前行绑定的对象  `Person person = currentRow.DataBoundItem as Person`

### 属性

* Name
  * 在后台要获取前台的控件对象，需要使用Name属性
* Text
  * 显示的文本
* Anchor
  * 根据top、bottom、left、right来进行定位
* BackColor
  * 背景颜色
* BackgroundImage
  * 背景图片
* BackgroundImageLayout
  * 背景图片的布局（None，Stretch，Zoom）
* ContextMenuStrip
  * 右键菜单
* Cursor
  * 设置鼠标的样式
* Visible
  * 控件是否可见
* Enabled
  * 控件是否可用
* ClientSize
  * 窗体工作区的大小（会根据放大缩小也进行变化）
  * ClientSize.Width
  * ClientSize.Height

### MenuStrip

菜单栏

### 事件

注册事件

触发事件

* Click

  * 单击事件

  ```c#
  private void button1_Click(object sender, EventArgs e)
  {
      Button btn = (Button) sender;
      MessageBox.Show("Hello World");
  }
  ```

* MouseEnter
  * 鼠标移入的事件
* Load
  * 每当用户加载窗体的时候触发

### MDI窗体

Multiple Document Interface  多文档窗体

设置方法：将窗体的属性`IsMdiContainer`设置为True

在MDI窗体中弹出新窗体，需要在show方法前使用新窗体的MdiParent设置为MDI窗体

```c#
Form parentForm = new Form();
parentForm.IsMdiContainer = True;

Form subForm = new Form();
subForm.MdiParent = parentForm;
subForm.Show();
```

* 排列MDI的子窗体
  * LayoutMdi(MdiLayout value)
  * MdiLayout是枚举
    * TileHorizontal
      * 横向排列
    * TileVertical
      * 纵向排列

## 多线程

> 多线程的最好解决方案就是没有冲突

### Process

进程类

```c#
using System.Diagnostics;
```

* static Process[] GetProcess()
  * 获取当前运行的所有进程
* static GetProcessByName(string name)
  * 获取指定名称的线程
* Kill()
  * 杀死当前进程
* static Start(string ProcessName)
  * 启动某个进程

第二种创建的方式

```c#
ProcessStartInfo psi = new ProcessStartInfo(string filePath);
Process p = new Process();
p.StartInfo = psi;
p.Start();
```

### Thread

```c#
using System.Threading;
// 创建一个thread执行func方法
Thread t = new Thread(ThreadStart | ParameterizedThreadStart);  
// ThreadStart是无参数无返回值的委托（不能使用Action）
// ParameterizedThreadStart是有参数无返回值
t.Start();  // 标记线程为准备就绪状态
```

程序会在所有的前台线程运行结束时才会结束，此时所有的后台线程也会立即结束

.Net不允许跨线程访问

解决方法是：不允许程序检查错误线程的调用

```c#
Control.CheckForIllegalCrossThreadCalls = false;
```

#### 属性

* IsBackground
  * 是否是后台线程
  * 默认情况下，手动创建的线程是前台线程
  * 只要有前台线程在运行，那么应用程序就会一直处于活动状态
  * 一旦所有的前台线程停止，那么应用程序就停止了，任何的后台线程也会突然终止
  * 应用程序无法正常退出的一个原因是还有活跃的前台线程
  
* static CurrentThread
  * 获取当前线程的引用对象
  
* IsAlive
  * 线程是否在运行
  
* ThreadPriority Priority

  * 线程的优先级

  * ThreadPriority是一个枚举Enum

    * Lowest
    * BelowNormal
    * Normal
    * AboveNormal
    * Highest

  * 如果想要让某线程的优先级比其他进程中的线程高，那么就必须提升进程的优先级

    * ```c
      Process.GetCurrentProcess().PriorityClass = ProcessPriorityClass.High;

#### 方法

* Abort
  * 将当前线程终止，并抛出异常
  * 不一定及时，并且有些动作发生停止不了
  * 使用`Thread.ResetAbort()`取消所有abort请求
* static Sleep(int millisencond)
  * 暂时让当前线程睡眠
  * `Thread.Sleep(0)`
    * 会导致线程立即放弃自身当前的时间片，自动将CPU移交给其他线程

* static Yield()
  * 与Sleep相同，但是只会把执行交给同一处理器上的其他线程

* Start()
  * 将该线程标记为准备就绪状态
* Start(object parameter)
  * 将该线程标记为准备就绪状态，并且传入线程执行所需要的参数
  * 注意：如果线程执行的方法是带参数的，则参数的类型必须是object
* void Join()
  * 调用join的线程执行完毕之后，再执行外面的线程
* bool Join(Timespan)
  * 设置一个超时，返回true，即线程正常结束；返回false，超时

当等待Sleep或Join的时候，线程处于阻塞状态，即`ThreadState.WaitSleepJoin`

判断是否处于阻塞状态:  `bool blocked = (someThread.ThreadState & ThreadState.WaitSleepJoin) != 0`

解除阻塞：

* 阻塞条件被满足

* 操作超时（如果设置超时时间的话）
* 通过Thread.Interrupt()进行打断
* 通过Thread.Abort()进行中止

#### 本地状态

local  state

* CLR为每个线程分配自己的内存栈stack，以便使本地变量保持独立

#### 共享状态

Shared State

* 如果多个线程都引用到同一个对象的实例，那么它们就共享了数据
* 被Lambda表达式或匿名委托所捕获的本地变量，会被编译器转化为字段field，所以也会共享
* 静态字段也会在线程间共享数据



#### 加锁机制

C#使用lock语句来加锁

当两个线程同时竞争一个锁的时候（锁可以基于任何引用类型对象），一个线程会等待或阻塞，直至锁变成可用的状态

```c#
static void Main(string[] args)
{
    new Thread(GoWithLocker).Start();
    GoWithLocker();
}

static void GoWithLocker()
{
    lock (_locker)
    {
        if (!_done)
        {
            Console.WriteLine("Done");
            _done = true;
        }
    }
}
```



#### 数据传递和异常处理

数据传递的两种办法

* 创建Thread时，使用lambda表达式

  * ```c#
    new Thread(() => Method(parms)).Start();
    ```

* 在Start方法中加入参数（注入参数是object类型）

  * 过时的方式

  * ```c#
    new Thread(Method).Start(parms);
    ```

异常处理

在线程内抛出的异常，只能在线程内部进行处理，不能在创建线程的地方进行处理



#### Signal

让某线程一直处于等待的状态，直至接收到其他线程发来的通知。这就叫做signaling 发送信号

最简单的信号结构就是ManualResetEvent

​	调用WaitOne方法会阻塞当前的线程

​	直到另一个线程通过调用Set方法来开启信号

​	调用Set之后，信号处于“打开”状态，可以通过调用Reset方法将其再次关闭

```c#
var signal = new ManualResetEvent(false);

new Thread(() => {
    Console.WriteLine("Waiting for signal...");
    signal.WaitOne();
    signal.Dispose();
    Console.WriteLine("Got signal");
}).Start();

Console.ReadLine();
signal.Set();
```



#### 富客户端应用程序处理耗时操作的一种办法

在WPF，UWP，WinForm等类型的程序中，如果在主线程执行耗时的操作，就会导致整个程序无响应。因为主线程同时还需要处理消息循环，而渲染和鼠标键盘事件处理等工作都是通过消息循环来执行的

针对这种耗时的操作，一种流行的办法是启动一个worker线程

​	执行完操作后，再更新到UI

富客户端应用的线程模型通常是

​	UI元素和控件只能从创建它们的线程来进行访问（通常是主UI线程）

​	当想从worker线程更新UI时，需要请求UI线程

比较底层的实现是：

* 在WPF，元素的Dispatch对象上调用BeginInvoke或者Invoke
* 在Winform，调用控件的BeginInvoke或Invoke
* UWP，调用Dispatcher对象上的RunAsync或Invoke
* 所有方法都接受一个委托
* BeginInvoke或RunAsync通过将委托排队到UI线程的消息队列来执行工作
* Invoke执行相同的操作，但随后会进行阻塞，直到UI线程读取并处理消息
  * 因此，Invoke允许从方法中获取返回值
  * 如果不需要返回值，BeginInvoke/RunAsync更可取，因为它们不会阻塞调用方，也不会引入死锁的可能性



#### Synchronization Contexts

同步上下文

在System.ComponentModel下有一个抽象类，SynchronizationContext，它使得Thread Marshalling（线程间转移数据）得到泛化

针对移动、桌面（WPF、UWP、WinForms）等富客户端应用的API，它们都定义和实例化了SynchronizationContext的子类

* 可以通过静态属性SynchronizationContext.Current来获得（当运行UI线程）
* 捕获该属性让你可以在稍后的时候从worker线程向UI线程发送数据
* 调用Post方法等价于Dispatcher或者Control上面的BeginInvoke方法
* 调用Send方法等价于Invoke方法



#### 线程的异常

多线程中子线程抛出的异常可能会被吞掉（因为在子线程抛出异常的时候，主线程的代码已经执行到trycatch代码块之外的地方）

建议：在子线程中最好不要抛出异常，而是自己处理好



#### 线程的取消

当某个线程失败之后，希望通知别的线程停止操作

由于多线程的无序性，以及线程是OS的资源，最好的方法是让线程自己停止自己

使用一个公共的访问变量，线程不断检测，当改变之后就自己停止

`CancellationTokenSource`标志任务的取消

* void Cancel()
  * 取消任务
* void CancelAfter(new TimeSpan(0, 0, 0, 4))
  * 延时取消任务
* bool IsCancelledRequest
  * 是否发出取消请求
* string Token
  * Task.Factory.StartNew(Action action, CancellationTokenSource)
  * 当token被cancel过了，则Task不启动并抛出异常

```c#
CancellationTokenSource source = new CancellationTokenSource();
// 注册一个线程被取消之后执行的逻辑
source.Token.Register(() =>
                      {
                          Console.WriteLine("The business logic after thread cancellation");
                      });

Task.Run(() =>
         {
             // 判断取消任务请求是否发出
             while (!source.IsCancellationRequested)
             {
                 Thread.Sleep(1000);
                 Console.WriteLine("Now thread={0} is running", Thread.CurrentThread.ManagedThreadId);
             }
         }, source.Token);

Thread.Sleep(1000);
source.Cancel();
```

* static CancellationSourceToken CreateLinkedTokenSource(CancellationSource source1, CancellationSource source2)
  * 将两个source结合起来，有一个取消，则都取消

```c#
CancellationTokenSource source1 = new CancellationTokenSource();
CancellationTokenSource source2 = new CancellationTokenSource();
CancellationTokenSource source = CancellationTokenSource
    .CreateLinkedTokenSource(source1.Token, source2.Token);
source2.Cancel();

Console.WriteLine(source1.IsCancellationRequested); // false
Console.WriteLine(source2.IsCancellationRequested); // true
Console.WriteLine(source.IsCancellationRequested); // true
```

* CancellationTokenSource.Token.ThrowIfCancellationRequested()
  * 当任务已经取消，此方法会抛出异常



#### 线程安全

三种方法

1. lock
   * 牺牲性能，尽量减小lock的范围
   
   ```c#
   private static readonly object obj = new object();
   
   lock (obj)
   {
       // 需要加锁的语句
   }
   ```
   
   
   
2. 使用类似安全队列`ConcurrentQueue`的技术

3. 数据拆分，避免线程安全问题



#### 线程池 Thread Pool

> 重用thread，防止多余的创建和销毁
>
> 简化了线程的api和使用

* static void QueueUserWorkItem(WaitCallback callback)

  * 将callback加入线程的执行队列，当有空闲线程时会执行该方法

  ```c#
  WaitCallback action = (object state) =>
  {
      for (int i = 0; i < 10; i++)
          Console.WriteLine(Thread.CurrentThread.Name + "_" + i.ToString());
  };
  
  ThreadPool.QueueUserWorkItem(action);
  ```

* ThreadPool.GetMaxThreads(out int workerThreads, out int completionPortThreads)

  * 获取最大的worker thread和IO异步 thread数量

* GetMinThreads(out int workerThreads, out int completionPortThreads)

* SetMaxThreads(out int workerThreads, out int completionPortThreads)

* SetMinThreads(out int workerThreads, out int completionPortThreads)



使用`ManualResetEvent`事件完成线程等待

内部保存一个bool参数

执行WaitOne()方法时，如果内部参数是false，则一直等待，如果内部参数是true，则放行

调用`Set()`方法将内部参数设置为true

调用`Reset()`方法将内部参数设置为false

```c#
ManualResetEvent manualResetEvent = new ManualResetEvent(false);
WaitCallback action = (object state) =>
{
    for (int i = 0; i < 10; i++)
        Console.WriteLine(Thread.CurrentThread.Name + "_" + i.ToString());
    manualResetEvent.Set();
};

ThreadPool.QueueUserWorkItem(action);

manualResetEvent.WaitOne();

Console.WriteLine("done");
```



* 当开始一个线程的时候，将花费几百微秒来组织类似以下内容
  * 一个新的局部变量栈
* 线程池可以节省这种开销
  * 通过预先创建一个可循环使用的线程的池来减少这一开销
* 线程池对于高效的并行编程和细粒度并发是必不可少的
* 它允许在不被线程启动的开销淹没的情况下运行短期操作

**使用线程池需要注意的几点**

* 不可以设置线程池的Name
* 池线程都是后台进程
* 阻塞池线程可使性能降级
* 释放池线程回到线程池之后，线程的优先级将回到正常状态
* 通过`Thread.CurrentThread.IsThreadPoolThread`属性判断是否执行在线程池

**进入线程池**

* 最简单、显式的在池线程运行代码的方式就是使用`Task.Run`

  * ```c#
    Task.Run(() => Console.WriteLine("Hello from the thread pool"));
    ```

**谁使用了线程池**

* WCF、Remoting、ASP.NET、ASMX Web Services应用服务器
* System.Timers.Timer、System.Threading.Timer
* 并行编程结构
* BackgroundWorker类（现在很多余）
* 异步委托 （现在很多余）

**线程池中的整洁**

* 线程池提供了一个功能，即确保临时超出 Compute-Bound的工作不会导致CPU超额订阅
* CPU超额订阅：活跃的线程超过CPU的核数，操作系统就需要对线程进行时间切片
* 超额订阅对性能影响很大，时间切片需要昂贵的上下文切换，并且可能使CPU缓存失效，而CPU缓存对现代处理器的性能至关重要

![image-20211107154556842](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211107154556842.png)



#### Task

##### Thread的问题

* 线程（Thread）是用来创建并发concurrency的一种低级别工具，它有一些限制，尤其是：
  * 虽然开始线程的时候可以方便地传入数据，但是当join的时候，很难从线程获取返回值
    * 可能需要设置一些共享字段
    * 如果操作抛出异常，捕获和传播该异常都很麻烦
  * 无法告诉线程在结束时开始做另外的工作，你必须进行大量的Join操作（在进程中阻塞当前的线程）
* 很难使用较小的并发来组建大型的并发
* 导致了对手动同步的更大依赖以及随之而来的问题



##### Task简介

方法

* static Task Run(Action action)
* static void WaitAll(params Task[] tasks)
  * 等待所有的task运行结束，才继续当前thread的运行
* WaitAll是用来处理阻塞式操作，WhenAll是用来处理非阻塞式操作
* static void WaitAll(params Task[] tasks, TimeSpan time)
* static void WaitAny(params Task[] tasks)
  * 等待一个task任务，就继续当前thread的运行
* static void WhenAll(params Task[] tasks)
* static void WhenAny(params Task[] tasks)

```c#
List<Task> tasks = new List<Task>();
tasks.Add(Task.Run(() => Coding("1", "Test")));
tasks.Add(Task.Run(() => Coding("2", "Develop")));
tasks.Add(Task.Run(() => Coding("3", "Deploy")));
tasks.Add(Task.Run(() => Coding("4", "Design")));

// 上述任一任务完成之后，会开启ContinueWith中的Task
Task.WhenAny(tasks.ToArray()).ContinueWith(t => Console.WriteLine("First work is done."));
```

* static Task Delay(int milliseconds)
  * 返回一个延迟milliseconds的Task
* void Wait()

  * 当前线程等待此Task的运行完成







* Task可以很好地解决Thread的问题

* Task是一个相对高级的抽象，它代表了一个并发操作concurrent

  * 该操作可能有Thread支持，或不支持

* Task是可组合的（可使用Continuation把它们串成链）

  * Tasks可以使用线程池来减少启动延迟
  * 使用TaskCompletionSource，Tasks可以利用回调的方式，在等待IO绑定操作时，完全避免线程

* Task默认使用线程池，也就是后台线程

  * 当主线程结束时，创建的所有Tasks都会结束

* Task.Run返回一个Task对象，可以用其来监视其过程

  * 在Task.Run之后，没有调用Start方法，因为该方法创建的Task是“热”任务（hot task）
    * 可以通过Task的构造函数创建“冷”任务（cold task），但是很少这样做

* 调用Wait方法会进行阻塞直到操作完成

  * 相当于Thread的Join方法
  * Wait也可以让你指定一个超时时间和一个取消令牌来提前结束等待

* 默认情况下，CLR在线程池中运行Task，这非常适合短时间运行的Compute-Bound类工作

* 针对长时间运行的任务或者阻塞操作，可以不采用线程池（例如LongRunning）

  * ```c#
    Task task = Task.Factory.StartNew(() => {
        Thread.Sleep(3000);
        Console.WriteLine("Task Run not in a thread pool");
    }, TaskCreationOptions.LongRunning);
    task.Wait();
    ```

* 如果同时运行多个long running tasks（尤其是其中有处于阻塞状态的），那么性能将会受到很大的影响，这时有比TaskCreationOptions.LongRunning更好的办法

  * 如果任务是IO bound，TaskCompletionSource和异步函数可以让你用回调Continuations代替线程来实现并发
  * 如果任务是Compute bound，生产者/消费者队列允许你对任务的并发性进行限流，避免把其他线程或进程饿死

开始一个Task最简单的方法就是使用`Task.Run`这个静态方法，在.NET4.0的时候是使用`Task.Factory.StartNew`（参数传入一个Action委托即可）



##### Task的返回值

* Task有一个泛型子类`Task<TResult>`，它允许发出一个返回值
* 使用`Func<TResult>`委托或兼容的Lambda表达式来调用Task.Run就可以得到`Task<TResult>`
* 随后，可以通过Result属性获得返回的结果
  * 如果这个task还没有完成，访问Result属性会阻塞该线程直到该task完成操作
* `Task<TResult>`可以看做是一种所谓的“未来/许诺”（future，promise），在它里面包裹着一个Result，在稍后就会变得可用



##### Task的异常

* 与Thread不一样，Task可以很方便地传播异常

* 如果task里面抛出了一个未处理的异常（故障），那么该异常会重新抛出给
  * 调用Wait()的地方

  * 访问了`Task<TResult>`的Result属性的地方

  * ```c#
    Task task = Task.Run(() => { throw null; });
    try
    {
        task.Wait();
    }
    catch (AggregateException e)
    {
        Console.WriteLine("Catch");
        Console.WriteLine(task.IsFaulted);  // true
        Console.WriteLine(task.IsCanceled); // false
    }
    ```

* CLR将异常包裹在`AggregateException`里，以便在并行编程场景中发挥很好的作用

* 无需重新抛出异常，通过Task的IsFaulted和IsCanceled属性也可以检测出Task是否发生了故障

  * 如果两个属性都为false，那么没有错误发生
  * 如果IsCanceled为true，那就说明OperationCanceledException抛出了
  * 如果IsFaulted为true，说明另一个异常抛出，而Exception的属性也将指明错误

* 自治Task

  * 自治的：设置完就不管了，指不通过调用Wait()方法，Result属性或continuation进行会合的任务

  * 针对自治的Task，需要像Thread一样，显式的处理异常，避免发生“悄无声息”的异常
  * 自治Task上未处理的异常称为未观察到的异常

* 未观察的异常

  * 可以通过全局的`TaskScheduler.UnobserverdTaskException`来订阅未观察到的异常
  * 关于什么是“未观察到的异常”，有一些细微的差别
    * 使用超时等待的Task，如果在超时后发生故障，那么将会产生一个“未观察到的异常”
    * 在task发生故障之后，如果访问task的Exception属性，那么该异常就被认为是“已观察到的”



##### Continuation

* 一个Continuation会对Task说，“当你结束的时候，继续再做点其他的事”

* Continuation通常是通过回调的方式实现

  * 当操作一结束，就开始执行

* 获取Continuation的第一种方式

  * 在Task上调用GetAwaiter()，类型是`TaskAwaiter<TResult>`
  * 它的OnCompleted方法会告诉之前的task，”当结束/故障的时候，要执行委托“
  * 可以将Continuation附加到已经结束的Task上面，此时Continuation将会被安排立即执行
  * 任何可以暴露下列两个方法和一个属性的对象就是Awaiter
    * OnCompleted
    * GetResult
    * 一个叫做IsCompleted的bool属性
  * 没有接口或父类来统一这些成员
  * 其中OnCompleted是INotifyCompletion的一部分
  * 如果之前的任务发生故障，那么当Continuation代码调用awaiter.GetResult()的时候，异常会被重新抛出
  * 无需调用GetResult()，我们可以直接访问task的Result属性
  * 但调用GetResult的好处是，如果task发生故障，异常会被直接抛出，而不是包裹在AggregateException里，这样的话catch块就会简洁很多
  * 针对非泛型的task，GetResult()方法有一个void返回值，它就是用来重新抛出异常的

* 如果同步上下文出现了，那么OnCompleted会自动捕获它，并将Continuation提交到这个上下文中。这一点在富客户端应用中非常有用，因为它会将Continuation放回到UI线程中

* 如果是编写一个库，则不希望出现上述行为，因为开销较大的UI线程切换应该在程序运行离开库的时候只发生一次，而不是出现在方法调用之间。所以，我们可以使用ConfigureAwait方法来避免这种行为

  * ```c#
    primeNumberTask.ConfigureAwait(false).GetAwaiter();
    ```

* 如果没有同步上下文出现，或者你使用的是ConfigureAwait(false)，那么Continuation会运行在先前task的同一个线程上，从而避免造成不必要的开销

* 第二种方式是使用ContinueWith

  * ContinueWith本身返回一个Task，它可以用来附加更多的Continuation
  * 但是，必须直接处理AggregateException
    * 如果task发生故障，需要写额外的代码来将Continuation marshall到UI应用上
    * 在非UI上下文中，若想要Continuation和task执行在同一个线程中，必须指定`TaskContinuationOptions.ExecuteSynchronously`，否则它将会弹回线程池
  * ContinueWith对于并行编程很重要



##### TaskCompletionSource

* 也是创建Task的一种方式

* 允许你在稍后开始或结束的任意操作中创建Task

  * 它会为你提供一个可手动执行的slave task
    * 指示操作何时结束或发生故障

* 对IO-Bound类工作比较理想

  * 可以获得所有Task的好处（传播值、异常、Continuation）
  * 不需要在操作的时候阻塞线程

* 有一个Task属性，返回一个Task，该Task完全由TaskCompletionSource控制

  * ```c#
    public class TaskCompletionSource<TResult>
    {
        public void SetResult(TResult result);
        public void SetException(Exception exception);
        public void SetCanceled();
        
        public bool TrySetResult(TResult result);
        public bool TrySetException(Exception exception);
        public bool TrySetCanceled();
        public bool TrySetCanceled(CancellationToken cancellationToken);
    }
    ```

  * 调用任意一个方法都会给Task发信号

    * 完成、故障、取消

  * 这些方法只能调用一次，如果再次调用

    * SetXxx 会抛出异常
    * TryXxx 会返回false

  * ```c#
    var tcs = new TaskCompletionSource<int>();
    
    new Thread(() =>
               {
                   Thread.Sleep(5000);
                   tcs.SetResult(42);
               })
    { IsBackground = true }.Start();
    Console.WriteLine(tcs.Task.Result);
    ```

  * ```c#
    // 调用此方法相当于调用Task.Factory.New
    // 并使用TaskCreationOptions.LongRunning选项来创建非线程池的线程
    static Task<TResult> Run<TResult>(Func<TResult> func)
    {
        var tcs = new TaskCompletionSource<TResult>();
        new Thread(() =>
                   {
                       try
                       {
                           tcs.SetResult(func());
                       }
                       catch (Exception e)
                       {
                           tcs.SetException(e);
                       }
                   }).Start();
        return tcs.Task;
    }
    ```

* 使用TaskCompletionSource创建Task，不会占用线程

* Task.Delay()

  * 相当于异步版本的Thread.Sleep
  * `Thread.Delay(5000).GetAwaiter().OnCompleted(() => Console.WriteLine(43));`
  * `Thread.Delay(5000).ContinueWith(task => Console.WriteLine(43));`



##### Task.Factory

* static Task StartNew(Action action)
  * 开启一个新的Task
* static Task<T> StartNew<T>(Action action)
* static Task StartNew(Action action, CancellationTokenSource)
  * 开启一个新的Task，并包含一个CancellationTokenSource
* static ContinueWhenAll(Task[] tasks, Action<Task> action)
* static ContinueWhenAny(Task[] tasks, Action<Task> action)

```c#
List<Task> tasks = new List<Task>();
tasks.Add(Task.Run(() => Coding("1", "Test")));
tasks.Add(Task.Run(() => Coding("2", "Develop")));
tasks.Add(Task.Run(() => Coding("3", "Deploy")));
tasks.Add(Task.Run(() => Coding("4", "Design")));

Task.Factory.ContinueWhenAny(tasks.ToArray(), t => Console.WriteLine("First work is done."));
```



#### Parallel

> 卡界面，但是主线程参与计算，节约一个线程

* static void Invoke(params Action[] actions)

  * 并行执行多个Action，主线程也参与计算

* static void For(int startIndex,int endIndex, Action<int> action)

  * 可能并行执行一个for循环

  ```c#
  Parallel.For(0, 5, i => Coding(i.ToString(), "Test"));
  ```

* static void ForEach(IEnumerable<T> source, Action<T> action)



##### ParallelOptions

* int MaxDegreeOfParallelism
  * 设置或获取最大同时运行的并发任务数量

```c#
ParallelOptions parallel = new ParallelOptions();
parallel.MaxDegreeOfParallelism = 3;
Parallel.For(0, 10, parallel, i => Coding(i.ToString(), "Test"));
```

Parallel在线程停止的方面不是很靠谱



#### 同步与异步

* 同步操作会在返回调用者之前完成它的工作
* 异步操作会在返回调用者之后去做它的（大部分）工作
  * 异步的方法很少见，会启用并发，因为它的工作会与调用者并行执行
  * 异步方法通常很快（立即）就会返回到调用者，所以叫非阻塞方法
* 目前见到的大部分的异步方法都是通用目的：
  * Thread.Start
  * Task.Run
  * 可以将Continuation附加到Task的方法



#### 异步编程

* 异步编程的原则是将长时间运行的函数写成异步的
* 传统的做法是将长时间运行的函数写成同步的，然后从新的线程或者Task进行调用，从而按需引入并发
* 上述异步方式的不同之处在于，它是从长时间运行函数的内部启动并发。这有两点好处：
  * IO-Bound并发可以不用线程来实现，可提高可扩展性和执行效率
  * 富客户端在worker线程会使用更少的代码，简化了线程安全性
* 异步编程的两种用途
  * 编写高效处理大量并发的应用程序（典型的：服务器应用）
    * 挑战：并不是线程安全（因为共享状态是最小化的），而是执行效率
      * 特别地，每个网络请求并不会消耗一个线程
  * 在富客户端应用里简化线程安全
    * 如果调用图中任何一个操作是长时间运行的，那么整个call graph必须运行在worker线程上，以保证UI响应
      * 得到一个横跨多个方法的单一并发操作（粗粒度）
      * 需要为call graph中的每个方法考虑线程安全
    * 异步的call graph，直到需要才开启一个线程，通常较浅（IO-Bound操作完全不需要）
      * 其他的方法可以在UI线程执行，线程安全简化
      * 并发粒度适中
        * 一连串小的并发操作，操作之间会弹回UI线程
* 为了获得上述好处，下列操作建议使用异步编写
  * IO-Bound和Compute-Bound
  * 执行超过50毫秒
* 另一方面过细的粒度会损害性能，因为异步操作也有开销
* Task非常适合异步编程，因为他们支持Continuation
  * TaskCompletionSource是实现底层IO-Bound异步方法的一种标准方式
* 对于Compute-Bound方法，Task.Run会初始化绑定线程的并发
  * 把Task返回调用者，创建异步方法
  * 异步编程的区别：目标是在调用图较低的位置来这样做
    * 富客户端应用中，高级方法可以保留在UI线程和访问控制以及共享状态上，不会出现线程安全问题
* 命令式循环结构就不要和continuation混合在一起，因为它们依赖于当前状态

与传统的同步编程不同，异步编程：

```c#
static void Main(string[] args)
{
    DisplayPrimeCounts();
    Console.ReadKey();
}

static void DisplayPrimeCounts()
{
    for (int i = 0; i < 10; i++)
    {
        var awaiter = GetPrimesCountAsync(i * 1000000 + 2, 1000000).GetAwaiter();
        awaiter.OnCompleted(() => Console.WriteLine(awaiter.GetResult() +
                                                    " primes between " + (i * 1000000) + " and " + ((i + 1) * 100000 - 1)));
    }
    Console.WriteLine("Done");
}

static Task<int> GetPrimesCountAsync(int start, int count)
{
    return Task.Run(() => ParallelEnumerable.Range(start, count).Count(n =>
                                                                       Enumerable.Range(2, (int)Math.Sqrt(n) - 1).All(i => n % i > 0)));
}
```

但是这种方式有一个不好的地方，就是输出的顺序与同步不同（此时是乱序的）

需要对Task的执行序列化

* 为此，可以在continuation内部触发下一次循环

```c#
static void Main(string[] args)
{
    DisplayPrimeCounts();
    Console.ReadKey();
}

static void DisplayPrimeCounts()
{
    DisplayPrimeCounts(0);
    Console.WriteLine("Done");
}
static void DisplayPrimeCounts(int i)
{
    var awaiter = GetPrimesCountAsync(i * 1000000 + 2, 1000000).GetAwaiter();
    awaiter.OnCompleted(() => {
        Console.WriteLine(awaiter.GetResult() + 
                          " primes between " + (i * 1000000) + " and " + ((i + 1) * 100000 - 1));
        if (++i < 10)
        {
            DisplayPrimeCounts(i);
        }
    });
}

static Task<int> GetPrimesCountAsync(int start, int count)
{
    return Task.Run(() => ParallelEnumerable.Range(start, count).Count(n =>
                                                                       Enumerable.Range(2, (int)Math.Sqrt(n) - 1).All(i => n % i > 0)));
```

使用Async和Await来简化编写

```c#
static void Main(string[] args)
{
    DisplayPrimeCounts();
    Console.ReadKey();
}

static async void DisplayPrimeCounts()
{
    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine(await GetPrimesCountAsync(i * 1000000 + 2, 1000000) +
                          " primes between " + (i * 1000000) + " and " + ((i + 1) * 100000 - 1));
    }
    Console.WriteLine("Done");
}

static Task<int> GetPrimesCountAsync(int start, int count)
{
    return Task.Run(() => ParallelEnumerable.Range(start, count).Count(n =>
                                                                       Enumerable.Range(2, (int)Math.Sqrt(n) - 1).All(i => n % i > 0)));
}
```



##### BeginInvoke

是委托用来执行异步调用

`Action.BeginInvoke(AsyncCallback callback, object obj)`

callback是执行的回调函数

obj是回调函数的参数IAsyncResult的AsyncState属性

`IAsyncResult.IsCompleted`表示异步执行是否完成

主线程可以使用`asyncResult.AsyncWaitHandle.WaitOne()`来等待异步任务完成（没有延迟，内部是使用信号量机制）

可以传入参数，比如1000，表示等待1000ms，再超时就不等待了

```c#
static void Main(string[] args)
{
    Action action = new Action(PrintList);
    AsyncCallback callback = (res) => Callback(res);
    string obj = "123";
    action.BeginInvoke(callback, obj);

    Console.WriteLine("done");
    Console.ReadKey();
}


private static void PrintList()
{
    for (int i = 0; i < 100; i++)
    {
        Console.WriteLine(i);
    }
}

private static void Callback(IAsyncResult res)
{
    Console.WriteLine(res);
    Console.WriteLine(res.AsyncState);
    Console.WriteLine("Run the callback method!");
}
```

##### EndInvoke

可以异步调用委托，并获取传入的IAsyncResult最后运行的委托返回值

每一次异步操作，只能调用一次EndInvoke

```c#
Func<int> func = () =>
{
    Thread.Sleep(2000);
    return DateTime.Now.Day;
};
IAsyncResult ar = func.BeginInvoke(r => { Console.WriteLine(r.AsyncState); }, "Hello");
Console.WriteLine(func.EndInvoke(ar));
```





#### 异步函数

async和await关键字可以让你写出和同步代码一样简洁且结构相同的异步代码

* await关键字简化了附加continuation的过程

  * ```c#
    var result = await expression;
    statement(s);
    
    // 等价于
    var awaiter = expression.GetAwaiter();
    awaiter.OnCompleted(() => 
                        {
                            var result = awaiter.GetResult();
                            statement(s);
                        });
    ```

* async修饰符会让编译器把await当做关键字而不是修饰符

* async修饰符只能应用于方法（包括Lambda表达式）

  * 该方法可以返回void、Task、或者`Task<TResult>`

* async修饰符对方法签名或public元数据没有影响（和unsafe一样），它只会影响方法内部

* 在接口内使用async是没有意义的

* 使用async来重载async的方法缺失合法的

* 遇到await表达式，执行（正常情况下）会返回给调用者

  * 就像iterator里面的yield return
  * 在返回前，运行时会附加一个continuation到await的task
    * 为保证task结束时，执行跳回原方法，从停止的地方继续执行
    * 其实是将await之后的语句做成一个callback函数
  * 如果发生故障，那么异常会被重新抛出
  * 如果一切正常，那么它的返回值就会赋给await表达式

* await的表达式通常是一个Task，也可以是满足下列条件的任意对象

  * 有GetAwaiter方法，返回一个Awaiter（实现INotifyCompletion.OnCompleted接口）
  * 返回适当类型的GetResult方法
  * bool类型的IsCompleted属性

* await之后在哪个线程上执行

  * 在await表达式之后，编译器依赖于continuation（通过awaiter模式）来继续执行
  * 如果在富客户端应用的UI线程上，同步上下文会保证后续是在原线程上执行
  * 否则，就会在task结束的线程上继续执行

* 对于任何异步函数，你可以使用Task替代void作为返回类型，让该方法成为更有效的异步（可以进行await）

* 并不需要在方法体中显式返回Task。编译器会生成一个Task（当方法完成或发生异常时），这使得创建异步的调用链非常方便

* 编译器会对返回Task的异步函数进行扩展，使其成为当发送信号或发生故障时，使用TaskCompletionSource来创建Task的代码

  * ```c#
    // normal code
    Task PrintAnswerToLife()
    {
        await Task.Delay(5000);
        int answer = 21 * 2;
        Console.WriteLine(answer);
    }
    
    // code after compiling
    Task PrintAnswerToLife()
    {
        var tcs = new TaskCompletionSource<object>();
        var awaiter = Task.Delay(5000).GetAwaiter();
        awaiter.OnCompleted(() => 
                {
    				try
                    {
                        awaiter.GetResult();
                        int answer = 21 * 2;
                        Console.WriteLine(answer);
                        tcs.SetResult(null);
                    }
                    catch (Exception ex)
                    {
                        tcs.SetException(ex);
                    }
                });
        return tcs.Task;
    }
    ```

* 因此，当返回Task的异步方法结束的时候，执行就会跳回对它进行await的地方（通过continuation）

  * 在富客户端场景下，执行在此刻会跳回到UI线程（如果目前不在UI线程上）
  * 否则，就在continuation返回的任意线程上继续执行
  * 这意味着，在异步调用图向上冒泡的时候，不会发生延迟成本，除非是UI线程启动的第一次“反弹”

* 如果方法返回TResult，那么异步方法就可以返回`Task<TResult>`

  * 其原理就是给TaskCompletionSource发送的信号带有值

* 因为编译器能对异步函数生成Task，意味着

  * 大多数情况下，你只需要在初始化IO-Bound并发的底层方法内显式的初始化TaskCompletionSource，这种情况很少见
  * 针对初始化Compute-Bound的并发方法，你可以使用Task.Run来创建Task

#### 异步中的同步上下文

* 富客户端应用通常依赖于集中的异常处理事件来处理UI线程上未捕获的异常

  * 例如WPF的Application.DispatcherUnhandledException
  * ASP.NET Core中定制ExceptionFilterAttribute也是差不多的效果

* 其内部原理就是：通过在它们自己的try/catch块来调用UI事件（在ASP.NET Core里就是页面处理方法的管道）

* 顶层的异步方法会使事情更加复杂

  * ```c#
    async void ButtonClick(object sender, RoutedEventArgs args)
    {
        await Task.Delay(1000);
        throw new Exception("Will this be ignored?");
    }
    ```

  * 当点击按钮后，event handler运行时，在await后，执行会正常返回到消息循环，但1秒后抛出的异常无法被消息循环中的catch块处理

  * 为了缓解该问题，AsyncVoidMethodBuilder会捕获未处理的异常（在返回void的异步方法内），并把它们发布到同步上下文中（同步上下文存在的话），以确保全局异常处理事件能够触发

  * 注意：编译器只会把上述逻辑应用于返回类型为void的异步方法

    * 如果ButtonClick的返回类型是Task，那么未处理的异常将导致结果Task出错，然后Task无处可去（导致未观察到的异常）
    * 而且，无论在await前面还是后面抛出异常，没有区别
    * 如果同步上下文不存在，异常会在线程池上传播，从而终止应用程序

  * 不直接将异常抛回给调用者是因为确保可预测性和一致性

* 如果存在同步上下文，返回void的异步函数也会在进入函数时调用其OperationStarted方法，在函数完成时调用其OperationClosed方法
  * 如果为了对返回void的异步方法进行单元测试，而编写一个同步上下文，那么重写这两个方法很有必要



#### 优化--同步完成

* 异步函数可以在await之前就返回

  * ```c#
    static Dictionary<string, string> _cache = new Dictionary<string, string>();
    
    static async Task<string> GetWebPageAsync(string uri)
    {
        string html;
        if (_cache.TryGetValue(uri, out html))
        	return html;
        return _cache[uri] = await new WebClient().DownloadStringTaskAsync(uri);
    }
    ```

  * 如果uri在缓存中，那么就不会有await发生，执行就会返回给调用者，方法会返回一个设置好信号的Task，这就是同步完成

  * 当await一个同步完成的Task时，执行不会返回到调用者，也不通过continuation返回，而是立即执行下个语句

  * 编译器是通过检查awaiter上的IsCompleted属性来实现优化的

    * ```c#
      // 原来的code
      Console.WriteLine(await GetWebPageAsync("http://www.baidu.com"));
      
      // 同步完成时，编译器会释放可短路的continuation代码
      var awaiter = GetWebPageAsync().GetAwaiter();
      if (awaiter.IsCompleted)
      {
          Console.WriteLine(awaiter.GetResult());
      }
      else
          awaiter.OnCompleted(() => Console.WriteLine(awaiter.GetResult()));
      ```

  * 对一个同步返回的异步方法进行await，会有一个小的开销（20纳秒左右，19年PC）

  * 跳回到线程池，会引起上下文切换的开销（1-2毫秒）

  * 跳回UI消息循环，至少是10倍开销（当UI繁忙，时间会更长）

* 编写完全没有await的异步方法也是合法的，但是编译器会发出警告

  * 可以使用`Task.FromResult`，它会返回一个已经设置好信号的task

* 如果从UI线程上调用，GetWebPageAsync是隐式线程安全的

  * 一个简单的方法实现避免重复下载的task

  * ```c#
    static Dictionary<string, Task<string>> _cache = new Dictionary<string, Task<string>();
    
    Task<string> GetWebPageAsync(string uri)
    {
        if (_cache.TryGetValue(uri, out var downloadTask))
            return downloadTask;
        return _cache[uri] = new WebClient().DownloadStringTaskAsync(uri);
    }
    ```



#### ValueTask

* `ValueTask<T>`用于微优化场景
* Task和`Task<T>`是引用类型，实例化它们需要基于堆的内存分配和后续的收集
* 优化的一种极端形式是编写无需分配此类内存的代码，换句话说，这个不会实例任何引用类型，不会给垃圾收集增加负担
* 为了支持这种模式，C#引入了ValueTask和`ValueTask<T>`这两个struct，编译器允许它们代替Task和`Task<T>`
* 如果操作则同步完成的，`await ValueTask<T>`是无分配的
* 如果操作不是同步完成的，`ValueTask<T>`实际上会创建一个普通的`Task<T>`，并将await转发给它
* 使用AsTask方法，可以把`ValueTask<T>`转换给`Task<T>`(也包括非泛型版本)
* `ValueTask<T>`并不常见，出现纯粹是为了性能
* 这意味着它被不恰当的值类型语义所困扰，可能会导致意外。为了避免错误行为，必须避免以下情况：
  * 多次await同一个`ValueTask<T>`
  * 操作没结束就调用`.GetAwaiter().GetResult()`
* 如果你需要进行这些操作，那么先调用AsTask方法，操作它返回的Task



## Socket编程

![通信流程](C:\zhan\Note\编程语言\c#_pic\2.png)

### 服务端编程

```c#
using System.Net;
using System.Net.Sockets;

// 在服务器端创建一个负责监听ip地址和端口号的socket
// AddressFamily.InterNetWork表示是ipv4
Socket server = new Socket(AddressFamily.InterNetwork. SocketType.Stream, ProtocolType.Tcp);
// 提供一个IP地址，指示服务器应侦听所有网络接口上的客户端活动
IPAddress ip = IPAddress.Any; // IPAddress.Parse(txtServer.Text);

// 创建端口号对象
IPEndPoint point = new IPEndPoint(ip, Convert.ToInt32(txtPort.Text));
// 监听
server.Bind(point);
// 设置监听队列
server.Listen(10);

while(true) 
{
    // 等待客户端的连接，并创建一个负责通信的socket
    Socket serverSocket = server.Accept();
    byte[] buf = new byte[1024*1024];
    // count表示实际接收到的有效字节数
    int count = serverSocket.Receive(buffer); 
    
    // 服务端发送
    serverSocket.Send(buf);
    
    Console.WriteLine(serverSocket.RemoteEndPoint.ToString());
}
```

### 客户端编程

```c#
Socket client = new Socket(AddressFamily.InterNetwork. SocketType.Stream, ProtocolType.Tcp);
IPAddress ip = IPAddress.Parse(txtServer.Text);
IPEndPoint point = new IPEndPoint(ip, 50000);
client.Connect(point);
```

## GDI

Graphics Device Interface  是专门用来画图的接口

```c#
Graphics graphics = this.CreateGraphics(); // this是Form对象
Pen pen = new Pen(Brushes.Blue);
Point p1 = new Point(30, 30);
Point p2 = new Point(100, 100);
graphics.DrawLine(pen, p1, p2);
```

当窗体位置发生移动时，os会重新绘制form对象，但是由程序绘制的图形确不会绘制，需要在form对象的paint事件中重新绘制

### 验证码

```c#
Random r = new Random();
string data = null;
for (int i = 0; i < 5; i++)
{
    int rNum = r.Next(0, 10);
    data += rNum;
}

Bitmap bmp = new Bitmap(120, 20);
Graphics g = Graphics.FromImage(bmp);
// 画随机数
for (int i = 0; i < 5; i++)
{
    Point p = new Point(i * 20, 0);
    string[] fonts = { "微软雅黑", "宋体", "黑体", "隶书" };
    Color[] colors = { Color.Yellow, Color.Red, Color.Blue, Color.Green };
    g.DrawString(data[i].ToString(), new Font(fonts[r.Next(0, 4)], 15, FontStyle.Bold), new SolidBrush(colors[r.Next(0, 4)]), p);

}
// 画线
for (int i = 0; i < 10; i++)
{
    Point p1 = new Point(r.Next(0, bmp.Width), r.Next(0, bmp.Height));
    Point p2 = new Point(r.Next(0, bmp.Width), r.Next(0, bmp.Height));
    g.DrawLine(new Pen(Brushes.Green), p1, p2);
}
// 画随机像素点
for (int i = 0; i < 300; i++)
{
    Point p = new Point(r.Next(0, bmp.Width), r.Next(0, bmp.Height));
    bmp.SetPixel(p.X, p.Y, Color.Black);
}
pictureBox.Image = bmp;
```

## int?关键字

### int?

```c#
int? a = 1;
```

声明一个int类型，并且该int类型可以赋值为null

### ??

```c#
int? a = 1;
Console.WriteLine(a ?? 0);
```

??用作判断并且赋值：判断变量a是否为null，如果是，就可以赋值为其后的新值0，否则跳过

**注意**

GetValueOrDefault:该方法返回Nullable所赋的值，如果不存在，则返回underlying type的默认值（int类型就是0）

## 索引器

类似于class中的属性

```c#
class Program
{
    static void Main(string[] args)
    {
        var indexNames = new IndexNames();
        indexNames[0] = "Hello";
        indexNames[3] = "Hi";
        indexNames[9] = "String";
        for (int i = 0; i < 10; i++)
        {
            Console.WriteLine(indexNames[i]);
        }
    }
}

public class IndexNames
{
    private string[] nameList = new string[10];

    public IndexNames()
    {
        for (int i = 0; i < nameList.Length; i++)
        {
            nameList[i] = "N/A";
        }
    }

    public string this[int index]
    {
        get
        {
            string tmp;
            if (index >= 0 && index <= nameList.Length - 1)
            {
                tmp = nameList[index];
            }
            else
            {
                tmp = "";
            }
            return tmp;
        }
        set
        {
            if (index >= 0 && index <= nameList.Length - 1)
            {
                nameList[index] = value;
            }
        }
    }
}
```

定义索引器之后，对象可以通过与数组相同的索引方式进行访问

一个类可以定义多个字符串

**索引器在接口中的使用**

```c#
public interface ISomeInterface
{
    int this[int index]
    {
        get;
        set;
    }
}

public class IndexClass : ISomeInterface
{
    public int this[int index] 
    {
        get
        {
            return arr[index];
        }
        set
        {
            arr[index] = value;
        }
    }
    private int[] arr = new int[100];
}
```

## 委托

是一种类（数据类型），也是事件和回调函数的基础

* 事件是委托的实例
* 事件只能在声明的地方进行Invoke使用

### Action委托

用来给无返回值的方法进行委托

语法:  `Action<In> actionName = new Action<In>(methodName);`

```c#
Calculator calc = new Calculator();
Action actionDelegate = new Action(calc.Reporter);
// 直接调用
calc.Reporter();
// 间接调用
actionDelegate.Invoke();
actionDelegate();  // 简写形式

class Calculator
{
    public void Reporter()
    {
        Console.WriteLine("I will report to you.");
    }
}
```

### Func委托

用来给Action委托不起作用的其他方法进行委托

语法:  `Func<in..., out> funcName = new Func<in..., out>(methodName);`

```c#

Func<int, int, int> funcAdd = new Func<int, int, int>(calc.Add);
// 直接调用
int res = 0;
int res_1 = 0;
int res_2 = 0;
res = calc.Add(1, 2);
Console.WriteLine("Func Delegate direct invoke: {0}", res);
// 间接调用
res_1 = funcAdd.Invoke(1, 2);
Console.WriteLine("Func Delegate indirect invoke: {0}", res_1);
res_2 = funcAdd(1, 2);
Console.WriteLine("Func Delegate indirect invoke: {0}", res_2);


class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```



### 静态方法的委托

```c#
// 声明一个委托(形式参数与之后被委托的函数的形式参数一致) 返回值与函数返回值一致
delegate int NumberChanger(double n);
class Program
{
    static double num = 10;
    static void Main(string[] args)
    {
        // 创建一个委托，并把需要委托调用的函数传入
        NumberChanger numberChanger = new NumberChanger(AddNum);
        numberChanger(10);  // 让委托调用其对应的方法
        Console.WriteLine(num);  // 20
    }

    public static int AddNum(double p)
    {
        num += p;
        return (int) num;
    }
}
```

### 实例方法的委托

对于委托实例方法，声明委托的过程不变，只是在创建委托的时候，将实例化的对象的函数传入即可

```c#
MyClass myClass = new MyClass();
NumberChanger numberChanger2 = new NumberChanger(myClass.AddNum);
```

### multi-casting delegate

委托变量创建之后，会在其内部维护一个委托列表，可以通过+-操作增加或删去新委托

* 多播委托不可以异步
* 让一个变量可以保存多个方法，并且按顺序执行

```c#
static void Main(string[] args)
{
    MyClass myClass = new MyClass();
    Delegt d1 = new Delegt(myClass.M1);
    d1(10);
    // C.M1:10
    Console.WriteLine();

    Delegt d2 = new Delegt(myClass.M2);
    d1 += d2; // d1 d2  此时d1中维护了两个委托
    d1(100);
    Console.WriteLine();

    d1 -= d2;
    d1(-1);  // 此时d1中维护了一个委托
    Console.WriteLine();

}
public class MyClass
{
    public void M1(int i)
    {
        Console.WriteLine("C.M1:{0}", i);
    }
    public void M2(int i)
    {
        Console.WriteLine("C.M2:{0}", i);
    }

    public static void M3(int i)
    {
        Console.WriteLine("C.M3:{0}", i);
    }
}
```

### 异步调用

使用delegate的`BeginInvoke(AsyncCallback callback, Object object)`会自动创建一个分支线程，运行该方法，并且在方法结束之后调用callback回调函数



## 泛型

提供代码重用性

类型安全

**泛型类**

`public class MyClass<T>`

`public class MyClass<K, V>`

还可以进一步添加限制

`public class MyClass<T, K> where T : struct`

此时T必须是值类型

**泛型中的继承**

`public class SubGenericClass<T> : MyClass<T> where T : Employee`

**泛型方法**

如果方法与泛型类上声明的泛型类型相同，可以直接使用

如果不同的时候，需要额外声明（此时参数的类型只有在方法运行的时候才会被确定）

`public void GenericMethod<X>(X x) {}`

泛型方法也可以声明在静态方法上

**泛型委托**

```c#
// 声明一个泛型委托
delegate T NumberChanger<T>(T t);
// 创建委托
NumberChanger<int> nc1 = new NumberChanger(AddVal);
NumberChanger<string> nc2 = new NumberChanger(PrintVal);

public string PrintVal(string val) {
    Console.WriteLine(val);
    return val;
} 
public int AddVal(int num) {
    num += 1;
    return num;
}
```

**泛型约束**

```c#
public void Test<T>() where T:class{}  // T是引用类型
public void Test<T>() where T:struct{} // T是值类型
public void Test<T>() where T:new() {} // T有一个无参构造方法

// 类约束只能有一个，接口约束可以有多个
```



## Attribute

给程序添加一个附加的说明信息

### 内置的Attribute

* Conditional

  * 表示标注的方法在何种模式下运行

  ```c#
  using System.Diagnostics;
  
  // 该attribute表示此方法在Debug模式下运行
  [Conditional("DEBUG")]
  public static void Message(string message) 
  {
      Console.WriteLine(msg);
  }
  ```

* Obsolete

  * 表示标注的方法已经过时了

  ```c#
  using System.Diagnostics;
  
  [Obsolete("Don't use old method!", true)]
  static void Function1()
  {
      
  }
  ```

  * 第二个参数是可选的，如果是true，在调用此方法时会抛出异常

### Meta Attribute

`[AttributeUsage(AttributeTargets.Class, AllowMultiple =false, Inherited =false)]`

用来标注在自定义的Attribute类的

AttributeTargets (必须)：指定该Attribute可以作用的范围

AllowMultiple (可选)：在同一个类或方法上是否允许多次标注

Inherited (可选)：是否被继承的同事Attribute也被继承

### 自定义Attribute

创建类继承`SystemAttribute`

命名规范`XxxAttribute`

使用`[Xxx(...)]`

```c#
[AttributeUsage(AttributeTargets.Class, AllowMultiple =false, Inherited =false)]
public class HelpAttribute : Attribute
{
    protected string description;

    public HelpAttribute(string description)
    {
        this.description = description;
    }

    public string Description
    {
        get
        {
            return this.description;
        }
    }

    public string Name
    {
        get;
        set;
    }
}


[Help("This class does nothing!", Name ="optional field")]
public class MyClass {}
```

### 反射获取Attribute信息

```c#
HelpAttribute helpAttribute;
// 遍历被标注的类的所有自定义Attribute(包括被继承的)
foreach (var attr in typeof(MyClass).GetCustomAttributes(true))
{
    helpAttribute = attr as HelpAttribute;
    if (null != helpAttribute)
    {
       Console.WriteLine(helpAttribute.Description);
    }
}
```



## 反射Reflection

### System.Type

抽象类

对于对象

```c#
string s = "Hello World";
// 获取类型的方式1
Type t = s.GetType();
// 获取类型的方式2
// 第一个参数是需要查找的类型名
// 第二个参数是找不到是否报错
// 第三个参数是是否忽略大小写
Type t2 = Type.GetType("System.String", false, true);
```

对于类对象

```c#
Type t3 = typeof(string);
```

**属性**

* BaseType
* NameSpace
* FullName
* IsAbstract
* IsArray
* IsClass

**方法**

* GetConstructor(type[] types)
* GetEvent(string name)
* GetField(string name)
* GetProperty(string name)
* MethodInfo[] GetMethod(string name)
* MethodInfo[] GetMethod(string name, BindingFlags)

```c#
t.GetMethods(BindingFlags.Public | BindingFlags.Instance);
```

### 动态加载

通过反射获取到Assembly程序集对象，在通过`GetTypes`方法获取到该程序集下的所有类型

```c#
Assembly objAssembly;
objAssembly = Assembly.Load("OOP");
Type[] types = objAssembly.GetTypes();

foreach (var t in types)
{
    Console.WriteLine(t.Name);
}
```

### 延迟加载

```c#
Assembly objAssembly;
// objAssembly = Assembly.Load("OOP");
objAssembly = Assembly.GetExecutingAssembly();
// 获取到指定的类型
Type car = objAssembly.GetType("ReflectionDemo.Car", false, true);
// 通过type创建对象
object obj = Activator.CreateInstance(car);
// 获取方法
MethodInfo mi = car.GetMethod("IsMoving");
// 运行方法
int res = (int)mi.Invoke(obj, null); // 第二个参数是方法执行的参数
if (res == 1)
{
    Console.WriteLine("Car.IsMoving has been run!");
}
```

Environment.Directory

当前程序的exe或dll命令运行的文件夹目录

### Assembly

* Assembly Load(str)
  * 加载一个程序集
  * 该程序集的dll需要在启动项目中（可以通过添加引用的方式）
* Assembly LoadFile(string fullPath)
* Assembly LoadFrom(string fullPath)
* GetExecutingAssembly()
  * 获取当前正在运行的程序集

AssemblyLoadContext.Default.LoadFromAssemblyPath(string filepath)

从指定文件路径加载程序集

### 创建实例

Activator.CreateInstance(Type type)  调用无参方法返回实例

Activator.CreateInstance(Type type, object[] args)  调用有参方法返回实例

Activator.CreateInstance(Type type, bool nonPublic)  调用私有无参方法返回实例

### 运行方法

```c#
Type reflectionType = assembly.GetType("ReflectionDemo2.ReflectionClass");
object obj = Activator.CreateInstance(reflectionType);
MethodInfo show1Method = reflectionType.GetMethod("Show1");
show1Method.Invoke(obj, new object[] { });
```

### 运行泛型方法

```c#
MethodInfo show3Method = reflectionType.GetMethod("Show3");
MethodInfo show3GenericMethod = show3Method.MakeGenericMethod(new Type[] { typeof(int) });
show3GenericMethod.Invoke(obj, new object[] { });
MethodInfo show4Method = reflectionType.GetMethod("Show4");
MethodInfo show4GenericMethod = show4Method.MakeGenericMethod(new Type[] { typeof(int), typeof(string) });
show4GenericMethod.Invoke(obj, new object[] { 1, "zhangsan"});

public void Show3<T>()
{
    Console.WriteLine($"Show3 is runing: {typeof(T).FullName}");
}

public void Show4<T, V>(int id, string name)
{
    Console.WriteLine($"Show4 is runing: {id}, {name}. Types are {typeof(T).FullName}, {typeof(V).FullName}");
}
```

### 获取泛型类型

```c#
// type后加`反引号再加数字（数字代表泛型参数的个数）
Type genericType = assembly.GetType("ReflectionDemo2.GenericClass`2").MakeGenericType(typeof(int), typeof(int));
Console.WriteLine(genericType);
```

### 访问属性

```c#
Type propertyType = assembly.GetType("ReflectionDemo2.PropertyClass");
object propertyObj = Activator.CreateInstance(propertyType);
foreach (var property in propertyType.GetProperties())
{
    if ("Id".Equals(property.Name))
    {
        property.SetValue(propertyObj, 1);
    }
    else if ("Name".Equals(property.Name))
    {
        property.SetValue(propertyObj, "123");
    }
    else if ("Phone".Equals(property.Name))
    {
        property.SetValue(propertyObj, 123456789);
    }
    Console.WriteLine($"Property {property.Name} : {property.GetValue(propertyObj).ToString()}");
}

public class PropertyClass
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Phone { get; set; }
}
```





## 预处理指令

定义标志或常量

```c#
#define DEBUG  // 运行时变成DEBUG模式
#undef DEBUG   // 运行时取消DEBUG模式
#define TRACE
```

### 条件预处理指令

```c#
#if (DEBUG)
    Console.WriteLine("Debugging is enabled.");
#elif (TRACE)
    Console.WriteLine("Tracing is enabled.");
#else
    Console.WriteLine("Debugging is disabled.");
#endif
```

`#warning message` 将message输出到error list，类型是warning

`#error message`  将message输出到error list，类型是error

`#line 200 "Special"`  将当前行数变成运行时的200行，并把运行时文件名改为Special.cs

`#line default`  将当前行数变成默认行数

`#progma warining disable 414, 3021` 将编号为414和3021的warning禁用

`#progma warning restore 3021` 将编号为3021的warning恢复



## 正则表达式

#### IsMatch

```c#
using System.Text.RegularExpressions;

string[] values = { "111-22-3333", "111-2-333" };
string pattern = @"^\d{3}-\d{2}-\d{4}$";
foreach (var value in values)
{
    // 判断字符串是否符合pattern
    if (Regex.IsMatch(value, pattern))
    {
        Console.WriteLine("{0} is valid.", value);
    }
    else
    {
        Console.WriteLine("{0} is invalid.", value);
    }
}
```

### Match

第一种形式

返回第一个match，使用NextMatch方法获得下一个match，如果匹配成功，则match.Success返回true

```c#
var input = "This is China China !";
var pattern = @"\b(\w+)\W(\1)\b";  // \1表示与第一个group内容相同
Match match = Regex.Match(input, pattern);
while (match.Success)
{
    // China China
    Console.WriteLine("Duplicate {0} found.", match.Groups[1].Value);
    match = match.NextMatch();
}
```

第二种形式

返回所有匹配的项MatchCollection

```c#
MatchCollection matches;
Regex r = new Regex("abc");
matches = r.Matches("123abc4abcd");
foreach (Match match in matches)
{
    Console.WriteLine("{0} found at position {1}", match.Value, match.Index);
    // Result方法返回该match替换为replacement的值
    Console.WriteLine("{0}", match.Result("$&, hello china!"));
}
```

### Replace

```c#
var pattern = @"\b\d+\.\d{2}\b";
// 前两个$表示一个$符号
// $&表示匹配到的字符串
var replacement = "$$$&";  // 在匹配到的字符串的前面添加$符号
var input = "Total cost: 132.64";
Console.WriteLine(Regex.Replace(input, pattern, replacement));
```

### Split

```c#
string input = "1. Egg 2. Bread 3. Coffee 4. Mike";
string pattern = @"\b\d{1,2}\.\s";
foreach (var s in Regex.Split(input, pattern))
{
    if (!string.IsNullOrEmpty(s))
    { 
        Console.WriteLine(s);
    }
}
```

### Group

```c#
string input = "Born: July 28, 1989";
string pattern = @"\b(\w+)\s(\d{1,2}),\s(\d{4})\b";
Match match = Regex.Match(input, pattern);
if (match.Success)
{
    // 第0个group是匹配到的字符串全部
    // 之后是所有()包着的group
    for (int ctr = 0; ctr < match.Groups.Count; ctr++)
    {
        Console.WriteLine("Group {0}:{1}", ctr, match.Groups[ctr].Value);
    }
}
```



## 匿名方法与Lambda表达式

### 匿名方法

使用委托的形式是

```c#
delegate void TestDelegate(string s);  // 声明委托

public void Print(string s)
{
    Console.WriteLine(s);
}

// 创建委托
TestDelegate testDelegate = new TestDelegate(Print);
```

使用匿名方法的形式（C#2.0）

```c#
delegate void TestDelegate(string s);

// 使用匿名方法
TestDelegate testDelegate = delegate(string s) {Console.WriteLine(s);};
```

**匿名函数的语法**

```c#
delegate(参数列表) 
{
    函数体;
}
```

注意：参数列表不可以使用out或者ref关键字

Example

```c#
Thread t = new Thread(delegate () { Console.WriteLine("Hello, function!"); });
t.Start();
```

### Lambda表达式

使用lambda表达式的形式（C#3.0)

```c#
delegate void TestDelegate(string s);
TestDelegate test = (x) => {Console.WriteLine(x);}
```

语法

```c#
(参数列表) => {expression;}
```

lambda表达式还可以与泛型连用

Example

```c#
delegate TRes Func<TArg, TRes>(TArg arg);

Func<int, bool> func = x => x == 5;
```

注意：lambda表达式必须有参数列表

空参数： `()`

一个参数时，可以省略括号

一条语句时，可以省略大括号



## Linq

> Language Integrated Query
>
> 对实现了IEnumerable接口都可以使用Linq

* Linq to SQL
* Linq to Xml
* Linq to DataSet
* Linq to Objects

### Query Syntax

> 第一种实现方式，类似于sql语句

&&表示条件与

||表示条件或

升序：ascending

降序：discending

```c#
use System.Linq;

// 1. Data Source
int[] numbers = { 5, 10, 8, 3, 6, 12 };
// 2. Query Creation
var numQuery1 = from num in numbers  // from: Data Source
    where num % 2 == 0  // where: 过滤条件
    // group num by key // 根据key将num分组
    orderby num  // orderby: 排序规则
    select num;  // select: 查询

// 3. Query Execution
foreach (var i in numQuery1)
{
    Console.Write(i + "\t");
}
// 在使用的时候才执行query，或者强制调用query的方法
int numCount = numQuery.Count();  // 结果的个数
numQuery.ToList();
numQuery.ToArray();
```

#### group & into

```c#
List<Customer> customers = new List<Customer>();
customers.Add(new Customer() { Age = 12, Name = "zhangsan" });
customers.Add(new Customer() { Age = 12, Name = "lisi" });
customers.Add(new Customer() { Age = 13, Name = "wangwu" });
var query = from c in customers
    group c by c.Age  into cusGroup
    where cusGroup.Count() >= 2;
foreach (var cn in query)  // 得到分组
{
    Console.WriteLine(cn.Key);
    foreach (var c in cn)  // 每个分组内的所有record
    {
        Console.WriteLine("\t{0}", c.Name);
    }
}
```

#### join

```c#
List<Customer> customers = new List<Customer>();
customers.Add(new Customer() { Age = 12, Name = "zhangsan" });
customers.Add(new Customer() { Age = 12, Name = "lisi" });
customers.Add(new Customer() { Age = 13, Name = "wangwu" });
List<Employee> employees = new List<Employee>();
employees.Add(new Employee() { ID = 1, Name = "lisi" });
employees.Add(new Employee() { ID = 2, Name = "wangwu" });
employees.Add(new Employee() { ID = 3, Name = "zhaoliu" });
var query = from c in customers
    join e in employees on c.Name equals e.Name
    select new { PersonName = c.Name, PersonID = e.ID, PersonAge = c.Age };
foreach (var person in query)
{
    Console.WriteLine("{0} - {1} - {2}", person.PersonID, person.PersonName, person.PersonAge);
}
```

#### let

```c#
string[] strings = { "Hello World.", "Today is Wednesday!", "are yoU happy?" };

var query = from s in strings
    let words = s.Split(' ')
    from word in words
    let w = word.ToUpper()
    select w;
foreach (var s in query)
{
    Console.WriteLine(s);
}
```



### Method Syntax

> 第二种实现方式，使用调用链

```c#
int[] numbers = { 5, 10, 8, 3, 6, 12 };
var numQuery2 = numbers.Where(n => n % 2 == 0).OrderBy(n => n);
foreach (var i in numQuery2)
{
    Console.Write(i + "\t");
}
```



### 扩展方法

**排序**

```c#
int[] ints = { 3, 5, 1, 4, 8, 6 };
var res = ints.OrderBy(num => num);
```

### 自定义扩展方法

创建一个静态类，并将扩展方法定义为静态

```c#
public static class StringExtension  // 增强类必须是static
{
    public static int WordCount(this string str)  // 以this开头，之后是要extend的类型
    {
        return str.Split(new char[] { ' ', '.' }, StringSplitOptions.RemoveEmptyEntries).Length;
    }
}
```

然后可以直接对该类型使用定义的增强方法

```c#
string s = "This is China.";
int count = s.WordCount();
```

### 例子：对枚举类型的增强

```c#
public enum Grade
{
    A=1,
    B=2,
    C=3,
    D=4,
    E=5
}
public static class GradeExtension
{
    private static Grade minGrade = Grade.C;

    public static bool JudgeGrade(this Grade grade)
    {
        return grade <= minGrade;
    }
}
```

使用枚举的扩展类

```c#
Grade g1 = Grade.A;
Grade g2 = Grade.B;
Grade g3 = Grade.C;
Grade g4 = Grade.D;
Grade g5 = Grade.E;

Console.WriteLine(g1.JudgeGrade());
Console.WriteLine(g2.JudgeGrade());
Console.WriteLine(g3.JudgeGrade());
Console.WriteLine(g4.JudgeGrade());
Console.WriteLine(g5.JudgeGrade());
```

### IQueryable

继承自IEnumerable，区别就是IEnumerable每次查询之后在内存中过滤，IQueryable是在数据库中进行过滤



## 初始化器

### 对象初始化器

```c#
// 初始化器会覆盖构造方法注入
Student stu1 = new Student("lisi", 2, false) { Name = "zhangsan", ID = 1, IsMale = true };
```

### 匿名类

匿名类的属性都是只读的

通过初始化器进行初始化

```c#
var pet = new {Age = 10; Name = "Miaomiao"};
```

### 集合初始化器

```c#
var students = new List<Student>()
{
    new Student(),
    new Student() { FirstName = "Mei"},
    new Student() {ID=1, FirstName="Lei"}
}

Dictionary<int, string> dicts = new Dictionary<int, string>()
{
    {111, "Hello"},
    {112, "World"}
};
```



## ==与Object.Equals与ReferenceEquals三种比较

**==**

对于值类型是比较其类型的值，并且会自动进行类型转换

对于引用类型，比较两个对象的引用是否相等

**Object.Equals**

对于值类型是比较其类型的值，但不会自动进行类型转换（先比较类型再比较值）

对于引用类型，比较两个对象的引用（隐式包含对类型的检查）（如果两个变量都是null，返回true）

**ReferenceEquals**

比较两个变量的引用是否相同



## ConfigurationManager

读取project下的`App.config`文件中的配置

```c#
// 读取所有的appSettings配置
static void ReadAllSettings()
{
    try
    {
        var appSettings = ConfigurationManager.AppSettings;
        if (appSettings.Count == 0)
        {
            Console.WriteLine("AppSettings is empty.");
        }
        else
        {
            foreach (var key in appSettings.AllKeys)
            {
                Console.WriteLine("Key: {0}, Value: {1}", key, appSettings[key]);
            }
        }

    }
    catch (ConfigurationErrorsException)
    {
        Console.WriteLine("Error reading app settings");
    }
}

// 根据key读取appSettings配置
static void ReadSetting(string key)
{
    try
    {
        var appSettings = ConfigurationManager.AppSettings;
        string res = appSettings[key] ?? "Not found";
        Console.WriteLine(res);
    }
    catch (ConfigurationErrorsException)
    {
        Console.WriteLine("Error reading app settings");
    }
}

// 根据key增加或修改appSettings中的value
// 只是在运行阶段修改，不会真正改变源文件的内容
static void AddUpdateAppSettings(string key, string val)
{
    try
    {
        var configFile = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
        var settings = configFile.AppSettings.Settings;
        if (settings[key] == null)
        {
            settings.Add(key, val);
        }
        else
        {
            settings[key].Value = val;
        }
        configFile.Save(ConfigurationSaveMode.Modified);
        ConfigurationManager.RefreshSection(configFile.AppSettings.SectionInformation.Name);
    }
    catch (ConfigurationErrorsException)
    {
        Console.WriteLine("Error writing app settings");
    }
}
```

读取连接字符串connectionstring

```c#
static string ReadConnectionStrings()
{
    return ConfigurationManager.ConnectionStrings["myConnectionString"].ConnectionString;
}
```

App.config

```config
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
	<startup>
		<supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
	</startup>
	<appSettings>
		<add key="Settings1" value="May 5, 2014"/>
		<add key="Settings2" value="May 6, 2014"/>
		<add key="myKey" value="myValue"/>
	</appSettings>
	<connectionStrings>
		<add name="myConnectionString" connectionString="Data Source=...;Initial Catalog=...;Integrated Security=True;Pooling=False"/>
	</connectionStrings>
</configuration>
```



### Http发送Get&Post

#### Get

```C#
static async void GetRequest()
{
    using (var client = new HttpClient())
    {
        using (HttpResponseMessage response = await client.GetAsync("http://127.0.0.1:7000/api/product"))
        {
            Console.WriteLine(response.StatusCode);
            using (HttpContent content = response.Content)
            {
                // content.Headers
                string mycontent = await content.ReadAsStringAsync();
                Console.WriteLine(mycontent);
            }
        }
    }
}
```



#### Post

```c#
using (var client = new HttpClient())
{
    using (HttpContent requestContent = new StringContent("{\"ID\":\"223\", \"Name\":\"XXXX\", \"Price\":\"988\", \"Description\":\"66666!\"}", 
                                                          Encoding.UTF8, "application/json"))
    {
        using (HttpResponseMessage response = await client.PostAsync("http://127.0.0.1:7000/api/product", requestContent))
        {
            Console.WriteLine(response.StatusCode);
            //using (HttpContent responseContent = response.Content)
            //{
            //    string mycontent = await responseContent.ReadAsStringAsync();
            //    Console.WriteLine(mycontent);
            //}
        }
    }
}
```



## 导出Excel

1. 创建并导出

```c#
string excelPath = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, DateTime.Now.ToString("yyyyMMdd_HHmmss") + ".xlsx");
Application excel = new Application();
//excel.Visible = false;
Workbook workbook = excel.Workbooks.Add(System.Reflection.Missing.Value);
Worksheet worksheet = workbook.Worksheets[1];

int rowNum = 1;
worksheet.Cells[rowNum, 1] = "IDO Method Name";
worksheet.Cells[rowNum, 2] = "Activity Type";
worksheet.Cells[rowNum, 3] = "Comment";

workbook.SaveAs(excelPath, Type.Missing, Type.Missing, Type.Missing, Type.Missing, Type.Missing, XlSaveAsAccessMode.xlNoChange, Type.Missing, Type.Missing, Type.Missing);
workbook.Close(false, Type.Missing, Type.Missing);
excel.Quit();
```

2. open并导出

```c#
using Microsoft.Office.Interop.Excel;

public static bool ToExcel(System.Data.DataTable dt, string excelPath, string outputPath)
{
    Application excel = new Application();
    Workbook wbk = excel.Workbooks.Open(excelPath);
    Worksheet ws = wbk.Sheets[1];
    for (int i = 0; i < dt.Rows.Count; i++)
    {
        for (int j = 0; j < dt.Columns.Count; j++)
        {
            Microsoft.Office.Interop.Excel.Range r = ws.Cells[i + 1, j + 1];
            r.Value = dt.Rows[i].ItemArray[j];
        }
    }

    if (!string.IsNullOrEmpty(outputPath)) wbk.SaveAs2(outputPath);
    wbk.Save();
    excel.Quit();
    return true;
}
```



## 表达式目录树

> 会将表达式根据后缀表达式的拆分变成一个运算符的二叉树
>
> ```c#
> // 调用compile方法会生成一个委托类型
> Expression<TDelegate> expr = TDelegate;
> TDelegate delegate1 = expr.Compile();
> ```
>
> 

注意：

* 只能一行，不能有大括号
* 可以使用lambda表达式生成表达式目录树



### 声明表达式目录树

快捷声明

```c#
Expression<Func<int, int, int>> exp = (m, n) => m * n + 2;
int res = exp.Compile().Invoke(3, 4);
Console.WriteLine(res);
```

自己拼接表达式目录树

```c#
// 拼接1
// () => 345 + 456
ConstantExpression constantLeft = Expression.Constant(345);
ConstantExpression constantRight = Expression.Constant(456);
BinaryExpression binary = Expression.Add(constantLeft, constantRight);  // 345+456
Expression<Action> actionExpression = Expression.Lambda<Action>(binary, null);
actionExpression.Compile()();

// 拼接2
ParameterExpression parameterExpr = Expression.Parameter(typeof(People), "x");
var property = Expression.Property(parameterExpr, typeof(People).GetProperty("Id"));
var toString = typeof(object).GetMethod("ToString");
MethodInfo equals = typeof(object).GetMethod("Equals", BindingFlags.Instance | BindingFlags.Public);
var body = Expression.Call(Expression.Call(property, toString, new Expression[0]),
                           equals, new Expression[] { Expression.Constant("5", typeof(string)) });
Expression<Func<People, bool>> expression = Expression.Lambda<Func<People, bool>>(body, parameterExpr);
var res2 = expression.Compile().Invoke(new People { Id = 5 });
```



### 修改表达式目录树

使用`public abstract class ExpressionVisitor`的`Visit(Expression expression)`方法

Visiti方法会递归去访问表达式目录树的每一个节点，可以重写一些`VisitXxx`方法来指定一些访问某些Node的规则



## 加密解密

### MD5 不可逆加密

> 可以通过`撞库`来解密

```c#
//MD5类是抽象类
MD5 md5 =MD5.Create();
//需要将字符串转成字节数组
byte[] buffer = Encoding.Default.GetBytes("123");
//加密后是一个字节类型的数组，这里要注意编码UTF8/Unicode等的选择
byte[] md5buffer = md5.ComputeHash(buffer);
string str = null;
// 通过使用循环，将字节类型的数组转换为字符串，此字符串是常规字符格式化所得
foreach (byte b in md5buffer)
{
    //得到的字符串使用十六进制类型格式。格式后的字符是小写的字母，如果使用大写（X）则格式后的字符是大写字符 
    //但是在和对方测试过程中，发现我这边的MD5加密编码，经常出现少一位或几位的问题；
    //后来分析发现是 字符串格式符的问题， X 表示大写， x 表示小写， 
    //X2和x2表示不省略首位为0的十六进制数字；
    str += b.ToString("x2");
}
Console.WriteLine(str);//202cb962ac59075b964b07152d234b70
```

* 用途
  * 重复文件检索
  * 文件完整性验证
  * 数据库密码
  * git/svn比对文件

* 其他MD5加密
  * 加盐（在密码后加上某个固定的字符串）
  * 双MD5



### DES 对称可逆加密

DES加密

```c#
public static string Encrypt(string stringToEncrypt, string sKey)
{
    DESCryptoServiceProvider des = new DESCryptoServiceProvider();
    byte[] inputByteArray = Encoding.GetEncoding("UTF-8").GetBytes(stringToEncrypt);
    des.Key = ASCIIEncoding.UTF8.GetBytes(sKey);
    des.IV = ASCIIEncoding.UTF8.GetBytes(sKey);
    MemoryStream ms = new MemoryStream();
    CryptoStream cs = new CryptoStream(ms, des.CreateEncryptor(), CryptoStreamMode.Write);
    cs.Write(inputByteArray, 0, inputByteArray.Length);
    cs.FlushFinalBlock();
    StringBuilder ret = new StringBuilder();
    foreach (byte b in ms.ToArray())
    {
        ret.AppendFormat("{0:X2}", b);
    }
    ret.ToString();
    return ret.ToString();
}
```

DES解密

```c#
public static string Decrypt(string stringToDecrypt, string sKey)
{
    DESCryptoServiceProvider des = new DESCryptoServiceProvider();
    byte[] inputByteArray = new byte[stringToDecrypt.Length / 2];
    for (int x = 0; x < stringToDecrypt.Length / 2; x++)
    {
        int i = (Convert.ToInt32(stringToDecrypt.Substring(x * 2, 2), 16));
        inputByteArray[x] = (byte)i;
    }
    des.Key = ASCIIEncoding.UTF8.GetBytes(sKey);
    des.IV = ASCIIEncoding.UTF8.GetBytes(sKey);
    MemoryStream ms = new MemoryStream();
    CryptoStream cs = new CryptoStream(ms, des.CreateDecryptor(), CryptoStreamMode.Write);
    cs.Write(inputByteArray, 0, inputByteArray.Length);
    cs.FlushFinalBlock();
    StringBuilder ret = new StringBuilder();
    return Encoding.UTF8.GetString(ms.ToArray());
}
```



### RSA 非对称可逆加密

* 防止信息被篡改
* 数字签名

生成公钥和私钥

```c#
string publicKey, privateKey;
RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();
privateKey = rsa.ToXmlString(true);
publicKey = rsa.ToXmlString(false);
```

加密解密

```c#
public static string Encrypt(String sSource, string publicKey) // 加密
{
    RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(publicKey);
    byte[] cipherbytes = rsa.Encrypt(Encoding.UTF8.GetBytes(sSource), false);

    return Convert.ToBase64String(cipherbytes);
}

public static string Decrypt(string sSource, string privateKey)  // 解密
{
    RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(privateKey);
    byte[] cipherbytes = rsa.Decrypt(Convert.FromBase64String(sSource), false);

    return Encoding.UTF8.GetString(cipherbytes);
}
```



### SSL 数字证书

CA:

1. 持有者姓名
2. 发证机关
3. 有效日期
4. 证书持有人的公钥
5. 扩展信息
6. 发证机关对该证书的数字签名（使用CA的私钥）

![image-20220118201055841](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20220118201055841.png)



## 垃圾回收

CLR的内存是连续分配的

只有完全访问不到的引用类型需要被回收

主动回收使用`GC.Collect()`

被动回收只有在每次new对象的时候或者程序退出的时候，会去检查是否要回收

静态属性是全局唯一，永远不会被回收

GC时其他线程都停止



**分级策略**

1. 首次GC前，全部对象都是0级
2. 第一次GC后，还保留的对象是1级
3. 回收先找0级对象，如果空间还不够，再去找1级对象，这之后还存在的对象是2级

越是最近分配的，越是会被回收



**大对象策略**

大对象 内存是链式分配的

如果大对象大于85KB，就单独管理

避免频繁的内存移动



## CommandLine

> Nuget: ComandLineParser

Demo:

```java
using System;
using System.Collections.Generic;
using CommandLine;

namespace Walterlv.Demo
{
    class Program
    {
        public class Options
        {
            [Option('f', "file", Required = true, HelpText = "需要处理的文件。")]
            public IEnumerable<string> Files { get; set; }

            [Option('o', "override", Required = false, HelpText = "是否覆盖原有文件。")]
            public bool Override { get; set; }
        }

        static void Main(string[] args)
        {
            Parser.Default.ParseArguments<Options>(args).WithParsed(Run);
        }

        private static void Run(Options option)
        {
            // 使用解析后的命令行参数进行操作。
            foreach (var file in option.Files)
            {
                var verb = option.Override ? "覆盖" : "使用";
                Console.WriteLine($"walterlv 正在{verb}文件 {file}");
            }
        }
    }
}
```

命令行参数

```cmd
dotnet demo.dll -f C:\Users\lvyi\Desktop\Test.txt
```



## DbType转CSharp代码

```c#
public static Type DbType2CSharpType(DbType dbType)
{
    Type t = null;
    switch (dbType)
    {
        case DbType.AnsiString:
        case DbType.AnsiStringFixedLength:
        case DbType.String:
        case DbType.StringFixedLength:
        case DbType.Xml:
            t = typeof(string);
            break;
        case DbType.Binary:
            t = typeof(byte[]);
            break;
        case DbType.Byte:
            t = typeof(byte);
            break;
        case DbType.Boolean:
            t = typeof(bool);
            break;
        case DbType.Currency:
        case DbType.Decimal:
        case DbType.VarNumeric:
            t = typeof(decimal);
            break;
        case DbType.Date:
        case DbType.DateTime:
        case DbType.DateTime2:
        case DbType.DateTimeOffset:
        case DbType.Time:
            t = typeof(DateTime);
            break;
        case DbType.Double:
            t = typeof(double);
            break;
        case DbType.Guid:
            t = typeof(Guid);
            break;
        case DbType.Int16:
            t = typeof(short);
            break;
        case DbType.Int32:
            t = typeof(int);
            break;
        case DbType.Int64:
            t = typeof(long);
            break;
        case DbType.Object:
            t = typeof(object);
            break;
        case DbType.SByte:
            t = typeof(sbyte);
            break;
        case DbType.Single:
            t = typeof(float);
            break;
        case DbType.UInt16:
            t = typeof(ushort);
            break;
        case DbType.UInt32:
            t = typeof(uint);
            break;
        case DbType.UInt64:
            t = typeof(ulong);
            break;

        default: 
            // return null 
            break;
    }
    return t;
}
```



