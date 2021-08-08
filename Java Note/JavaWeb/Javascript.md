# Javascript

练习代码位置：D:\idea\IntelliJ IDEA 2019.1.3\project

## JS简介

动态  弱类型   基于原型

动态：只有在执行时，才能确定数据类型

弱类型：变量数据的类型是不确定的，可以随意进行改变

* 原型：新对象继承对象（作为模板），将自身的属性共享给新对象，模板对象称为原型。这样新对象实例化后不但可以享有自己创建时和运行时定义的属性，而且可以享有原型对象的属性

> 一个完整的浏览器包含浏览器内核和浏览器的外壳（Shell）
>
> 浏览器核心——分成两部分：渲染引擎和js引擎

2013年以前

* IE——Trident
* Firefox——Gecko
* Safari/chrome——Webkit
* Opera——Presto

2013年以后

* chrome/opera——blink
* 国内的chrome系浏览器也开始使用blink



## JS严格模式

对js的语法的检查更为严格

```
'use strict';
```



## Javascript变量
1. NaN是Number类型的
2. 五种基本数据类型： number，string，boolean，null，undefined
3. 其他数据类型转number
    1. string转number，按照字面值转换，如果不成功，则是NaN（一般直接使用-0，/1，*1进行转换）
    2. boolean转number，true为1，false为0
4. 其他数据类型转boolean
    1. number转boolean：0或***NaN***为false，其余为true
    2. string转boolean：“”(空字符串)为false，其余为true
    3. null和undifined：false
    4. 对象：true
5. 通过var声明的变量是局部作用域，而JS允许不通过var声明变量，这时变量的作用域为全局作用域（实际上是给window定义了一个属性）     ***重要***
6. typeof 的返回值
    1. number
    2. string
    3. boolean
    4. undefined
    5. object
    6. function
7. 对函数对象使用typeof运算符返回的是**function**，要说明的是function是隶属于object的，只是这里强调该对象是可以执行的
8. 重复定义变量，最后一次的定义会覆盖上面的变量
9. NaN是指运算结果不是数字，isNan(variable)判断variable是不是NaN，如果是返回true
10. undefined是声明变量但没有赋值时候的值
11. undefined直接运算结果也是NaN，即undifined不可以当作0，但是null在运算的时候转换成0
12. null可以用来清空对象
13. 注意：undefined == null  返回值是true；undefined === null返回false
14. JS中数值型所能表达的最大值是Number.MAX_VALUE，超过的值就是Infinity（正无穷）和-Infinity（负无穷）
15. 浮点数比较可能会有问题   因为浮点数计算的结果并不精确



## 流程控制语句
* switch与java不同，可以接收任意类型
* 循环
  * for
  * while
  * do while



## JS内置函数

* alert(str)
* prompt('提示文字', 默认值)  返回值是字符串类型
* confirm(str)     返回值是true或false



## JS作用域
1. 全局作用域
    * 作用范围是当前页面
    * 直接在script标签内声明的变量
    * 直接赋值没有声明的变量
    * 细节：
        * 在全局作用域中创建的变量，会作为window对象（js内置对象）的属性
        * 在全局作用域中创建的方法，会作为window对象的方法
2. 函数作用域（局部作用域）
    * 在函数内部声明的变量的作用范围只能在当前函数体内
3. 变量和函数的声明提前
    * 对于通过用var关键字进行声明的变量，会将变量的声明放至当前作用域的开始（但不赋值，赋值仍然是在原来的位置）
    * 对于通过function 函数名(){}这种形式声明的函数，会将函数的声明（即函数名和函数体）放至当前作用域的最前面
4. 函数和方法的调用与this
    * 在函数或方法调用的时候会传入一个隐含对象this
    * 如果是调用***函数***，则this对象是window对象
    * 如果是调用***对象的方法***，则this对象是方法所属的对象
    * 如果是调用***构造函数***，this是当前构造函数创建的实例
    * 用处之一：对于同一类对象，如果有同一个name属性，可以创建同一个方法操作其name属性，通过this关键字可以达到对自身的name的操作



## JS数组

1. 创建数组
   1. new Array(): 创建一个空数组
   2. new Array(num):创建一个长度为num的数组，但是此时的值都是undefined
   3. new Array(1, 2, 3):创建一个数组，含有元素1，2，3
   4. var a = [];  创建一个数组（推荐）
   
2. 方法

   * push(var)

     * 在数组最后插入var

   * pop

     * 在数组最后删除var

   * unshift(var)

     * 在数组首位插入var

   * shift

     * 删除数组的第一个元素

   * slice(start, end)

     * 返回数组start到end（不包括end）
     * 第二个参数不写就是默认到数组末尾
     * 第二个参数是负值，则从后往前计算
     * 不会影响原数组

   * splice(start，length，var...)

     * 可以用来删除数组的指定元素
     * 使用splice会影响到原数组
     * 将删除的元素返回
     * 第三个及之后的参数可以传递新的数组元素，这些元素会自动插入到start索引之前

   * concat(arr2)

     * 连接两个数组并返回
     * 不会对原数组产生影响

   * join

     * 将数组转换成一个字符串
     * 不会对原数组产生影响
     * 在join方法中指定一个字符串，会作为数组元素连接的连接符

   * reverse

     * 反转数组，对原数组产生影响

   * sort

     * 对数组进行排序（默认按照Unicode编码）
     * 对数组产生影响
     * 可以传入一个参数——回调函数callback来指定排序规则

     ```js
     function callback(a, b) {
         如果返回一个大于0的数，交换位置
         返回0，表示这两个元素相等，不交换位置
         小于0，不交换位置
     }
     ```

     



## JS对象

