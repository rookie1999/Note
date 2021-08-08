> 视频：https://www.bilibili.com/video/BV1Qx41127FJ?p=4
>
> 代码：

## 1、开发环境

1. XAMPP （或者是Apache+Php）
2. PhpStorm

### 1.1 Apache的使用

* 运行`httpd.exe`启动Apache服务
* `httpd -M`查看模块
  * static：静态加载，服务启动时系统加载
  * shared：动态加载，需要使用时才加载
* `httpd -t`查看配置文件是否有错

### 1.2 Apache配置默认站点

* Apacheq确定服务器访问的位置：网站文件夹所在的位置
  * httpd.conf文件中的`Document Root`
* 方便用户使用名字访问对应的网站：给文件夹取一个别名
  * httpd.conf文件中的`ServerName`
* 监听端口
  * httpd.conf文件中的`Listen`
* 如果Apache的配置文件修改，需要重启服务

### 1.3 Apache加载PHP

* Apache加载php模块

  ```
  LoadModule <php模块名>  <php提供的模块链接所在路径>
  
  LoadModule php7_module "D:/php-7.4.13-Win32-vc15-x64/php7apache2_4.dll"
  ```

* 将PHP的配置文件加载到Apache配置文件中

  ```
  PHPIniDir <php.ini所在路径>
  
  PHPIniDir "D:\php-7.4.13-Win32-vc15-x64"
  ```

* Apache分配工作给PHP，如果文件是PHP，就交给PHP模块解析

  ```
  AddType application/x-httpd-php .php
  ```

* 将PHP文件夹下的`php.ini-development`修改为`php.ini`
* 注意：此时修改`php.ini`也会影响到Apache，需要重启Apache服务



## 2、编写PHP

- 如果是在php文件里全部编写PHP，可以没有结束标签
- 如果是在html中嵌入php，必须添加结束标签

```php
<?php
    
?>
```



## 3、变量和常量

* 定义变量

  ```php
  $a = 5;
  $b = 10;
  echo $a + $b;
  ```

* 定义常量

  * php5以后的做法

  ```php
  const CONST_VALUE = 3.14;
  echo CONST_VALUE;
  ```

  * php5以前的做法

  ```php
  define('THE_VALUE', 100);
  echo THE_VALUE;
  ```



## 4、函数

* 定义并使用一个无参无返回值的函数

  ```php
  function tracePhP() {
          echo "Hello Php<br>";
          echo "Hello World<br>";
      }
  tracePhP();
  ```

* 函数的另外一种使用方式

  ```php
  $func = "tracePhP";
  $func();
  ```

* 定义并使用有参无返回值的函数

  ```php
  function sayHelloTo($name) {
          echo "Hello ".$name."<br>";
      }
  sayHelloTo("zhangsan");
  sayHelloTo("lisi");
  ```

* 定义有参有返回值的函数

  ```php
  function add($num1, $num2) {
          echo "num1 = $num1, num2 = $num2<br>";
          return $num1 + $num2;
      }
  echo add(12, 13)."<br>";
  ```



注意：用echo输出字符串有两种方式

1. `echo "name ".$name."<br>"`
2. `echo "name = $name<br>"`



## 5、流程控制

* if语句（与Java不同的是elseif是连在一起写的）

  ```php
  function getGradeLevel($grade) {
          $result = null;
          if ($grade >= 90) {
              $result = "优秀";
          } elseif ($grade >= 80) {
              $result = "良好";
          } elseif ($grade >= 70) {
              $result = "中等";
          } elseif ($grade >= 60) {
              $result = "合格";
          } else {
              $result = "差";
          }
  
          return $result;
  }
  ```

* switch语句

  ```php
  function getGradeLevelUsingSwitch($grade) {
          $result = null;
          switch (intval($grade / 10)) {
              case 10:
              case 9:
                  $result = "优秀";
                  break;
              case 8:
                  $result = "良好";
                  break;
              case 7:
                  $result = "中等";
                  break;
              case 6:
                  $result = "合格";
                  break;
              default:
                  $result = "差";
          }
          return $result;
  }
  ```



