> 视频位置：https://www.bilibili.com/video/BV1LV411z72g?from=search&seid=10938795996533337535
>
> 代码位置：F:\projects\thstudy\1__FrontEnd

## 1、HTML

### 1.1 标签

#### 1.1.1 meta

除了常用的charset之外，还可以设置name属性和content属性

* name: 要设置的属性的名字

* content: 要设置属性的内容

```html
<!-- description用于告诉搜索引擎网页的主要内容-->
<meta name="description" content="这是一个个人博客">

<!-- keywords用于让搜索引擎通过一些关键词找到该网页-->
<meta name="keywords" content="blog,personal,个人,博客">
```

* http-equiv

```html
<!--可以设置网页的重定向-->
<meta http-equiv="refresh" content="3;url=https://www.baidu.com"
```

#### 1.1.2 html5网页结构标签

html5新增的**块级元素**，也是**语义化标签**（只有语义上的作用）

* header
  * 表示网页的头部，主要放置一些logo、导航、搜索框等内容
* main
  * 网页的主体，不能超过一个
* footer
  * 表示网页的底部，主要放置一些版权信息、友情链接
* nav
  * 导航栏
* article
  * 文章，表示网页中的一块内容
* aside
  * 侧边栏
* hgroup
  * 表示标题组，一般用来放置多个h标题
* section
  * 表示一块区域

html5新增的语义化标签，并且是行内元素（与块级元素对应），也叫内联元素

* em
  * 语义上表示强调，浏览器默认斜体
* strong
  * 语义上表示强调，浏览器默认加粗
* q
  * 语义上表示引用，quote，浏览器默认会加引号
* blockquote
  * 语义上表示长引用，是**块级元素**
  * 注意：不能和p标签一起用，因为p标签不能放块元素

#### 1.1.3 图片的格式

* jpeg(jpg)
  * 支持的颜色比较丰富，不支持透明效果
  * 适合用来显示照片
* gif
  * 支持颜色比较少，支持简单透明，支持动图
  * 颜色单一的图片（logo）
* png
  * 支持的颜色丰富，支持复杂透明

选择格式的方法

1） 效果一致，用小的图片

2） 效果不一致，用效果好的

* webp
  * google专门为网页设计的一种图片格式
  * 支持的颜色丰富，支持复杂透明，支持动图，并且占用的存储空间小
  * 但是兼容性差
* base64编码的图片（特殊情况下使用）
  * base64可以用来将图片转换为字符串，直接在页面中使用（写在img的src属性中）
  * 如果对网页加载速度有限制，可以使用base64

#### 1.1.4 iframe引入外部网页

内联框架iframe，用来引入一个外部的网页

一般在开发中**不推荐使用**，iframe的内容不会被搜索引擎抓取

```html
<iframe src="https://www.baidu.com" frameborder="0" width="800" height="640"></iframe>
```



#### 1.1.5 音频audio，视频video

都属于**替换元素**

* audio

  * 默认情况下音频不会在页面中显示和播放
  * controls：显示音频播放进度
  * autoplay：设置自动播放
  * loop：设置循环播放

  ```html
  <!--设置audio的第一种方式-->
  <audio src="audio/song.mp3" controls loop></audio>
  
  <!--设置audio的第二种方式-->
  <!--如果支持mp3则播放，如果没有，则播放ogg，如果ogg也不支持，显示下面的文字-->
  <audio controls loop>
  	<source src="audio/song.mp3">
      <source src="audio/song.ogg">
      对不起，您的浏览器不支持音频播放
  </audio>
  
  <!--设置audio的第三种方式-->
  <!--浏览器不支持audio标签时-->
  <audio controls loop>
  	<source src="audio/song.mp3">
      <source src="audio/song.ogg">
      <embed src="audio/song.mp3" type="audio/mp3">
  </audio>
  ```

* video

  * controls
  * allowfullscreen: 设置允许全屏

  ```html
  <!--第一种方式-->
  <video src="video/movie.mp4" controls loop width="600"></video>
  
  <!--第二种方式-->
  <video controls loop width="600">
  	<source src="video/movie.mp4">
      <source src="video/movie.mp4">
      <embed width="600" height="480" src="src/video.mp4" type="video/mp4">
  </video>
  ```

#### 1.1.6 超链接a标签的锚点定位