* 删除对象的属性
  * delete 对象.属性
* 要使用特殊的属性名
  * 需要使用obj["123"]这种形式

```js
var n = "nihao";
var obj = {};
obj[n] = 123;
console.log(obj[n]);
```

* 检查某个属性是否在对象中

  * in运算符
  * 语法：“属性名” in “对象”

  ```js
  console.log(document in window);
  ```

### Function对象
1. 创建
    * 其实就是声明一个函数（有三种方法，掌握两种）
2. 属性
    * length属性：方法的形式参数个数
3. 方法
    * call：调用function对象声明的内容，传入的第一个参数可以改变调用函数执行时的this，之后的参数为调用函数传入的参数
    * apply：与call类似，但是对于执行时需要的参数，必须封装到一个数组中（类似arguments对象，但不是，因为arguments对象不是数组）
注：两种方法都是用来改变函数执行时的this，但如果不传，this为window对象（所以调用对象的方法如果想改变this为window，可以使用这种方式）
4. 特点
    * 包含一个隐藏的内置对象，对象名为arguments，是一个数组，封装了实际参数
    * arguments对象有一个属性，是callee，即当前执行的函数对象

#### 立即执行函数

匿名函数，只执行一次

```js
(function() {
    alert("hello!");
})();
```



### 构造函数
1. 构造函数与普通函数的区别
    * 构造函数的首字母需要大写
    * 构造函数如果直接调用，与普通函数一样；如果通过new关键字进行调用，才是构造函数
2. 构造函数的调用过程
    1. 创建一个对象，类型是以构造函数名字为类型（隐式）
    2. 将构造函数中的this，设置为新创建的对象（隐式）
    3. 逐行执行代码
    4. 返回新创建的对象（隐式）
3. instanceof关键字
    1. a instanceof b
    2. 检查对象a是否是一个类（构造函数）b的实例
4. 所有的对象都是Object的后代
    1. 任何对象instanceof Object的返回值都是true
5. 在构造函数中声明方法和每个实例相同的变量有缺点：因为对于构造函数中的属性和方法，会在每一次实例创建的时候再创建一次，影响内存
    * 解决办法：可以使用原型对象来解决

### 原型对象Prototype
1. 在函数创建的时候，解析器会自动为函数添加一个属性prototype，该属性的类型是Object
2. prototype属性对于普通函数没有作用
3. 当函数以构造函数形式调用时，会在创建的实例中添加一个额外的属性，该属性的属性名是`__proto__`，并且属性的类型是Object，指向构造函数原型所指对象
4. 原型对象相当于一个***公共区域***，每一个实例都可以访问到其中的内容。因此，对于每一个实例共有的属性或方法，最好设置到构造函数的原型对象中，这样就不用为每一个实例重新创建一次，并且不会影响全局作用域
5. 访问一个对象的属性或方法时，首先会在对象自身的属性或方法中寻找；如果找不到，就在原型对象中找；如果还是找不到，就设置为undifined
6. 对于函数的原型对象，它的原型对象是`__proto__`,对象的原型对象也有原型对象
    * 使用in关键字可以判断一个对象是否含有某个属性，会去***对象本身的属性和其原型对象***中寻找 eg.    "name" in obj
    * 使用hasOwnProperty方法判断一个对象是否含有某个属性，只会在***对象本身的属性***中寻找    eg.    obj.hasOwnProperty("name")
7. 原型对象有原型对象作为属性，一直找到Object为止，Object的原型对象是null
    1. `obj.__proto__.__proto__.__proto__ = null`

**注：打印对象的时候，默认调用的是原型中的toString方法**

### 垃圾回收机制
1. JS中也有与Java类似的垃圾回收机制，对于堆内存中没有变量或属性指向的空间，垃圾回收器会在特定的时间对其进行清理
2. 但是与Java不同的是，JS中的垃圾回收需要**显式**将对象赋值为null

### 内置对象
#### Array数组对象
数组的存储效率比自定义对象要高
1. length属性：默认返回最大索引+1        可以通过编程设置（数组对于空出来没有赋值的位置，默认是undifined）
2. 数组中可以添加任意的数据类型，包括对象，函数，数组（多维数组）
3. 数组的几种构建方式
    1. 字面量：对于想要直接在创建的时候就赋值的数组（可以在赋值的时候  索引中不放置任何东西  默认是undifined）  形式：[1, 2, 3]
    2. Array(length)：使用构造函数，创建一个长度为参数的数组
    3. Array(Object... args)：使用构造函数，创建一个数组，值依次为参数（但要求参数的数量必须多于一个）
5. 操作数组的四个方法
    * push(Object... obj)：在数组的末尾添加一个或多个元素，并返回新数组的长度
    * pop():删除数组的最后一个元素，并返回被删除元素的值
    * unshift(Object... args)：在数组的开头增加一个或多个元素，并返回新数组的长度
    * shift():删除数组第一个元素，并返回新数组的长度
6. forEach方法：用来遍历数组
    * 对于IE浏览器，只有IE9及以上的浏览器支持（所以考虑到兼容性问题，一般该方法用在移动端）
    * 需要传入一个函数，该函数为回调函数（即由浏览器自行调用的函数），一般使用匿名函数
    * 函数执行的次数就是数组的长度
    * 浏览器调用该函数时，会传入三个实参
        * 第一个实参是数组的元素的值（递增索引）
        * 第二个实参是数组的索引（递增）
        * 第三个参数是数组本身