## 6、循环

* for循环

  ```php
  for ($i = 0; $i < 100; $i++) {
          echo "Hello for ".$i."<br>";
  }
  ```

* while循环

  ```php
  $i = 0;
  while ($i < 10) {
      echo "Hello while ".$i."<br>";
      $i++;
  }
  ```

* do while 循环

  ```php
  $i = 0;
  do {
      echo "Hello do while ".$i."<br>";
      $i++;
  } while ($i < 10);
  ```



## 7、字符串

* strpos(str, char | substr)

  * 获取一个字符或者字符串在原字符串的位置

  ```php
  echo strpos($str, 'll');
  ```

* substr(str, offset, `<length>`)

  * 获取一个字符串从offset开始，长度为length的子字符串
  * 如果不存在，返回空
  * 如果不指定length，默认到字符串末尾

  ```php
  echo substr($str, 2);
  echo "<br>";
  echo substr($str, 2, 5);
  ```

* str_split(str, length)

  * 将str划分为元素长度是length的数组

  ```php
  $result = str_split($str, 2);
  print_r($result);
  ```

* explode(delimiter, str)

  * 按照分隔符划分字符串

  ```php
  $result1 = explode(" ", $str);
  print_r($result1);
  ```

* strcmp($a, $b)
  
  * 比较两个字符串（大小写敏感）



## 7、数组

* 声明一个空数组

  * `$arr = array();`

* 赋值方式1

  * `$arr[0] = 'Hello';`
  * `$arr['h'] = 'Hello'`

* 赋值方式2

  ```php
  for ($i = 0; $i < 10; $i++) {
          array_push($arr, $i);
  }
  ```

* 赋值方式3

  * 通过初始化赋值

  ```php
  $arr1 = array('h' => 'Hello', 'w' => 'World');
  ```

总结：PHP中的数组既可以当作普通数组使用数字索引来处理，也可以当作map使用键值对来处理



* array_walk($array, $callback)
  * 对$array中的每一个元素都调用一次callback函数进行处理
* sizeof($arr)
  * 获取数组长度
* unset($arr[$i])
  * 删除索引为$i的数组元素
* array_splice($arr, $offset, $len)
  * 删除从$offset开始，长度为$len的数组元素



## 8、include和require

* 相同之处
  * include和require都把一个外部的php文件引入到当前文件
* 不同之处
  * include引入的文件如果不存在，会产生warning
  * require引入的文件不存在，会产生error

如果使用include或require引入相同的文件两次，该文件会被解析两次，如果其中有函数被定义，会产生error提示函数被重复定义

这时候需要使用关键字

* require_once
* include_once



## 9、类和命名空间

* 在php中声明一个类

  ```php
  class Hello {
  	public function sayHello() {
          echo "Hello, my friend";
      }
  }
  ```

* 相同名称的类可以放到不同的命名空间

  ```php
  namespace classes;
  class Hello {
  
      public function __construct() {
          echo "Hello object is created<br>";
      }
  
      public function sayHello() {
          echo "Hello, my friend";
      }
  }
  ```

* 创建实例

  ```php
  // 创建默认命名空间的实例
  $obj = new Hello();
  // 创建命名空间为classes的实例
  $obj = new \classes\Hello();
  ```



## 10、构造方法

* php中的构造方法是`__construct`

  ```php
  namespace classes;
  
  
  class Person {
      private $_name;
      private $_age;
  
      /**
       * Person constructor.
       * @param String $name
       * @param int $age
       */
      public function __construct($name, $age) {
          $this->_name = $name;
          $this->_age = $age;
      }
  
      public function getName() {
          return $this->_name;
      }
      public function getAge() {
          return $this->_age;
      }
  
  }
  ```

  ```php
  $zhangsan = new \classes\Person("zhangsan", 18);
  echo $zhangsan->getName()."<br>";
  echo $zhangsan->getAge()."<br>";
  ```



