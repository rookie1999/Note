# jQuery
## 1. jQuery基础
### 1.1 jQuery入口函数
#### 1.1.1 入口函数形式
```
    第一种
    $(document).ready(function() {
        
    });
```
#### 1.1.2 与JS的页面加载函数的异同
* 与原生JS的window.onload类似，但是在加载页面的时候才执行
* 区别
    * 加载模式不同
        * jQuery的入口函数是在页面加载DOM元素的时候之后执行，此时页面的图片还没加载
        * 原生的JS是在DOM元素和图片加载之后才执行
    * 执行次数
        * jQuery的入口函数编写多个，会依次执行
        * 原生JS的加载函数会覆盖掉上一个，最后只执行最后一个加载函数
#### 1.1.3 入口函数的其他形式
```
    第二种
    jQuery(document).ready(function() {
    
    });

    第三种（推荐）
    $(function() {
    
    });

    第四种
    jQuery(function() {
    
    });
```
#### 1.1.4 jQuery的冲突问题
* 问题：在使用jQuery的时候，同时也使用了其他的框架。如果此时$在至少两个框架之中被定义了，则会产生$不明确的冲突。
* 解决办法：
    1. 释放jQuery对于$的定义,使用jQuery作为代替
        jQuery.noConflict();
    2. 自定义一个访问符号来代替$
       var _$ = jQuery.noConflict();
       以后使用_$来编写jQuery