7. slice和splice方法
    * slice(start, end):返回数组从start开始到end-1的子数组，如果end为负值，则从前往后计算
    * splice(start, length, Object... args):将数组start索引开始删除长度为length的子数组返回，并在原数组的start位置增加args的参数（会对原数组产生影响）
        * 该功能可以对原数组的元素进行获取，也可以在原数组中插入新元素（此时length=0），删除元素（不传递args参数），替换元素
8. 数组的其余方法
    * concat():可以用来连接一个或多个数组，也可以直接传入元素
    * join()：将数组转为字符串，默认用“，”分隔。同时，也可以传入参数，即指定分隔符
    * reverse():反转数组（对原数组产生影响）
    * sort(回调函数(可选)):对原数组进行排序，默认按照Unicode编码进行排序（会改变原数组）
        * 对数字数组进行排序的时候，可能会产生错误的影响，所以需要指定具体的回调函数
        * 对于回调函数，每次浏览器调用的时候会传入两个参数，并最终根据函数的返回值进行元素的交换
            * 如果返回大于0的数，则两个数交换位置
            * 返回小于0的数，不变
            * 返回0，相等，仍不变
#### Date对象
Date属于Function类型
1. 创建方式
    * 创建当前日期对象
      
        * var date = new Date();
        
    * 创建指定日期对象
      
        ```js
        var date = new Date("12/31/2016 12:11:11");
        ```
        
        * var date = new Date("yyyy-MM-dd hh:mm:ss");
        * var date = new Date("yyyy/MM/dd hh:mm:ss");
        * var date = new Date("MM/dd/yyyy hh:mm:ss");
2. 方法
    * getDate:    返回日期在月中的天数
    * getDay:    返回日期在星期中的天数，取值范围是0-6（星期天为0）
    * getMonth:返回日期的月份，取值范围是0-11
    * getFullYear:    返回年份（此方法替代了getYear）
    * getTime:    返回日期对象的时间戳（由于日期的单位有很多，计算机为了统一管理与存储，底层存储的是日期的时间戳），从格林尼治时间1970年1月1日到现在经过的毫秒数
    * Date.now():    返回当前时刻的时间戳
#### Math对象
Math属于Object
1. 不需要创建，可以直接使用其方法
2. 方法
    * ceil ：小数进一法
    * floor ：小数去尾法
    * round ：小数四舍五入法
    * abs ： 返回绝对值
    * random ：返回0-1之间的随机数（不包括0和1）
        * 生成x到y闭区间的随机数：**Math.round(Math.random * (y - x) + x)**
    * max ：返回多个数中的最大值
    * min ：返回多个数中的最小值
#### 包装类
1. 是对于基本数据类型的包装
    * Number
    * String
    * Boolean
2. 不推荐在日常开发中使用，其产生的目的是为了给浏览器使用
3. 一般来说，方法和属性只能加给对象，不能加给基本数据类型，但是基本数据类型可以调用属性或方法，其原因在于每次调用属性或方法时，浏览器会临时生成一个该类型的包装类，调用完相应的方法之后，再将它转换回去（自动拆箱装箱），但是每次装箱都是一个临时的新对象
4. String包装类的一些常用属性及方法
    * 底层是以字符数组的方式进行存储，可以直接通过**索引**来访问元素
    * length属性：返回字符串的字符数
    * chartAt：返回具体索引上的字符
    * charCodeAt：返回具体索引上的字符的Unicode编码
    * concat：连接多个字符串，但更好的方法是直接通过“+”号进行连接
    * indexOf：如果只有一个参数，返回该字符第一次出现的索引，不存在就返回-1；如果有两个参数，就返回从第二个参数为索引开始的第一次出现第一个参数字符的索引
    * lastIndexOf：与indexOf不同的是，该方法是从后往前找
    * slice：与数组的slice方法一样，返回指定索引区间的元素（包含头不包含尾）
        * 第二个参数如果是负数，则末尾从后往前数
    * substring：基本与slice一样，但是如果第二个参数为负数时，当作0看待；并且如果第一个参数>第二个参数，会自动进行交换
    * substr：虽然每一个浏览器都支持，但是ecma没有对该方法进行标准化，所以不推荐使用（了解即可）
        * 第一个参数是起始索引的位置
        * 第二个参数是长度（如果没有，默认为当前索引到字符串末尾）
    * toLowerCase：转为小写
    * toUpperCase：转为大写
5. 与正则表达式相关的方法
    * split：将字符串按照传入的正则表达式对字符串进行分割转化为数组（不管设不设置g，都是全局匹配模式）
        * 注：如果传入的是**空字符串**，就把字符串中每一个字符都作为数组中的一个元素
    * search：传入一个正则表达式，返回第一次匹配正则表达式的起始索引，如果不存在，就返回-1（不管设不设置g，都是非全局匹配模式）
    * match：传入一个正则表达式，如果没有设置g，就返回匹配表达式的第一个子字符串，如果设置g，就返回所有匹配项
        * 该方法返回的是数组（只有一个匹配的时候也是数组）
    * replace：
        * 第一个参数：正则表达式
        * 第二个参数：替换后的子字符串
        * 开启全局匹配，就替换所有，如果没有开启，默认替换第一次出现的
#### 正则表达式对象RegExp
1. 创建对象
    * RegExp是一个构造函数，第一个参数为正则表达式，第二个参数为匹配模式
    
    * 匹配模式有两种：
        * i    ：忽略大小写
        * g    ：全局匹配（从头匹配至尾）
        
    * 使用字面量创建
    
        `/正则表达式/匹配模式`
    
        ```js
        var reg = /[a-z]/i;
        ```
2. 方法
   
    * test：检查传入字符串中是否匹配正则表达式对象的正则表达式