## 11、成员方法和类方法

* 成员方法：需要通过对象来调用

  ```php
  public function sayHello() {
  	echo "Hello, my friend<br>";
  }
  ```

* 类方法：可以通过类或者对象来调用，推荐使用类

  ```php
  public static function sleep() {
      echo "Sleep<br>";
  }
  ```

  * 类方法比成员方法多了一个static

* 静态变量

  ```php
  private static $abc;
  ```

* 常量

  ```php
  const MAX_VALUE = 100;
  ```

* 调用

  ```php
  // 调用静态方法
  Person::sleep();
  // 调用静态变量
  Person::MAX_VALUE;
  ```



## 12、继承

* 类A可以继承另一个类B

  * A是子类
  * B是父类

* 继承的关键字是extend

  ```php
  class B extends A {
      
  }
  ```

```php
class Person {
    private $_name;
    private $_age;
    private $_sex;

    public function __construct($name, $age, $sex) {
        $this->_name = $name;
        $this->_age = $age;
        $this->_sex = $sex;
    }

    public function introduction() {
        echo "I am ".$this->_name." and ".$this->_age.". I am ".$this->_sex.".<br>";
    }

    public function getName() {
        return $this->_name;
    }
    public function getAge() {
        return $this->_age;
    }
    public function getSex() {
        return $this->_sex;
    }
}

class Male extends Person {
    private $_height;

    public function __construct($name, $age, $height) {
        // 调用父类的构造方法
        parent::__construct($name, $age, "male");
        $this->_height = $height;
    }

    public function introduction() {
        echo "I am ".$this->getName()." and ".$this->getAge().". I am ".$this->getSex()
            ." and I am ".$this->_height.".<br>";
    }
}

$man = new Male("lisi", 28, 1.72);
$man->introduction();
```



## 13、时间和日期

* 获取当前时间的时间戳
  * 基于1970年的时间
  * time()
* 获取当前设置的默认时区
  * date_default_timezone_get()
* 设置当前的默认时区
  * date_default_timezone_set('Asia/Shanghai')
* 获取当前的日期
  * date('Y-m-d H:i:s')
* 获取指定时间戳的日期
  * date('Y-m-d H:i:s', '30000')

```php
echo "The current timestamp is ".time()."<br>";
echo "The current date is ".date('Y-m-d H:i:s')."<br>";
echo "The default time zone is ".date_default_timezone_get()."<br>";
date_default_timezone_set('Europe/Berlin');
echo "The default time zone is ".date_default_timezone_get()."<br>";
echo "The current date is ".date('Y-m-d H:i:s')."<br>";
```



## 14、Json数据

* 对象转成json数据

  * json_encode($obj)

  ```php
  $arr = array(1, 2, 3, array("name"=>"zhangsan", "age"=>"18"));
  echo json_encode($arr);
  ```

  ```
  // result
  [1,2,3,{"name":"zhangsan","age":"18"}]
  ```
  * 注意：如果传入的对象中数组元素的子元素是以键值对形式存储的，则会被当作是对象处理

* json数据转换成对象

  * json_decode($obj)

  ```php
  $json_str = '[{"name":"zhangsan"}, {"name":"lisi"}, {"name":"wangwu"}]';
  $obj = json_decode($json_str);
  echo "start to print<br>";
  print_r($obj);
  ```

  ```
  // result
  Array ( [0] => stdClass Object ( [name] => zhangsan ) [1] => stdClass Object ( [name] => lisi ) [2] => stdClass Object ( [name] => wangwu ) ) 
  ```

  * 注意：编写json格式数据时，最外面一层是单引号，里面使用双引号
  * 即`'{"name":"zhangsan", "age":14}'`



## 15、文件操作

* fopen($file_name, $mode)
  * 打开一个文件，模式为$mode,返回一个与文件关联的流
  * $mode有很多种，常用的有：
    * w :写入（将pointer放在文件开始，即写入会进行覆盖）
    * r ：只读
    * a ：追加（将pointer放在文件最后）