* 回到顶部

  ```html
  <a href="#">回到顶部</a>
  ```

* 去指定的标签

  ```html
  <a href="#bottom">去底部</a>
  。。。
  ...
  ...
  <span id="bottom">底部</span>
  ```

#### 1.1.7 对于input标签

* 如果设置了required属性，则必须填写该属性才可以提交

* 如果radio设置了相同的name属性，则它们只有其中一个radio可以被选中

* 如果label设置了for属性，其值为radio元素的id，则点击该label时，radio会被选中

  ```html
  <label for="indoor"> 
    <input id="indoor" type="radio" name="indoor-outdoor">Indoor 
  </label>
  ```

* 属于同一类的复选框checkbox需要有相同的name属性，并且label和checkbox的关系设置与radio一致

* 选中的radio和checkbox传递到后台的值是其value属性，通过name-value的键值对形式；如果省略，value是默认值on

* radio和checkbox设置默认选中通过checked属性

#### 1.1.8 表格的html5语义化标签

* thead：表头
* tbody：表格的主体
* tfoot：表格的底部
* 注意：创建表格时，如果没有使用tbody，浏览器会自动添加tbody，并且将所有的tr都添加到tbody
* 所以，tr的父元素并不是table而是tbody
* 如果一个div的父元素添加了vertical-align之后需要对div再添加`display: table-cell`才会垂直居中



## 2、CSS

Cascade Style Sheet层叠样式表

用来处理网页的表现

### 2.1 编写css的方式

1. 直接写在标签的style属性内部

```txt
将样式直接编写到style属性中，这样会导致结构和表现耦合，不方便代码的复用，并且不方便后期的维护，所以在开发中永远不要使用内联样式
```

```html
<h4 style="color:chocolate">这是通过style属性直接设置的</h4>
```

2. 将样式编写在style标签中（在head标签内）

```txt
将样式编写到style标签中，然后通过css选择器选中指定的元素并为其设置样式。
将HTML和CSS代码分离开，使CSS样式可以重复使用，并方便后期的维护
```

```html
<style>
    p {
      color:blue;
    }
  </style>
```

3. 将CSS编写到外部的CSS文件中，然后通过link标签进行引入

```txt
将CSS写在外部的样式表中，使结构与表现完全分离，可以在不同的页面对样式表进行复用，方便后期的维护

将样式表写在外部文件中，可以充分利用到浏览器的缓存机制，进而加快页面的加载速度，提高用户体验
```

```html
<link rel="stylesheet" href="../../css/lesson/01_css_编写css方式.css">
```



#### 2.1.1 注释

```css
/*
 CSS 注释
*/
```

### 2.2 语法

形式：`选择器` + `声明块`

* 选择器
  * 通过选择器可以选中页面的指定元素
* 声明块
  * 整体是一个大括号
  * 声明块由一个一个声明组成
  * 声明是一个键值对结构
  * 声明块中声明的所有样式将会应用到前边的选择器对应的元素

```css
p {
    color: red;
    font-size: 50px;
}
```

### 2.3 选择器

#### 2.3.1 元素选择器

作用：根据标签名选中指定的元素

语法： `tag {}`

例子： div {}  span {}

#### 2.3.2 id选择器

作用：根据元素的id值选中指定元素

语法：`#id {}`

例子： #p1 {}

#### 2.3.3 选择器分组（并集选择器）

作用：使用相同的声明块作用于多个选择器

例子：#p1, #p2, div {}

#### 2.3.4 类选择器

作用：根据元素的class属性选中元素

语法：`.class {}`

注意：一个标签的class值可以有多个，如`<p class="p1 p2 p3"></p`

#### 2.3.5 通配选择器

作用：选中页面中的所有元素

语法：`* {}`

#### 2.3.6 交集选择器

作用：选中同时符合多个选择器的元素

语法：`选择器1选择器2..选择器N {}`

例子： `div.box {}` 选中div中class为box的元素 

#### 2.3.7 后代选择器

作用：选中指定元素的指定后代

语法： `祖先 后代 {}` 

例子：`div span {}`  选中div下的span元素  

#### 2.3.8 子元素选择器

作用：选中指定父元素的指定子元素

语法：`父元素 > 子元素`

#### 2.3.9 兄弟元素选择器（+相邻）

作用：选中指定元素的紧随其后的兄弟元素