3. 正则表达式语法
    * a|b   a或b
    * [az]    a或z
    * [a-z]    任意小写字母都可以
    * [A-z]    任意字母都可以
    * [0-9]    任意数字都可以
    * [^ab]    除了ab都可以（即不能是只有ab、a、b），如果有ab还有其他的也可以，如abc
    * a{n}        a字符串出现n次
    * a{m,n}    a字符串出现m-n次
    * a{m,}    a字符串出现大于m次
    * ^a        以a开头
    * a$        以a结尾
    * `.`        匹配任意字符，除了换行符和分隔符
    * \w        字母、数组、下划线
    * \d        数字
    * \s        空白字符
    * \b        单词边界
    * 其余见w3c教程

## DOM（Document Object Model）
    文档对象模型
    JS通过DOM对HTML页面进行操作
### 概念解析
- 文档：即整个HTML网页文档
- 对象：网页中的每一个部分（元素，属性，文本）都是一个对象
- 模型：使用模型来表示对象之间的关系（文档对象树）
- 节点Node：构成网页的最基本的组成部分，网页中的每一个部分都是一个节点（html标签，属性，文本，注释，整个文档）
    * 虽然都是节点，但是所属的类型是不同的
        * 标签：元素节点
        * 属性：属性节点
        * 文本：文本节点
        * 文档：文档节点
- 文档对象（document）是window对象的一个属性
- 事件：用户与浏览器的交互行为，比如点击按钮、关闭窗口，鼠标移动
    * 有两种方式来设置事件
        * 一种是直接在元素的内部添加属性，并赋予js代码（但是这样子js和html耦合程度过高）
        * 另一种是在script标签中，获取到标签的节点对象，为其绑定对应的事件的响应函数
### JS事件加载
* 问题：由于html渲染并加载的时候是逐行进行的，所以如果将事件的绑定函数写在了head标签中。此时，body标签内的元素尚未加载，获取到的DOM对象都是null，函数绑定失败
* 解决办法
    * 将js代码统一放至页面底部（优点：性能好）
    * 将js事件绑定函数代码写进页面加载的绑定事件函数中
        * 由于页面的加载是一个事件，所以也可以通过绑定函数来进行一些操作，可以将其余的js事件绑定函数代码写入其中，会等到页面加载完成后执行，确保执行时DOM对象已经加载完毕，该事件是window.onLoad（优点：便于管理）
### DOM查询
* 获取元素节点（通过document对象获取）
    * getElementById：通过id属性获取一个元素节点对象
    * getElementsByTagName：通过标签名称获取一组元素节点对象，返回的类型是一个数组
    * getElementsByName：通过name属性获取一组元素节点对象，返回的类型是一个数组
    * 注：元素的class属性不能通过class获取，而是className
* 获取元素节点的子节点（通过具体的元素节点对象获取）
    * 方法getElementsByTagName：通过标签名称返回当前元素节点对象下的所有子元素节点对象
    * 属性childNodes：返回当前元素节点对象的所有子节点对象（包括文本节点，即标签与标签之间的空白）
    * 属性firstChild：返回当前元素节点对象的第一个子节点对象，可以是文本节点
    * 属性lastChild：返回当前元素节点对象的最后一个子节点对象，可以是文本节点
    * 属性children：返回当前元素对象的所有元素节点对象
    * 属性firstElementChild：返回当前元素对象的第一个子元素对象（ie8及以下不兼容，pc端不推荐使用）
* 获取元素节点的父节点和兄弟节点
    * parentNode：返回当前节点的父节点
    * previousSibling：返回当前节点的前一个兄弟节点（可以是Text节点）
    * previousElementSibling
    * nextSibling：返回当前节点的下一个兄弟节点
* 其余的DOM查询
    * document.body：document对象直接持有了一个对于body元素的引用
    * document.documentElement：document对象直接持有了一个对于html标签的引用
    * document.all：document对象直接持有了一个对于页面的所有标签的引用的数组（但是有一个bug是直接打印document.all是undifined），另一种方式可以使用getElementsByTagName("*")
    * getElementsByClassName：通过class属性返回一组元素节点对象（ie8及以下不支持，所以不推荐使用）
    * querySelector：可以通过css的类选择器直接返回元素节点对象，但是如果有多个，只会返回第一个（ie8支持）
    * querySelectorAll：可以通过css的类选择器直接返回所有匹配的元素节点对象的数组（结果只能是数组）
* 注：事件的响应函数中的this指向的是添加事件的组件



### DOM的增删改
* appendChild：添加一个子节点
* removeChild：删除一个子节点
* replaceChild(新节点，旧节点)：替换一个子节点
* createElement：创建一个元素节点对象
* createTextNode：创建一个文本节点对象
* insertBefore(添加的节点，插入的子节点)：在插入的子节点的前面添加一个节点
### DOM修改css
* 设置或读取元素的内联样式：元素.style.样式名 = 样式值
    * 如果样式名中包含了“-”符号，需要将该样式名修改为驼峰命名法
    * 通过style属性设置的都是内联样式
    * 如果读取的时候没有设置该属性的内联样式，则读取失败
    * 如果样式表中在样式值后面添加了***!important***，此时该样式有最高的优先级，JS代码无法对其修改
* 读取元素的当前样式
    * currentStyle：元素.currentStyle.样式名(只有ie支持)
    
    * window.getComputedStyle(元素节点对象，伪元素)：返回指定元素节点对象的当前样式对象，伪元素一般传递null（ie8及以下不支持）
    
    * 注：
        * 以上两种方法获取的样式都是只读的，如果修改，只能选择通过style修改
        * 如果没有设置样式，currentStyle返回默认值，getComputedStyle方法返回真实值
        
        ```js
        function getStyle(obj, style) {
                if (window.getComputedStyle) {
                  return getComputedStyle(obj, null)[style];
                } else {
                  return obj.currentStyle[style];
                }
        }
        ```