* fwrite($stream, $content)
  * 向流$stream中添加$content
* fclose($stream)
  * 关闭一个流$stream
* fgets($file)
  * 读取$file流中的一行，并将pointer移至下一行
* feof($file)
  * 返回true，即到达文件末尾

```php
// 写入操作
// 打开文件，获取一个流，该流的模式是'w'，即写入模式
$file = fopen('php created file', 'a');
//// 向流$file写入“Hello PHP ”
fwrite($file, "Hello PHP ");
fclose($file);

// 读取操作
$file = fopen('D:\migong.txt', 'r');
while (!feof($file)) {
    $content = fgets($file);
    echo $content."<br>";
}
```

* mkdir($path)
  * 创建目录
* rmdir($path)
  * 删除目录





## 16、图片操作

* imagecreate(width, height)
  * 创建一个宽为width，高为height的图片
* imagepng($img, $file_name)
  * 将图片输出到浏览器或文件中
  * $file_name是可选值
  * 也可以有`imagejpeg`
* imagecolorallocate($img, red, green, blue)
  * 返回一个填充颜色
  * rgb为0-255的一个值
* imageellipse($img, x, y, width, height, color)
  * 在$img的（x，y）处画一个宽为width，高为height的椭圆，边框的颜色为color
* imagestring($img, font, x, y, string, color)
  * 给$img在(x,y)处添加字体为font，颜色为color的string水印
  * font有1，2，3，4，5，分别对应5种built-in字体
  * 也可以使用imageloadfont()方法加载指定字体

* header("Content-type:image/png")
  * 设置MIME type为image/png
  * 因为默认以文本形式输出，显示为乱码，需要指定解析的类型

```php
$img = imagecreate(400, 300);
imagecolorallocate($img, 51, 51, 51);
imageellipse($img, 100, 100, 100, 50,
    imagecolorallocate($img, 255, 0, 0));
header("Content-type:image/png");
imagepng($img);
```

注：如果显示无法使用function，则需要在php.ini配置文件中开启`php_gd2.dll`库



## 17、水印实例

```php
$img = imagecreatefromjpeg('img/winter.jpg');
imagestring($img, 3, 10, 10, "belong to Mr.Zhan", imagecolorallocate($img, 0, 0, 0));
header("Content-type:image/png");
imagepng($img);
```



## 18、处理Get请求

get方式表单提交的数据会存储到`$_GET['key']`，key对应url的key，即表单中的name属性

```php
// 判断收到的get请求中是否有username作为键的值，并且不是空
if (isset($_GET['username']) && $_GET['username']) {
    echo $_GET['username'];
} else {
    echo 'Please upload your name';
}
```



## 19、处理POST请求

```
$_SERVER['REQUEST_METHOD'] == 'POST'
```

post方式提交的数据会保存到`$_POST['key']`，key对应表单中的name属性

```php
if ((isset($_POST['num_1']) && $_POST['num_1']) || (isset($_POST['num_2']) && $_POST['num_2'])) {
    $sum = $_POST['num_1'] + $_POST['num_2'];
    echo "The sum is ".$sum."<br>";
} else {
    echo "The inputs have some problems<br>";
}
```



## 20、文件上传