语法：`兄弟元素1 + 兄弟元素2 {}` 选中兄弟元素1的紧随其后的一个兄弟元素2

#### 2.3.10 兄弟元素选择器（~所有）

作用：选中指定元素的之后的所有指定兄弟元素

语法：`兄弟元素1 ~ 兄弟元素2 {}`   选中兄弟元素1的之后的兄弟元素2

#### 2.3.11 属性选择器

作用：选中指定属性(有指定值)的元素

语法：

1. `[attr='value']`

	2. [attr]
 	3. [attr^='value']  属性atter以value开头的元素
 	4. [attr&='value']  属性attr以value结尾的元素

5. [attr*=‘value']  属性attr含有value的元素*
6. *[attr*='value' i]  属性attr含有value（忽略大小写）的元素

#### 2.3.12 伪类选择器

冒号开头，表示特殊的状态或位置

* :first-child
  * 第一个子元素（限定子元素的）
  * 例子：`div p:first-child {}`  选中div的第一个子元素并且该子元素必须是p标签
* :first-of-type
  * 同类型的第一个子元素
  * 例子：`div p:fisrst-of-type {}`  选中div下第一个p元素
* :last-child
* last-of-type
* :nth-child(num)
  * 第num个child
  * num=odd       序号为奇数的元素
  * num=even      序号为偶数的元素
* :nth-of-type(num)

* :only-child
  * 当前子元素是父元素的唯一子元素才可被选中
* :only-of-type
* :empty
  * 匹配标签内为空的元素
* :not()
  * 否定伪类
  * 例子：`p:not(.p1)` 选中p元素，但是p的class的值不能是p1

#### 2.3.12 和访问相关的伪类选择器

* :link
  * 正常的没有访问过的超链接
  * 例子： `a:link {}`
* :visited
  * 超链接访问过的状态
  * 注意：由于隐私的原因，visited伪类只能够设置字体的颜色，不能设置其他样式
* :hover
  * 鼠标悬浮的状态
  * 不仅仅适用于超链接
* :active
  * 鼠标点击的状态
  * 不仅仅适用于超链接

#### 2.3.13 伪元素

主要表示一些特殊的元素

* ::before

  * 开始标签和文本内容之间的位置

  * ```css
    div::before {
    	/*在该位置添加hello文字*/
        content:"hello"
    }
    ```

* :: after

  * 文本内容和结束标签之间的位置

* ::first-letter

  * 第一个字母或汉字（首字母）

  * ```css
    p::first-letter {
        font-size:50px;
    }
    ```

* ::first-line

  * 第一行

* ::selection

  * 选中的文字
  * 只能设置color和background-color
  * 设置font有关的属性无效

#### 2.3.14 选择器的优先级

当不同选择器对于同一个元素设置的样式冲突时，显示的样式由选择器的权重决定，权重高的优先显示

* 内联样式	（1，0，0，0）		数量级：4
* id选择器     （0，1，0，0）
* 类和伪类     （0，0，1，0）
* 元素选择器  （0，0，0，1）
* 通配选择器   （0，0，0，0）

* 但是如果为一个样式的最后添加了!important，则改样式会获得最高的优先级（js也无法修改）
* 对于复合选择器来说，将所有的选择器的优先级相加之后再比较（优先级的计算不会超过其最大的数量级）
* 并集选择器的优先级分别计算
* 继承来的样式没有优先级



### 2.4 单位

* px

  * 像素
  * CSS像素，物理像素在PC端默认是相同的
  * 网页缩放后，CSS像素不变，物理像素改变
  * 不同的显示器下，像素的大小也不相同
  * 一些高清屏，会将像素放大
    * 一个CSS像素对应多个物理像素

* 百分比

  * 百分比会相对于包含块的指定属性去计算

* 颜色单位

  * 颜色名
  * RGB值
  * 三种rgb的方式

  ```css
  p {
      background-color: rgb(255, 0, 0);
  }
  
  div {
      /*
      	最后一个参数是不透明度
      */
      background-color: rgba(255, 0, 0, 0.5);
  }
  
  span {
      background-color: #ff0000;
  }
  ```

  * hsl
    * h -- 色相
    * s -- 饱和度
    * l -- 亮度



### 2.5 盒子模型-- box model

每一个盒子由内到外由以下几个部分组成

* 内容区（content）
  
  * 内容区决定这个盒子能装多少东西
  * 子元素在内容区中

* 内边距（padding）
  
  * 内容区和边框的距离
  
* 边框（border）
  
  * 盒子的边框
  
* 外边距（margin）
  
  * 盒子和其他盒子的距离
  
* 盒子的可见框大小由内容区，内边距和边框共同决定

> 默认情况下，width和height设置的是内容区的宽和高
>
> 默认情况下，padding、border、margin都没有设置，为0

![CSS 框模型](https://www.w3school.com.cn/i/ct_boxmodel.gif)

一般来说，width和height设置的是内容区的大小

* box-sizing属性默认值是content-box，就是width设置的是内容区
  
  * 如果将border-sizing设置border-box，就是设置的可见框的大小
  
* 子元素必须满足一个等式

  ```txt
  /*对于子元素在父元素内水平布局的等式*/
  margin-left + border-left + padding-left + width + padding-right + border-right + margin-right = 父元素的width
  
  // 如果七个值得和不等于父元素，则属于过度约束
  // 浏览器会自动调整margin-right的值
  ```

  * 注意：如果超过，margin-right会自动设置为负值
  * 在水平方向上，margin-left  width  margin-right可以设置为auto，浏览器会自动计算该属性的值
  * 如果margin-left和margin-right都设置为auto，则子元素水平居中
  * 如果将margin-left或margin-right的一个设置为auto，则浏览器会设置尽可能的大

* 竖直方向
  * 如果不为父元素指定高度，则父元素会自动适应子元素的高度，确保能容纳所有的子元素
  * 如果为父元素指定了高度，则指定多少就是多少；此时如果子元素的大小超过了父元素，则会导致子元素从父元素中溢出。溢出的子元素不会影响布局
  * 可以使用overflow来设置溢出内容的处理方式
    * visible：显示溢出部分
    * overflow：溢出部分会被隐藏
    * scroll：生成滚动条，显示完整内容
    * auto：根据需要生成滚动条

  * 垂直方向兄弟元素的相邻外边距会产生重叠（取最大值）

    * 两正：取大值
    * 一正一负：求和

    * 两父：取绝对值大的

  * 父子元素的相邻垂直外边距，子元素的外边距会传递给父元素

    * 处理方式1：设置父元素的内边距或border（父元素padding-top为1px，然后height相应地减少1px）
    * 处理方式2：添加一个空的table标签

    * 处理方式3：

```css
.box1::before {
    content:'';
    display:table;
}
```





#### 2.5.1 border

边框相关的三个样式

* border-style

  * 默认值是none
  * solid：实线
  * dotted：点线
  * dashed：虚线
  * double：双线
  * 设置规则与border-width一致

* border-width

  * 有默认值，chrome是3px

  ```css
  /*
  边框宽度为10px
  */
  border-width:10px;  
  
  /*
  	上、右、下、左
  */
  border-width:10px 20px 30px 40px;
  
  /*
  	上、左右、下
  */
  border-width:10px 20px 30px;
  /*
  	上下、左右
  */
  border-width:10px 20px;
  ```

* border-color
  * 设置的规则和border-width一样
  * 默认值是前景色，字体的颜色

边框会影响盒子的可见框大小（两倍边框的width + content的宽度或高度）

* border-top-left-radius:50px 100px
  * 指定的元素左上角的圆角
  * x轴是50px，y轴是100px
* border-radius
  * 设置规则与border-width一致
  * border-radius:50%   设置为正圆
* 可以单独设置某一边，例如
  * border-left-color
  * border-top
  * border-bottom
  * border-right
* border-collapse：设置边框合并
* seperate： 默认值
  * collapse

border可以用来制作水平或竖直的细线





#### 2.5.2 padding

边框和内容区之间的距离

一共有四个方向的内边距

* padding-top
* padding-right
* padding-bottom
* padding-left

还可以直接通过padding设置四个方向的内边距，设置规则和border一样

#### 2.5.3 margin

当前盒子与其他盒子之间的距离称为外边距

外边距不会影响盒子的可见框大小

但是外边距会影响到盒子的位置

一共有四个方向的外边距

* margin-top
* margin-right
* margin-bottom
* margin-left

由于在浏览器中默认情况下，元素是靠左靠上排列的，所以设置上、左外边距时，会移动自身；而设置下、右外边距时，会移动其他元素

* 注意：外边距可以设置为负值，往相反方向移动
* 可以同时设置四个方向的外边距，与padding设置规则一致



### 2.6 文档流（标准流、常规流）

> 文档流是网页中的一个位置，默认情况下页面中的所有元素都在文档流中排列

#### 2.6.1 块元素在文档流中的特点

1. 自上向下进行排列（独占一行）
2. 默认宽度和元素一样
3. 默认高度被内容撑开
4. 主要是用于布局的
5. 注：p元素不能放任何块元素
6. 注：a元素可以放置任何元素，除了a标签自己

#### 2.6.2 行内元素在文档流中的特点

1. 自左向右水平排列，如果一行中不足以容纳所有元素，则切换到下一行继续自左向右排列（和我们书写习惯一样）
2. 高度和宽度永远被内容撑开
3. 行内元素的盒子模型
   1. 内联元素不支持设置width和height
   2. 内联元素可以设置padding、border、margin，但是垂直方向不会影响页面其他元素的布局（即可能会盖住某些元素，而被盖住的元素仍然在原来位置）
   3. 水平方向的相邻外边距不会重叠，而是求和
   4. 主要是用来显示文字

#### 2.6.3 行内元素与块元素互相转化

* 设置元素的display属性
  * inline：行内元素
  * block：块元素
  * inline-block：行内块元素（行内块元素设置为inline-block之后既有行内元素的特点，不独占一行，又有块元素的特点，可以设置宽高）
  * none：元素不在页面中显示
  * 还有其他值，在特定场合中使用
* 可以实现菜单隐藏和显示

#### 2.6.4 块元素脱离文档流的特点

1. 默认宽度被内容撑开
2. 默认高度也是被内容撑开
3. 块元素不会独占一行
4. 浮动元素不会超过其上面的没有浮动的块元素
5. 浮动元素不会超过其上面的浮动元素，最多同在一行

#### 2.6.5 行内元素脱离文档流的特点

脱离文档流之后行内元素自动变成块元素，特点和块元素一样



### 2.7 样式

#### 2.7.1 常用的样式

* width
* height

#### 2.7.2 继承

为祖先元素设置样式，也会同时应用到其后代元素上

```html
<style>
    div {
        color:red;
    }
</style>
<div>
    <p>
        <!--p标签字体颜色也是red-->
        你好
    </p>
</div>
```

* 继承的存在大大简化了样式的编写，可以只为祖先元素设置样式，即可让所有后代元素都同时具有该样式
* 在开发中可以将一些公共样式，统一设置到祖先元素上，这样即可让所有的元素都具有该样式
* 注意：**背景、布局**相关的都不会被继承

#### 2.7.3 visibility

设置元素显示的状态

* visible： 默认值，元素正常显示
* hidden： 隐藏元素，但是元素依然占据位置

#### 2.7.4 去除默认样式

为了确保网页在没有样式的情况下也可以浏览，所以浏览器都会为网页设置一些默认样式，而且默认样式在不同的浏览器中的显示效果不一样，所以需要手动去除

* 网络上有写好的css文件，实现了去除默认样式的功能
* 也有写好的css文件，实现了统一样式的功能

#### 2.7.5 其他样式

* list-style：设置li标签的项目符号
  * list-style:none  清除项目符号
  * list-style:inside 将项目符号放入文本内

* outline：元素的轮廓
  * outline: 1px solid red;
  * 设置方式与border一致
  * 轮廓就是不占空间的边框，不会影响页面的布局
  
* box-shadow：阴影
  * box-shadow: inset 10px 10px  20px yellow;
  * inset  内部阴影
  * 第一个值是水平方向，向右移动
  * 第二个值是垂直方向，向下移动
  * 第三个值是阴影的模糊半径
  
  ```css
  box-shadow: inset 0px 0px 10px rgba(0,0,0,.5);
  ```
  
  * 多层阴影之间用逗号隔开
  
* floating：浮动
  * floating: left
  * none：默认值，不浮动
  * left：向左浮动
  * right：向右浮动
  * 浮动的特点
    * 设置浮动以后，元素会向页面的左侧或右侧移动
    * 设置浮动以后，元素会完全脱离文档流，不在占据文档流中的位置，所以下边的元素会上移
    * 浮动元素不会盖住或超过没有浮动的兄弟元素，也不会浮出父元素的边框
  * 对li设置float就可以让li都显示在同一行
  * 文字不会被浮动元素所覆盖，我们可以利用浮动来实现一个文字环绕图片的效果
  
* opacity

  * 不透明度
  * 1是完全不透明，0是透明

#### 2.7.6 background

* background
  * background:no-repeat url("star.png");
  * 简写属性，所有背景相关的样式都可以在background中设置，并且没有顺序要求（唯一一个是background-position在background-size前面  并且使用/分隔）
* background-color
* background-image
  * 如果图片小于元素，则图片默认从左上角开始平铺
  * 如果图片大于元素，则多余部分不会显示
  * 该属性在哪里编写，相对路径就相对于当前位置进行选择
  * 背景图片和背景颜色可以一起设置

```css
background-image:url("star.png");
```

* background-repeat：设置背景如何重复
  
  * 背景图片默认水平垂直双方向重复
  * repeat-x：水平重复
  * repeat-y：垂直重复
  * repeat：默认值
  * space
  * round
  
  * no-repeat：不重复
  * space-repeat
  
* background-size：设置背景图片的尺寸

  * cover：确保图片比例不变，填充元素
  * contain：确保图片比例不变，完整显示图片，可能会有位置没有图片
  
  ```
  background-size: 200px 100px;
  /*宽度100%，高度自适应
  相当于background-size:100% auto;
  */
  background-size:100%;
  ```
  
* background-position：用来设置背景图片的位置

  * top, left, right, bottom, center
  
  ```css
  /*一个参数*/
  background-position:top;
  
  /*两个参数*/
  /*右上角（九宫格的一个）*/
  background-position:top right;
  background-position:25% 75%;
  /*水平方向：200px  竖直方向：100px*/
  background-position:200px 100px;
  ```

* background-clip：设置背景延伸到的区域
  * border-box：背景延伸到border
  * content-box：背景延伸到content
  * padding-box
  * ie8不兼容
  
* background-origin：背景定位的原点

  * border-box
  * content-box
  * padding-box

* background-attachment：设置背景是否固定

  * scroll：滚动
  * fixed：背景在视口（viewport）固定

#### 2.7.7 精灵图Sprite

> 场景描述：使用三个图片来表示按钮的正常状态，悬浮状态和点击状态
>
> 问题描述：从link状态切换到hover状态，或者从hover状态切换到active状态时，第一次会出现闪烁！

* 原因： 浏览器在加载页面时，需要先加载当前页面，然后再加载页面中引入的外部资源
* 而且外部资源不是立即加载的，外部资源需要在被使用时才会加载，当我们从link状态切换到hover状态时，需要去加载hover状态的图片

* 而加载和显示存在一个时间差，这样在图片加载时会出现没有背景图片的情况，导致闪烁
* 解决方式：
  * 可以将多个图片保存到一张图片中，这样一次性加载所有的图片，然后通过偏移量来修改需要显示的图片即可
  * 这种技术我们称为CSS Sprite，这种图片称为精灵图（又叫雪碧图）
* 优点：
  * 将多个图片保存到一张图，降低发送请求的次数，增加用户的访问速度
  * 将多个图片保存到一个图片里，也会降低图片的总大小，增快加载速度

#### 2.7.8 定位

通过定位position我们可以将元素摆放到页面中的任意位置

使用position属性来设置元素的定位

* static     没有开启定位  默认值 

* relative    开启相对定位

  * 开启相对定位后，元素不会发生任何变化
  * 开启相对定位后，**元素不会脱离文档流**
  * 相对定位的元素，是相对于其在文档流中的位置进行定位的
  * 相对定位会使元素提升一个层级（可以覆盖其他元素）
  * 相对定位不会改变元素的性质，块元素和内联元素还是其原来的性质

* absolute  开启绝对定位

  * 绝对定位会使元素完全脱离文档流
  * 绝对定位会改变元素的性质
  * 开启绝对定位后，如果不设置偏移量，元素的位置不会发生变化
  * 绝对定位会相对于离它最近的开启了定位的祖先元素进行定位
    * 如果所有的祖先元素都没有开启定位，则相对于HTML标签进行定位
    * 所以一般情况，我们为一个元素开启了绝对定位，会同时为它的父元素开启相对定位
  * 绝对定位会将元素提升一个层级
  * 总结：绝对定位元素会相对于它的**包含块**进行定位
    * 包含块：
      * 默认情况下，离元素最近的块级祖先元素
      * 对于绝对定位元素来说，离元素最近的开启了定位的块元素
    * HTML元素是第一个包含块，也叫初始包含块
  * 绝对定位元素水平方向等式

  ```
  left + margin-left + border-left + padding-left + width + padding-right + border-right + margin-right + right = 包含块的宽度
  
  如果不满足该等式，会自动补偿right和left的值，如果想要margin自动设置，需要显示设置left和right
  ```

  ```css
  /*
  	设置水平垂直居中
  */
  .box {
      position: absolute;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      margin: auto;
  }
  ```

  

* fixed        开启固定定位

  * 特点和绝对定位几乎一样
  * 但是固定定位是相对于浏览器窗口（视口viewport）定位的（是固定的）

```txt
当元素开启定位之后（即position的值不等于static），可以通过四个方向的偏移量来控制元素的位置
top  	元素与其定位位置的顶部距离
bottom  元素与其定位位置的底部距离
left	元素与其定位位置的左侧距离
right	元素与其定位位置的右侧距离
默认值都是auto
```

* 开启定位的元素都会提升一个层级
  * 定位元素的层级可以使用z-index来设置
  * z-index需要一个整数作为参数，值越大层级越高，
  * 如果z-index值相同，则后边的元素会盖住前边的元素
  * 浮动元素设置z-index无效
* 注意：父元素的层级再高，也不会盖住子元素
  
* sticky 开启粘滞定位

  ```css
  ul {
  	overflow: auto;
  }
  ul .l1 {
  	position: sticky;
  	/*该元素的top偏移量不会低于0*/
  	top: 0;
  }
  ```

#### 2.7.8 字体

* font-size：字体大小

  * 1em：css中的一个单位，代表一个font-size

  ```css
  height: 1em;
  width: 1em;
  font-size: 100px;
  ```

  * 1rem：css的一个单位，代表HTML的1个font-size（默认16px）

* color：字体颜色

* text-decoration：设置字体的修饰
  
  * text-decoration:none  一般用来取消a超链接的下划线
  * underline：下划线
  * line-through：删除线
  * overline：上划线
  
* text-align：设置文本的位置
  
  * start：默认值
  * left
  * center
  * right
  * justify：两端对齐
  * text-align:center  设置水平居中
  
* vertical-align

  * baseline：默认值
  * top：和父元素的顶部对齐
  * bottom
  * super：上标
  * sub：下标
  * middile：基线加上半个x的高度的位置
  * 数值：基线加上数值之后的位置

* text-transform：设置字母的大小写
  * capitalize：首字母大写
  * uppercase
  * lowercase
* none
  
  * full-width
* full-size-kana
  
* text-indent：设置首行缩进

  * text-indent:2em;    首行缩进两个字

* white-space：处理空白内容

  * normal：默认值，自动换行
  * nowrap：不换行
  * pre：保留文本格式

* text-overflow：溢出文字的处理方式

  * ellipsis：省略号

* 溢出的文本显示省略号

  ```css
  white-space: nowrap;
  overflow: hidden;
  text-overflow:ellipsis;
  ```

* letter-spacing：设置字母的间距

* word-spacing：设置单词之间的距离

* font-family

  * 可以指定字体的分类和具体的字体类别
  * 可以指定多个字体，用逗号隔开
    * 使用时，如果第一个字体不可以用，就用下一个（有些字体对中文无效）
  * 字体的分类
    * 衬线字体serif
      * 字体宽度各异，有衬线
      * Times, Georgia, 宋体
    * 无衬线字体sans-serif
      * 字体宽度各异，无衬线
      * Helvetica, Verdana, Arial，微软雅黑
    * 等宽字体monospace
      * 字体宽度一样，一般用于代码或表格
      * Courier New， Consolas
    * 草书字体cursive
      * 模仿人手写的字体
      * Indie Flower
    * 装饰字体fantasy
      * 没有什么统一特征，不属于上述的字体
  
* 引入项目内的字体

  ```css
  @font-face {
      	/*指定字体的名称*/
      font-family: 'test';
      /*本地字体的路径，必须在项目里
      	如果第一个url不可以使用，就使用下一个（提高兼容性）
      */
      src: url('./font/zcool.ttf'),
          url(),
          ...,
          url();
  }
  ```

* 一些图标可以使用图标字体（矢量图，不会失真）

  * 如font awesome

    ```
    使用步骤
    1.将css和fonts文件夹放入项目
    2.引入css文件
    ```

    * 都是在fas类下

  * 可以使用font-size调整图标大小
  
  * 还可以通过编码设置内容，然后将font-family设置为指定的字体族
  
  * 也可以通过阿里的IconFont图标库
  
    * 都是在iconfont类下
      1. 选择想要的图标（最好是单色的），加入购物车
      2. 添加到项目中
      3. 下载到本地
      4. 引入文件
      
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>阿里图标字体</title>
        <link rel="stylesheet" href="iconfont/iconfont.css">
        <style>
          /*
            第三种方式
           */
          .fake-class::before {
            content: "\e8ae";
            font-family: "iconfont";
          }
        </style>
      </head>
      <body>
        第一种引用方式，通过编码和class来引用:   <span class="iconfont" style="font-size:40px">&#xe8ad;</span><br>
        第二种引用方式，通过class属性:   <i class="iconfont icon-dingwei" style="font-size:40px"></i><br>
        第三中引用方式，通过伪元素： <span class="fake-class"> Beijing</span>
      </body>
      </html>
      ```
  
* line-height：设置行高，文字自动在行高的中间垂直居中

  * line-height:1       行高设置一个整数，则行高等于字体的倍数
  * 默认是1.33
  * 在文字框中，文字并不是贴着行的底部排列，是沿着行框的基线（base-line）排列的
  * 行间距 = 行高 - 字体大小
  * 当行高等于父元素的高度，文字自动水平居中

* font-weight：字体的粗细

  * 100 - 900之间的值，值越大，越粗
  * 注意：200不一定比100粗，可能一样
  * 所以，一般使用bold（700， 正常值是400）来加粗

  ```css
  font-weight： bold
  ```

* font-style：设置字体的样式（斜体）

  * normal：正常，默认值
  * italic：斜体
  * oblique：倾斜

* font-variant：字体变形

  * small-caps：将小写字母大写，但是比正常大写字母要小

* font

  ```
  font: bold italic small-caps 40px '微软雅黑';
  ```
  * font-size必须是倒数第二个
  * font-family必须是倒数第一个
  * font: [加粗 斜体 变形]  大小/行高  字体族;

### 2.8 布局

使用块元素和浮动设计

#### 2.8.1 高度塌陷

> 问题描述：
>
> 块元素的高度默认情况下是被子元素撑开的
>
> 如果子元素浮动，将会完全脱离文档流，脱离文档流子元素无法撑起父元素高度，将会导致父元素高度丢失
>
> 父元素的高度一旦丢失，页面中的其他元素会向上移动，会影响页面的布局

解决办法：

* BFC --- Block Format Context

  * 块级格式化环境
  * BFC是元素的一个隐藏的属性，一旦元素开启了BFC，它将会开启一个独立的布局的区域
    1. 开启了BFCd的元素子元素的垂直外边距不会传递给父元素
    2. 开启了BFC的元素不会被浮动的元素覆盖
    3. 开启了BFC的元素可以包含浮动的子元素

* 开启BFC的方法

  * BFC无法直接开启，需要通过一些属性的设置开启BFC

  1. 设置元素浮动可以开启BFC
  2. 将元素设置为行内块元素
  3. 将元素的overflow设置为一个非visible的值

* 总结：讲父元素设置`overflow: hidden;`开启父元素的BFC，从而解决高度塌陷的问题（但是有隐患）

#### 2.8.2 clear清楚浮动元素的影响

* clear用来清除浮动元素对当前元素的影响

  * none：默认值，不清楚
  * left：清楚左侧浮动元素对当前元素的影响
  * right：清楚右侧浮动元素对当前元素的影响
  * both：清楚两侧中对当前元素影响最大的一侧
  * 底层：浏览器会自动设置当前元素的外边距为浮动元素的高度

* 也可以用来解决高度塌陷

  ```css
  .box1 {
      border:10px solid red;
  }
  .box2 {
      width:200px;
      height:200px;
      background-color: #bfa;
      float:left;
  }
  .box1::after {
      content:'a'
      clear: both;
      display:block;
  }
  ```

  ```html
  <div class="box1">
      <div class="box2"></div>
  </div>
  ```

  