* 其他读取样式的途径
    以下这些方法都是只读的，并且直接返回数值
    * clientWidth和clientHeight：读取元素的可见高度和宽度（包括内容区和内边距）
    * offsetWidth和offsetHeight：读取元素的全部高度和宽度（包括内容区、内边距和边框）
    * offsetParent：获取元素最近的开启定位的祖先元素，如果都没有开启定位，则返回body
    * offsetLeft和offsetTop：获取元素相对于其定位父元素的水平（竖直）偏移量
    * scrollWidth和scrollHeight：获取整个滚动区域的宽度和高度
    * scrollLeft和scrollTop：获取水平（竖直）滚动条滚动的距离
    * 判断滚动条是否滚动到底
        * scrollWidth - scrollLeft ==clientWidth
        * scrollHeight - scrollTop == clientHeight
### DOM事件对象
    当事件的响应函数被触发时，浏览器会将一个事件对象作为实参传递进响应函数
    事件对象中封装了当前事件的一切相关信息，如鼠标的坐标，键盘的按键，鼠标滚轮的方向
    IE8没有将事件对象传入响应函数，而是将事件对象作为window的属性
    event = event || window.event;
* 鼠标坐标：
    * clientX    鼠标在可见窗口横坐标
    * clientY    鼠标在可见窗口纵坐标
    * pageX    鼠标在整个页面的横坐标（IE8不支持）
    * pageY    鼠标在整个页面的纵坐标（IE8不支持）
* 获取滚动条的滚动距离
    * 对于chrome浏览器，使用body.scrollLeft(水平滚动条),body.scrollTop(竖直滚动条)
    * 对于其他浏览器，使用documentElement.scrollLeft(水平滚动条),document.documentElement.scrollTop(竖直滚动条)
* 事件的冒泡
    * 所谓冒泡指的是事件的向上传导，当子元素上的事件被触发时，其父元素的的相同事件也会被触发
    * 大部分的冒泡都是有用的
    * 如果想要关闭冒泡，可以使用事件对象中的cancelBubble属性，将其设置为true
    * 应用：事件的委派：指对于一组元素具有相同的事件响应函数，可以直接将事件响应函数绑定给这些元素的父元素，所以当子元素的单击响应函数触发时，会冒泡到父元素的单击响应函数
        * 优点：事件委派利用冒泡减少了事件绑定的次数，提高了性能
        * 缺点：在父元素的区域仍可触发事件的响应函数
            * 解决方法：使用event.target，该属性的值为此次事件的触发元素节点对象，可以通过是否为目标元素节点，来让父元素的函数执行
* 事件的绑定
    * 将回调函数直接赋值给某个元素节点的事件，此时该事件只能绑定一个响应函数
    
    * 绑定多个响应函数
        * 对于非ie浏览器，可以使用addEventListener()方法
            * 第一个参数：事件名称，不要添加on（类型：字符串）
            * 第二个参数：事件触发时的回调函数
            * 第三个参数：事件是否在捕获阶段执行，一般设置为false
            * 方法中的this对象是被绑定事件的对象
        * 对于ie浏览器，可以使用attachEvent()方法
            * 第一个参数：事件名称，添加on
            * 第二个参数：事件触发时的回调函数
            * 方法中的this对象是window对象
        
        ```js
        function bind(obj, eventStr, func) {
                if (obj.addEventListener) {
                  obj.addEventListener(eventStr, func, false);
                } else {
                  obj.attachEvent("on" + eventStr, function() {
                    func.call(obj);
                  });
                }
        }
        ```
* 事件的传播
    * w3c将事件的传播分成了三个阶段
        * 捕获阶段：在捕获阶段时，从最外层的祖先元素向内进行事件的捕获，但是默认此时不会触发事件
        * 目标阶段：事件已经捕获到目标元素，在目标元素上开始执行事件
        * 冒泡阶段：事件从目标元素向它的祖先元素传递，并依次执行元素上的事件
    * 如果希望在捕获阶段就执行某一事件，可以将addEventListener的第三个参数设置为true，其余事件仍按照原来的顺序
    * 注意：ie8及以下的浏览器没有捕获阶段
#### 拖拽元素
    * 拖拽的流程
        * onmousedown：鼠标在拖拽元素上被按下
        * onmousemove：鼠标进行移动，拖拽元素与鼠标一起移动
        * onmouseup：鼠标松开，拖拽元素固定在当前位置
    * 拖拽的注意事项：当拖拽网页中的内容时，浏览器会默认去搜索引擎中搜索内容，此时会导致拖拽功能的异常，这个是浏览器的默认行为，如果不想要这个默认行为，可以在事件响应函数中的最后添加一个  **return false**（但对于使用**addEventListener添加事件响应函数的方式不起作用**）
        * 注：此方式在ie8及以下不支持
        * 解决办法：
            * obj.setCapture()：对鼠标按下事件进行捕获，可以将页面上所有的鼠标按下事件改变成当前对象obj的对应事件触发
            * releaseCapture：对鼠标按下事件的捕获进行释放
```js
function drag(obj) {
        obj.onmousedown = function(event) {
          obj.setCapture && obj.setCapture();
          // 获取鼠标距离box元素上和左边界的距离，保证每次拖拽鼠标相对于box的坐标不变
          var left = event.clientX - obj.offsetLeft;
          var top = event.clientY - obj.offsetTop;

          document.onmousemove = function(event) {
            obj.style.left = (event.clientX - left) + "px";
            obj.style.top = (event.clientY - top) + "px";
          };
          document.onmouseup = function() {
            document.onmousemove = null;
            document.onmouseup = null;
            obj.releaseCapture && obj.releaseCapture();
          };
          // 取消默认行为
          return false;
}
```