* 上传网页

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>上传文件</title>
  </head>
  <body>
  上传文件form表单的method属性必须是post，而enctype必须是multipart/form-data<br><br><br><br>
  <form action="upload.php" method="post" enctype="multipart/form-data">
      <input type="file" name="jpg">
      <input type="submit">
  </form>
  </body>
  </html>
  ```

  * method属性必须是post
  * enctype必须是multipart/form-data

* 处理文件上传的网页

  ```php
  <!doctype html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport"
            content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
  <?php
  //    Array ( [jpg] => Array ( [name] => 2.jpg [type] => image/jpeg [tmp_name] => D:\php-7.4.13-Win32-vc15-x64\uploadfile\phpB612.tmp [error] => 0 [size] => 33326 ) )
      /*
       * 上传的文件会存放在$_FILES这个变量中，
       * 上传项在原网页中的name属性会作为$_FIELS数组中的索引（Key）
       * 每个上传项会有name，type，tmp_name，error，size五个key
       */
      $file = $_FILES['jpg'];
      print_r($file);
      $file_name = $file['name'];
  	// 移动文件的方法
      move_uploaded_file($file['tmp_name'], $file_name);
      echo "<img src='".$file_name."'><br>";
  ?>
  </body>
  </html>
  ```

  * 上传的文件会存放在$_FILES这个变量中
  * 上传项在原网页中的name属性会作为$_FIELS数组中的索引（Key）
  * 每个上传项会有name，type，tmp_name，error，size五个key
  * name: 上传文件的名称（包括扩展名）
  * type：上传文件的类型
  * tmp_name：上传文件的临时文件的位置（网页关闭后会被释放）
  * error：文件是否有错误
  * size：文件的大小
  * 移动文件的方法`move_uploaded_file`



## 21、cookie的使用以及禁用后的方案

* 使用setcookie(key_str, value_str)来创建或刷新cookie

* cookie会被存放到`$_COOKIE`中

  ```php
  setcookie("hello", "hello how are u?");
  
  header("Location:look_cookie.php");
  ```

  ```php
  echo $_COOKIE['hello'];
  ```

* 当cookie禁用之后，可以使用url参数来进行设置

  ```php
  header("Location:look_url_param.php?sex=male&date=20201222");
  ```

  ```php
  echo "The sex is ".$_GET['sex']."<br>";
  echo "The date is ".$_GET['date']."<br>";
  ```



## 22、session的使用

* session存储的内容会存放到`$_SESSION`，并以键值对存储

* 存放session

  ```php
  $_SESSION['name'] = 'zhangsan';
  ```

* session_start()
  * 当前php页面如果需要使用session，需要在使用前调用此方法，表明session已启用
  * 如果不调用此方法，直接使用'$_SESSION'会显示undefined variable
  
* session_id()
  
  * 获取当前session的id值
  
* session_destroy()
  
  * 销毁当前session中的所有内容
  
* unset($_SESSION['name'])

  * 删除session中name属性

```
注意：php中session对象存储取出问题
对象需要序列化（因为存取的是字符串）
然后操作完毕之后重新存储到session中
```





## 23、表单验证

* 对于optional字段
  * 去除首尾空格（trim()）
  * 删除反斜杠（stripslashes()）
  * 将特殊字符转为html entites，防止脚本嵌入（htmlspecialchars()）
  
  ```php
  function test_input($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
  }
  ```
  
* 对于required字段
  
  * 先要进行非空判断（empty()）



## 24、操作Mysql数据库

### 24.1 建立和关闭连接

* 连接指定的mysql服务器

  ```php
  $mysqli_connect = @mysqli_connect($host, $user, $password, $database, $port);
  ```

* 连接时的错误提示

  * int mysqli_connect_errno() 		返回最后一次连接调用的错误代码
  * string mysqli_connect_error()     返回最后一次连接调用的错误代码的字符串表示

* 设置默认字符编码

  * bool mysqli_set_charset($conn, $charset)

  ```php
  mysqli_set_charset($conn, 'utf8');
  ```

* 选择特定的数据库

  * bool mysqli_select_db($conn, $dbName)

* 关闭连接

  * bool mysqli_close($conn)

### 24.2 执行SQL语句

* $resultset mysqli_query($conn, $query [, int $resultMode = MYSQLI_STORE_RESULT])

  * 对于insert、update、delete等不会返回结果的SQL语句，在执行没有错误时将返回true
  * 对于返回数据的SQL语句，执行成功时会返回结果集对象，可以使用操作结果集对象的函数从中获取数据

  ```
  MYSQLI_STORE_RESULT和MYSQLI_USE_RESULT决定了mysqli client和server之间取结果集的方式
  MYSQLI_STORE_RESULT:执行SQL时提起结果集返回给client，并分配内存，存到用户程序空间中，之后mysqli_fetch_array或mysqli_fetch_assoc相当于是从本地取数据
  而MYSQLI_USE_RESULT每次都需要向服务器请求数据
  
  默认是MYSQLI_STORE_RESULT
  ```

  ```php
  // 插入数据
  $query = 'insert into member values("石原里美", "2")';
  $res = mysqli_query($conn, $query);
  ```

  ```php
  $query = 'select * from member';
      $result_set = mysqli_query($conn, $query);
  
      // 使用非默认的MYSQLI_USE_RESULT查询，直接获取结果集会产生警告
      // 需要等所有的数据获取到本地，才可以使用
  //    $result_set = mysqli_query($conn, $query, MYSQLI_USE_RESULT);
      // 获取查询到的结果行数
  //    var_dump(mysqli_num_rows($result_set));
  
  
      // 一次性获取全部数据mysql_fetch_all
  //    var_dump(mysqli_fetch_all($result_set));
  
      // 使用mysqli_fetch_row获取索引数组形式的结果
  //    while ($data = mysqli_fetch_row($result_set)) {
  //        var_dump($data);
  //        echo "<br>";
  //    }
      // 使用mysqli_fetch_assoc获取关联数组形式的结果
  //    while ($data = mysqli_fetch_assoc($result_set)) {
  //        var_dump($data);
  //        echo "<br>";
  //    }
      // 使用mysqli_fetch_array获取索引或关联数组形式的结果
      while ($data = mysqli_fetch_array($result_set)) {
          var_dump($data);
          echo "<br>";
      }
  	// 释放结果集
  	mysqli_free_result($result_set);
  ```

  

* bool mysqli_real_query($conn, $query)

  * 执行sql语句，返回布尔值
  * 如果想要再获取结果集，可以使用`mysqli_store_result()`方法

* int mysqli_num_rows($result)

  * 返回结果集中的行数
  * 注意：如果使用MYSQLI_USE_RESULT模式，则必须在获取完所有的结果之后才可以使用该函数

* mysqli_fetch_row($result)

  * 返回一行数据，以索引数组的形式 

* mysqli_fetch_assoc($result)

  * 返回一行数据，以关联数组的形式

* mysqli_fetch_array($result [, $mode])

  * 返回一行数据，以关联或索引数组的形式
  * `$mode`是可选值，默认值是`MYSQLI_BOTH`
    * `MYSQLI_ASSOC`返回关联数组
    * `MYSQLI_NUM`返回索引数组
    * `MYSQLI_BOTH`每一次返回的数组既包含关联数组，也包含索引数组

* mysqli_fetch_all($result [, $mode])

  * 一次性返回所有数据
  * $mode的默认值是`MYSQLI_NUM`

* mysqli_free_result($result)

  * 释放结果集的内存
  * 返回的结果集在数据量很大时需要很多的内存支持，所以在操作完结果集的时候有必要立刻释放与一个结果集相关的内存，释放之后，结果集就不可用了

* int mysqli_errno($conn)

  * 返回最后一次执行SQL语句出错后的错误代码

* string mysqli_error($conn)

  * 返回最后一次执行SQL语句出错后的错误代码的详细说明

### 24.3 常用函数

* int mysqli_affected_rows($conn)
  
* 获取连接最后一次执行SQL的受影响行数
  
* int mysqli_insert_id($conn)

  * 返回最后一次操作自动生成并使用的id

* mysqli_real_escape($conn, $str)

  * 将$str字符串中的一些特殊字符进行转义，使其能够正常入库

* mysqli_multi_query($conn, $query)

  * 一次性执行多条SQL语句，多个SQL语句使用分号隔开

  ```php
  $query = 'use test;'
          .'insert into member values ("敖犬", 0);'
          .'insert into member values ("马薇薇", 0);'
          .'update member set gender = 1 where name = "马薇薇"';
  var_dump(mysqli_multi_query($conn, $query));
  ```

  

### 24.4 预处理语句机制

> 预处理语句机制：将整个语句只向MYSQL服务器发送一次，以后只有参数发生变化，所以MYSQL服务器只需对语句的结构做一次分析就够了
>
> 这样既减少了需要传输的数据量，还提高语句的处理效率

1. $stmt mysqli_prepare($conn, $query)
   * 生成一个预处理的语句，其中参数部分使用`?`进行占位

2. bool mysqli_stmt_bind_param($stmt, $types, $val1...)
   * 为预处理语句绑定参数
   * types：表示后面多个可选参数变量的数据类型
     * i: int类型
     * d: double或float类型
     * s: 字符串类型
     * b: 二进制数据类型

3. bool mysqli_stmt_execute($stmt)
   * 执行一个预处理语句
4. bool mysqli_stmt_bind_result($stmt, $val1,...)
   * 将查询的数据绑定到PHP变量上
5. bool mysqli_stmt_fetch($stmt)
   * 从一个prepared语句的执行结果集抓取一行到之前使用`mysqli_stmt_bind_result`绑定的变量当中
6. bool mysqli_stmt_store_result($stmt)
   * 取回一个结果集
7. int mysqli_stmt_num_rows($stmt)
   * 返回语句结果集中的行数
8. void mysqli_stmt_free_result($stmt)
   * 释放给定语句处理存储的结果集所占内存
9. bool mysqli_stmt_close($stmt)
   * 关闭一个prepared语句

```php
	// step 1:创建一个预处理sql语句
    $query = 'select * from member where gender = ?';
    // step 2:创建一个SQL预处理语句
    $stmt = mysqli_prepare($conn, $query);
    // step 3:为预处理语句绑定参数
    mysqli_stmt_bind_param($stmt, 'i', $val1);
    $val1 = 0;
    // stpe 4:执行预处理语句
    mysqli_stmt_execute($stmt);
    // step 5:将结果绑定到变量上
    mysqli_stmt_bind_result($stmt, $name, $gender);
    // step 6:保存结果到本地
    mysqli_stmt_store_result($stmt);
    // step 7:获取查询到的结果数量
    $num = mysqli_stmt_num_rows($stmt);
    // step 8:获取结果
    for ($i = 0; $i < $num; $i++) {
        mysqli_stmt_fetch($stmt);
        echo 'name = '.$name.', gender = '.$gender.'<br>';
    }
    // step 9:释放预处理语句的内存
    mysqli_stmt_free_result($stmt);
    // step 10:关闭预处理的语句
    mysqli_stmt_close($stmt);
