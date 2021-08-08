> 视频网址：https://www.bilibili.com/video/BV1cJ411R7bP?from=search&seid=17907533560601118423
>
> 文档：http://www.python3.vip/tut/py/gui/qt_01/
>
> 代码位置：D:\pycharm\code\qt_gui

## 安装

```
pip install pyside2
```

## Qt Designer

程序位置

```
D:\Anacode\envs\exercise\Lib\site-packages\PySide2
```

可视化的设计页面的控件及其位置，生成ui文件（本质是xml文件）

### 动态加载ui文件



## Layout

* QHBoxLayout   水平布局
* QVBoxLayout    垂直布局
* QGridLayout      网格布局
* QFormLayout    表格布局

水平布局保证控件在水平方向中间，垂直布局保证垂直方向中间

对界面控件进行布局，经验是：

按照如下步骤操作

- 先不使用任何Layout，把所有控件 按位置 摆放在界面上
- 然后先从 最内层开始 进行控件的 Layout 设定
- 逐步拓展到外层 进行控件的 Layout设定
- 最后调整 layout中控件的大小比例， 优先使用 Layout的 layoutStrentch 属性来控制



## 发布可执行程序

安装PyInstaller

```
pip install pyinstaller
```

将`name.py`进行打包发布

```
pyinstaller name.py --noconsole --hidden-import PySide2.QtXml

######################
--noconsole：  运行的打包后的可执行程序的时候，不显示命令行窗口
--hidden-import PySide2.QtXml：  QtXml库是动态导入，需要指定
```

发布的版本在当前目录的dist文件夹的name文件夹下

如果有一些程序运行时需要动态载入的文件（比如ui文件），需要复制到name文件夹内

## 程序图标

**添加主窗口图标**

```python
from PySide2.QtGui import QIcon
app = QApplication([])
app.setWindowIcon(QIcon('logo.png'))
```

注意：图标文件需要在发布后拷贝到程序所在目录

**添加发布的应用程序的图标**

```
pyinstaller name.py --noconsole --hidden-import PySide2.QtXml --icon='logo.ico'
```

注意：参数一定是ico文件，不能是png等图片文件

图片转ico文件的网站：https://www.easyicon.net/covert/

## 单选框QRadioButton

只有在一个按钮组或者是一个布局内的单选框或者是在一个QGroupBox内，互相影响只能选一个

## QSS

类似于CSS

在每个控件对象的stylesheet属性中，添加类似如下的语法：

```
QPushButton{
	color: red;
	font-size: 15px
}
```

### selector

![](D:\大学\笔记\python gui\pyside-pic\1.png)

## 多线程

Qt建议：只在主线程中操作界面

在另外一个线程中操作界面，可能会导致意想不到的结果，比如：输出显示不全，甚至程序崩溃