#### 鼠标滚轮事件

    * onmousewheel事件，在绑定事件的对象上滚动滚轮时触发
    * 但对于火狐浏览器，不支持onmousewheel事件，需要使用DomMouseScroll，并且该属性只能通过addEventListener
    * 判断鼠标滚轮滚动的方向
        * 使用event的deltaY，向上滚是-125，向下滚是125
        * 火狐使用event的detail属性，向上滚是-3，向下滚是3
    * 需要注意的是，滚轮事件在浏览器中默认的行为会滚动浏览器的滚动条
        * 可以使用return false取消默认行为
        * 但是对于火狐中使用addEventListener添加的事件来说，使用event.preventDefault();
#### 键盘事件
    * onkeydown：键盘被按下
        * event的keyCode属性可以获取键盘按下的键的unicode编码值(被弃用)
        * event.key：获取按键按下的键值（按下a就是a）
        * altKey：alt键是否被按下
        * shiftKey：shift键是否被按下
        * ctrlKey：ctrl键是否被按下
        * 在文本框中，如果对于onkeydown事件的默认行为是将按键显示在文本框中，如果要取消默认行为使用return false
    * onkeyup：键盘被松开

## BOM（浏览器对象模型）
    在BOM中提供了一组对象，用来完成对于浏览器的操作
* BOM对象
    * window：整个浏览器的窗口，也是网页中的全局对象
    * navigator：当前浏览器的信息，通过该对象可以识别不同的浏览器
    * location：当前浏览器的地址栏信息，通过该对象可以获取地址栏信息，或者跳转到其他yemian
    * history：浏览器的历史记录
        * 但由于隐私的原因，该对象不能访问到具体的历史记录，只能操作浏览器向前或向后翻页
        * 该操作只在当次访问内有效
    * screen：用户的屏幕信息，通过该对象可以获取用户显示屏的相关信息
    * 这些对象都是window对象的属性
### navigator
       由于历史原因，Navigator对象中的大部分属性已经不能帮助识别浏览器了（在这里鄙视一下不要脸的微软！！！）
* 一般我们仍会使用userAgent属性去判断浏览器
    * 火狐：/firefox/i.test(navigator.userAgent)
    * chrome：/chrome/i.test(navigator.userAgent)
    * ie8-ie10:/msie/i.test(navigator.userAgent)
    * ie11(edge):由于ie11将navigator中的大部分信息都改变成与其他浏览器类似的信息，就连window.ActiverXObject这个ie独有的对象，返回值也改成了false，所以想要判断，可以使用**"ActiveXObject" in window**
    * 注：现在edge浏览器里window对象没有ActiveXObject属性
```js
if (/edg/i.test(navigator.userAgent)) {
      alert("你是Edge");
    } else if ("ActionXObject" in window) {
      alert("你是IE");
    } else if (/chrome/i.test(navigator.userAgent)) {
      alert("你是chrome");
    } else if (/firefox/i.test(navigator.userAgent)) {
      alert("你是firefox");
    }
}
```



### history
* 属性
    * length：当前访问中已经访问过的url的数量
* 方法：
    * back：回退到上一个页面
    * forward：前进到上一个页面
    * go：跳转到指定页面，传入一个参数n。如果n大于0，前进到第n个页面，n为1时效果与back相同；如果n小于0，后退到第n个页面，n为-1时，效果与forward相同



### location
* 直接打印location对象，其toString方法会返回对象的href属性
* 可以对location对象进行修改，使用完整的url路径或是项目中的相对路径，页面会跳转到目标页面
* 属性：封装了一系列与url相关的属性，比如host，port，protocal
* 方法
    * assign：作用与直接修改location对象相同
    * reload(false)：重新加载页面，如果传入的值是true，会强制清空缓存加载
    * replace：用指定页面替换当前页面，作用与直接修改页面效果类似，但是**不会生成历史记录**



### 定时器
    通过**window（this）**对象的setInterval和clearInterval方法来实现的
* setInterval：开启定时调用
    * 第一个参数：回调函数，每隔一个时间会被调用一次
    * 第二个参数：回调函数调用的时间间隔，以毫秒作为单位
    * 返回值：返回一个Number，作为当前定时器的唯一标识
    * 注意：如果是通过事件触发创建的定时器，需要**控制定时器的个数的数量**，不然触发一次就会生成一个
    * 在开启定时器之前，需要将当前元素的其他定时器关闭
* clearInterval：清除定时器
    * 参数：传入需要被清除的定时器的唯一标识
### 延时器
    通过window对象的setTimeout和clearTimeout方法来实现
* setTimeout：开启延时调用
    * 第一个参数：回调函数，在一定的时间只会被调用
    * 第二个参数：回调函数的执行之前的时间，以毫秒为单位
    * 返回值：返回一个数字，作为当前延时器的唯一标识
* clearTimeout：清除延时器
    * 参数：传入需要被清除的延时器的唯一标识
### 类的操作
    由于使用js直接修改对象的style属性中的样式，每修改一次，就会导致页面被重新渲染一次，性能不好
    并且该方式使得表现与行为耦合程度过高
* 对于修改样式，可以使用class属性，先在style标签内设置好class的样式，通过js代码直接修改    对象.className
* 如果是在某个class样式的基础上，需要再覆盖一个样式 :    对象.className += " class属性值"
    * 注意：此处新添加的class属性值前需要有**空格**