```





## 常用函数

* rand($min, $max)
  * 获取随机数
* var_dump($expr)
  * 打印一个或多个表达式的结构信息，包括表达式的类型与值
* exit($status)
  * 输出$status并退出当前脚本
* die($expr)
  * 页面输出$expr，并停止运行之后的程序
* empty($var)
  * 如果$var为空，返回true



## 忽略警告

在方法的名称前添加一个@符号

```php
$file = @fopen("data", "w");
```

## $_SERVER['PHP_SELF']问题

* `$_SERVER['PHP_SELF']`代表的是正在执行的脚本

* 将表单的action属性，设置为`$_SERVER['PHP_SELF]`，数据仍旧发送到当前页面

* 但是可以被hacker嵌入JavaScript code

  ```
  <form method="post" action="<?php echo $_SERVER["PHP_SELF"];?>">
  
  // 执行完可能是
  <form method="post" action="test_form.php">
  ```

  但是如果用户输入了另一个url

  ```
  http://www.example.com/test_form.php/%22%3E%3Cscript%3Ealert('hacked')%3C/script%3E
  
  // 执行的结果就变成了
  <form method="post" action="test_form.php/"><script>alert('hacked')</script>
  ```

  这个时候会执行alert函数

* 解决办法：使用htmlspecialchars方法

  * htmlspecialchars会将一些特殊字符转为html entites
  * 比如`< 变成 &lt;`

  ```
  <form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
  ```

  