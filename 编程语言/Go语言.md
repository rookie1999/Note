# Go语言
## 1 Go语言概述
### 1.1 Go语言特点
* 优点
    * 开源
    * 编译型语言
    * 支持交叉编译、编译快速
    * 开发效率高
    * 执行性能好
    * 天生支持并发、
* 特点
    * 语言简洁
    * 开发效率高
    * 执行性能好
### 1.2 Go语言环境搭建
1. 下载[Go安装包](https://golang.google.cn/doc/install?download=go1.14.1.windows-amd64.msi)
2. cmd中输入`go version`出现版本号则安装成功
3. 创建workspace，并将该目录添加到环境变量中，名为GOPATH
4. 在该目录下创建三个文件夹
    1. src：存放源码
    2. pkg：存在编译之后的文件
    3. bin：存放可执行文件，该目录需要添加到path环境变量中
5. 在cmd中输入`go env`  出现结果，且GOPATH的值为修改后的值即成功
6. 在VS Code中安装Go扩展包，在extensions中搜索go插件
7. 配置快捷键，输入`ctrl+shift+p"`跳出菜单，输入`snippets`进行编辑
### 1.3 Go语言开发的项目结构
* 三个目录`src`,`pkg`,`bin`
* `src`目录
    * 对于个人开发者，可以直接将项目放在src目录下
    * 对于企业开发，在src目录下建立一级文件夹`网站域名`，二级文件夹`作者`，下面是各个项目名
### 1.4 第一个程序helloword
```
// 包为main，指此包最终会编译成可执行文件
// 其他名称的包 不会生成可执行文件
package main

// 导入语句
import "fmt"

// 函数体外只能放变量、常量、函数、类型的声明

// main函数，程序的入口，无参无返回值
func main() {
	fmt.Println("Hello world!")
}
```

* 编译源代码
    * `go build`  在项目目录下使用该命令，可以对整个项目进行编译，生成可执行文件为项目名
    * 如果不在项目目录下，可以使用`go build <项目的相对路径>`，这里的相对路径是相对于GOPATH/src来说的，并且生成的二进制文件在当前目录下
    * 如果不像生成的文件是项目名称，可以使用`-o <新名称>`
    * `go run`  直接运行go文件，像运行脚本文件一样，如`go run main.go`
    * `go install`分成两步
        * 先编译项目得到可执行文件
        * 将该文件保存在`GOPATH/bin`目录

### 1.5 跨平台编译
Go支持跨平台编译
1. 在windows平台编译出一个可以在Linux上运行的可执行文件
```
set CGO_ENABLED=0  // 禁用CGO
SET GOOS=linux        // 目标平台是linux（如果是Mac，就是darwin）
SET GOARCH=amd64    // 目标处理器的架构是amd64
```
2. Mac下编译Linux和Windows平台的64位程序
```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build
```
3. Linux下编译Mac和Windows平台64位程序
```
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build
```

## 2 Go语言基础
### 2.1 变量和常量
* 标识符：以字母、数字、下划线组成，以字母或下划线开头
#### 2.1.1 变量声明
* Go语言的函数内部变量`非全局变量`必须先声明再使用
* 函数外部`全局变量`的变量声明之后不使用
* 通过var关键字进行声明
* Go语言是静态类型语言，需要在声明时指定其数据类型
* 变量声明
    * var 变量名 变量类型
    * 系统自动对变量对应的内存区域进行初始化
```
    // 声明变量
    var name string
    var age int
    var isOk bool
```
* 批量声明
```
    var (
        name string    // ""
        age int        // 0
        isOk bool      // false
    )
```
* ***注意***：变量一旦进行声明就必须在程序中使用，否则编译失败
* 变量的命名与Java相同，使用驼峰命名法

### 2.1.2 变量初始化

* 格式

  var s1 string = "abc"

### 2.1.3 类型推导

* 如果在声明时进行初始化，可以忽略数据类型，编译器会根据值来推导变量的类型

  var s1 = "abc"

### 2.1.4 简短变量声明

* 在*函数内部*，可以使用更简略的方式`:=`声明并初始化变量（推荐使用）

  s1 := "abc"

### 2.1.5 匿名变量

* 在使用多重赋值时，如果想要忽略某个值，可以使用匿名变量

* 匿名变量用一个下划线 `_` 表示

* 当函数返回多个值时，不想要其中某个返回值，可以使用匿名变量接收

  x, _ := foo()

* 匿名变量不占用命名空间，不会分配内存，所以匿名变量不存在重复声明

### 2.1.6 注意事项

1. 函数外的每个语句都必须以关键字开始（var、const、func等）
2. `_`多用于占位，表示忽略值

### 2.1.7 常量的声明

* 常量的声明

```
const a = 10	// a为常量，值是10，之后无法被修改
```

* 常量的批量声明

```
const (
	a = 10
	b = 11
	c = 12
)
```

* 批量声明中若只声明了变量没有进行初始化（由于常量一经定义就必须存储一个不变的值），则自动初始化为上一行的值

### 2.1.8 iota

* iota是go语言的常量计数器，只能在常量的表达式中使用
* iota在const关键字出现时就重置为0，const中每新增一行常量声明将使iota自增1（iota的本质是当前const关键字内的行索引）。使用iota能简化定义，在定义枚举时很有用

```
const (
	n1 = iota  // 0
	n2 = 100   // 100
	n3         // 2
	n4         // 3
)
const n5 = iota // 0
```

* 可以使用匿名变量`_`跳过某些值
* 多个变量声明在一行

```
const (
	a, b = iota + 1, iota + 2	// a = iota + 1 = 1, b = iota + 2 = 2, iota = 0
	c, d				// c = iota + 1 = 2, d = iota + 2 = 3, iota = 1
	e, f				// e = iota + 1 = 3, f = iota + 2 = 4, iota = 2
)
```

### 2.2 基本数据类型

```
Go语言中有丰富的数据类型，除了基本的整型、浮点型、布尔型、字符串之外，还有数组、切片、结构体、函数、map、通道(channel)等。
```

#### 2.2.1 整型

* 整型分为两大类
  * 带符号整型：int8, int16, int32, int64
  * 无符号整型：uint8, uint16, uint32, uint64
* 特殊整型
  * int：32位操作系统上是int32，64位操作系统上就是int64
  * uint：32位操作系统是uint32，64位操作系统是uint64
  * uintptr：无符号整型，用来存放一个指针
* 例如，声明int类型

```
i := int8(9)	// 9默认是int类型，需要强制转换
```

#### 2.2.2 查看变量类型

* 使用%T格式输出

```
fmt.Printf("i's type is %T\n", i)	// int8
```

#### 2.2.3 浮点型

* Go语言支持：`float32` 和`float64` 
* float32的最大范围约为3.4e38，用常量`math.maxFloat32`定义
* float64的最大范围约为1.8e308，用常量`math.maxFloat64`定义
* Go语言中小数常量的类型都是float64
* float32类型的值不能够直接赋值给float64

#### 2.2.4 复数

* Go语言支持：`complex64`和`complex128`
* 复数有实部和虚部，对于`complex64`来说，其实部和虚部为32位；对于`complex128`来说，其实部和虚部为64位
* 输出格式与浮点数相同

#### 2.25 布尔型

* Go语言用`bool`类型声明布尔型数据
* 注意：
  1. 布尔类型默认值为`false`
  2. Go语言不允许将整型强制转换为布尔型
  3. 布尔型无法参与数据运算，也无法与其他类型进行转换
* 可以用` %v `进行输出，` %v ` 是将`变量的值` 进行输出，`%#v`是将变量的值加上描述符输出（如字符串会加上双引号）