```js
// 判断一个对象obj的class属性是否含有className
    function hasClass(obj, className) {
      var regExp = new RegExp("\\b" + className + "\\b");
      return regExp.test(obj.className);
    }
    // 如果一个对象obj没有className属性，则添加
    function addClass(obj, className) {
      if (!hasClass(obj, className)) {
        obj.className += " " + className;
      }
    }
    // 删除obj的所有className属性
    function removeClass(obj, className) {
      var regExp = new RegExp("\\b" + className + "\\b");
      obj.className = obj.className.replace(regExp, "");
    }
    // 如果obj有className属性，则删除，如果没有就添加
    function toggleClass(obj, className) {
      if (hasClass(obj, className)) {
        removeClass(obj, className);
      } else {
        addClass(obj, className);
      }
    }
```



## JSON
    Javascript Object Notation（JS对象表示法）
* 介绍
    * JSON是一种特殊格式的字符串，可以被任意的语言所识别，并且可以转换为任意语言的中的对象
    * JSON在开发中主要用来数据的交互
* 格式
    * JSON和JS对象的格式一样，只不过JSON字符串中的**属性名必须加双引号**
    * JSON分类
        * 对象
        * 数组
    * JSON键值对中value允许的值
        1. 字符串
        2. 数字
        3. 字符串
        4. null
        5. 对象
        6. 数组
* 数据转化
    * 在JS中，提供了一个对象JSON，可以用来将JSON字符串与对象或数组进行转化（IE7及以下不兼容）
    * 字符串-->对象
        * JSON.parse(字符串)：将JSON字符串转为对象返回
    * 对象-->字符串
        * JSON.stringify(对象/数组)：将对象或数组转化成JSON字符串返回
* 另外的转换方法-----------使用eval函数
    * eval(str)函数用一个字符串作为参数传入，并将其作为js代码块进行执行，返回执行结果
    * 注：如果eval函数传入的字符串中有{}，会将其中的内容作为代码块执行。如果不想作为代码块进行解析，需要在字符串前后添加一对（）
    * 对于eval还有些注意事项
        * 由于该函数功能过于强大，具有一定的安全隐患
        * 性能较差
* 对于后端处理json数据可以使用jackson来处理
    * 导入依赖
        * 导入jackson-dayabind
    * 具体使用步骤
        * 创建ObjectMappper对象
        * 调用ObjectMapper对象的writeValueAsString方法将传入的***对象***或***集合***转化成json字符串
    * 注意事项
        * jackson根据对象的**getter**方法来定位属性，而不是字段（转换之后的json字符串的属性名为getter方法名）
        * 如果想要某个属性在转换时不写入json字符串，可以添加***@JsonIgnore***注解在该属性的getter方法忽略该属性


## JS高级
### 类型
* undifined和null的区别
    * undifined是定义未赋值
    * null是定义且赋值为null
* 什么时候赋值为null
    * 初始赋值，表明将要赋值为对象
    * 结束前，让对象成为垃圾对象
* 变量类型和数据类型
    * 数据的类型
        * 基本类型
        * 对象类型
    * 变量的类型（在JS中是变量在内存中的值的类型）
        * 基本类型：直接存储值
        * 引用类型：存储的是对象的内存地址
### IIFE
    Immediately-Invoked Function Expression
* 理解：通过匿名函数自调用的形式，只调用一次，即在声明的时候进行调用
* 作用：
    * 隐藏实现
    * 不会影响外部的命名空间
    * 可以用来编写js的模块
### prototype原型对象
* prototype（显式原型）是函数的属性，默认指向一个空对象（Object例外）（只有constructor和__proto__（此隐式原型指向的是Object函数的原型对象）），在函数定义时创建
* __proto__（隐式原型）是函数实例的属性，默认值为函数的prototype，在创建对象的实例时创建
* prototype属性的constructor指向一个对象，函数对象
* 原型链（！！！important）
    * 对于一个函数来说，默认拥有prototype属性，指向一个Object对象，该对象是隐式通过Object函数创建的，所以该对象（函数原型指向的对象）的__proto__属性指向了Object函数的原型对象
    * Object函数的原型对象是null
    * 函数的实例对象默认拥有__proto__属性，指向函数的prototype指向的对象
    * Function是通过Function构造函数创建的对象（是其自身的实例，所有函数都是Function的实例），所以对于Function来说，其__proto__和prototype的值相同
    * 函数都是通过Function构造函数在堆内存中创建的对象（包括Object），其__proto__属性的值和Function.prototype相同
    * 对于实例的属性或方法的查找，通过隐式原型链去查找，即从对象实例的__proto__的属性开始查找原型对象，找到即返回，找不到返回undefined
* 原型继承
    * 将子函数的原型指向**父函数的实例对象**，并且修改原型对象的constructor属性指向子函数本身
* instanceof运算符
    * 表达式：A instanceof B
    * 当B函数的显式原型对象在实例A的隐式原型链上，返回true
### 执行上下文
* 代码分类
    * 全局代码
    * 函数（局部）代码
* 全局执行上下文
    * 在执行全局代码前将window确定为全局执行上下文
    * 对全局数据进行预处理
        * 通过var定义的全局变量赋值为undefined，并添加到window的属性中
        * function声明的全局函数，创建对象，并添加为window的方法
        * this赋值为window
    * 开始执行全局代码
* 函数执行上下文
    * **在调用函数时**，准备执行函数体之前，创建对应的函数执行上下文对象（虚拟的，存在于栈中）
    * 对局部数据进行预处理
        * 形参变量，赋值为实参，并添加为执行上下文的属性
        * arguments对象，赋值为实参列表，添加为执行上下文的属性
        * var定义的局部变量定义为undefined，添加为执行上下文的属性（变量提升）
        * function声明的函数，创建函数对象，添加为执行上下文的属性（函数提升）
        * 注：**变量提升先于函数提升**
        * this，赋值为调用函数的对象
    * 开始执行函数体代码
* 执行上下文栈
    * 在全局代码执行之前，JS引擎会创建一个栈来管理所有的执行上下文对象
    * 在全局执行上下文（window）确定后，将其添加到栈中
    * 在函数执行上下文创建后，将其添加到栈中
    * 在当前函数执行完后，将栈顶的执行上下文移除
    * 总结：
        * 栈顶的执行上下文是正在运行的上下文
        * 栈底的执行上下文是全局执行上下文
* 作用域
    * 全局作用域
    * 函数作用域
    * 作用域在代码确定之后就已经确定了
    * 变量的查找沿着**作用域链**
### 闭包
* 如何产生闭包
    * 当一个嵌套的内部（子）函数引用了嵌套的外部（父）函数的变量（函数）时，就产生了闭包
* 闭包是什么
    * 理解一：闭包是嵌套的内部函数
    * 理解二：包含被引用变量（函数）的对象
    * 注意：闭包存在于嵌套的内部函数中
* 产生闭包的条件
    * 函数嵌套
    * 内部函数引用了外部函数的数据（变量/函数）
    * 外部函数执行
* 闭包的作用
    * 使用函数内部的变量在函数执行完后，仍然存活在内存中（延长了局部变量的生命周期）
    * 让函数外部可以操作（读/写）函数内部的数据（变量/函数）
* 闭包的生命周期
    * 产生：在嵌套内部函数定义执行完时就产生了（即外部函数执行）
    * 死亡：在嵌套的内部函数成为垃圾对象
* 闭包的应用：定义JS模块
    * 具有特定功能的JS文件
    * 将所有数据和功能都封装在一个函数的内部（私有的）
    * 只向外暴露一个包含n个方法对象或函数
    * 模块的使用者，只需要通过模块暴露的对象调用方法来实现对应的功能
* 闭包的缺点
    * 函数执行完后，函数的局部变量没有释放占用内存的时间会变长
    * 容易造成内存泄露
        * 内存泄露的几个原因：1）意外的全局变量    2）闭包    3）没有及时清理的回调函数和定时器
    * 解决
        * 及时释放
        * 能不用就不用闭包
### 浏览器内核组成
* 主线程
    * js引擎模块：负责js的编译和运行
    * html，css文档解析模块：负责页面文本的解析
    * DOM/CSS模块：负责DOM/CSS在内存中的相关处理
    * 布局和渲染模块：负责页面的布局和效果的绘制（内存中的对象）
* 分线程
    * 定时器模块：负责定时器的管理
    * 事件响应模块：负责事件的管理
    * 网络请求模块：负责Ajax请求
### JS是单线程的
    这里说的单线程是JS引擎模块是单线程，用来操作界面，h5之后是多线程，只是这里的多线程包括了操作界面之外的其他线程
    JS的单线程与它的用途有关，作为浏览器脚本语言，js的主要用途是与用户互动，以及操作DOM，多线程会带来复杂的同步问题
* 代码的分类
    * 初始化代码
    * 回调代码：处理回调逻辑
* JS引擎执行代码的基本流程
    * 先执行初始化代码，：包含一些特别的代码        回调函数（异步执行）
        * 设置定时器
        * 绑定事件监听
        * 发送ajax请求
    * 后面的某个时刻会执行回调代码
* 事件循环模型
    * 事件管理模块
        * 定时器管理模块
        * 事件管理模块
        * ajax请求管理模块
    * 回调队列（回调逻辑触发时，会将要执行的回调函数放入callback queue中，等待执行）
        * 又叫消息队列，事件队列，英文名称为callback queue
    * 模块运转流程
        * 执行初始化代码，将事件回调函数交给对应模块管理
        * 当事件发生时，管理模块会将回调函数及其数据添加到回调队列中
        * 只有当初始化代码执行完（需要一定的时间），才会遍历读取回调队列中的回调函数执行
### Web Workers
#### 介绍
* Web Workers是HTML5提供的一个javascript解决方案
* 我们可以将一些大计算量的代码交由web workers运行而**不冻结界面**（原来没有该技术之前，如果界面中的主线程在进行计算或处于运行时，无法修改界面）
* **子线程完全受主线程控制，且不能操作dom**，所以并没有改变js是单线程运行的本质
* 主线程可以向分线程发送多条消息，分线程采用消息队列来存储，并依次执行
#### 使用
1. 创建在分线程执行的js文件
2. 在主线程中的js中发消息并设置回调函数
* 主线程的步骤
    * 创建一个Worker对象
        ```
        var worker = new Worker("分线程的js文件的路径");
        ```
    * 设置worker接收到消息后的响应函数
        ```
       worker.onmessage = function(event) {
          // event.data是分线程执行完之后的响应数据 
       }
       ```
    * 向分线程发送消息
        ```
        worker.postMessage("请求数据");
        ```
* 分线程的步骤
    * 设置onmessage函数
    ```
        var onmessage = function(event) {
            // event.data是主线程发送的数据
            // 执行分线程逻辑（调用分线程函数等等）
            postMessage("响应数据"); // 向主线程发送响应数据
        }
    ```
#### 不足
* 使用web worker的分线程js文件中的全局对象**不是window**，而是DedicatedWorkerGlobalScope（这也是分线程不能更新界面的原因）
* 缺点
    * 慢，因为多线程运行
    * 不能跨域名加载js
    * worker代码不能访问DOM（更新UI）
    * 不是所有浏览器都支持此特性（**这也是最重要的一个原因）