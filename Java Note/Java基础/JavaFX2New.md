B站： Aimls

练习代码：F:\projects\javafx_Aimls

# JavaFX

## 1、Hello World

> 关于在我的电脑上使用eclipse进行javafx开发时，开发环境的配置
>
> 由于JDK8，9，10内置了javafx，但是11之后又将其剔除
>
> 而本机使用的jdk是11，并且eclispe中通过new Javafx project创建的项目自带的javafx依赖为空，需要手动导入

方法：

1. 对于eclipse

首先将javafx的四个核心依赖base、controls、fxml、graphics导入到User Library（Build path -> Add Library -> User Library -> User Libraries -> New -> 选择需要导入的依赖并为这些依赖添加一个总的名称）

之后可以将User Library中添加好的javafx导入到build path中的classpath中

2. 对于idea

新建一个javafx项目，然后添加为maven项目（右键项目，add framewort support,设置为maven项目，同时修改导入的本地仓库的位置），导入依赖

如果遇到class has wrong version，需要查看自己的jdk的版本

如果遇到`Error:java: error: release version 5 not supported`，需要修改Java compiler中jdk的版本

注：对于以上两种方法，都需要使用一个App类专门来启动Application的子类



## 2、Application的生命周期

Application类在调用launch方法之后，会依次调用init()，start()方法；而在GUI窗口被关闭时，会调用stop()方法

并且init方法在JavaFX-Launcher线程中运行，start和stop在JavaFX-Application Thread线程（UI线程）中运行

注意：所以不要将窗口的布局写在init方法中，需要写在start方法中

1. 创建一个Application类的实例
2. 调用init()方法
3. 调用start(javafx.Stage stage)方法
4. 等待应用结束
   1. 调用Platform.exit()		// 推荐，System.exit(int)也是一种选择
   2. 最后一个窗口关闭，并且Platform.isImplicitExit(boolean)值为true
5. 调用stop()方法          // 不允许主动进行调用，并且该方法执行之后不允许继续使用JavaFX



## 3、Stage窗口

Stage objects must be constructed and modified on the JavaFX Application Thread.

* 显示窗口
  * show()
* 设置窗口的名字
  * setTitle(String title)
* 设置应用最小化的图标（hide the stage and show an icon,但是在移动端和嵌入式系统中不会显示图标）以及窗口左上角的图标
  * getIcons().add(new Image(String url))
  * url可以是resources文件夹下的资源
* 设置最大化
  * setMaximized(boolean)
* 设置最小化
  * setIconified(boolean)
* 关闭窗口
  * close()
* 设置显示宽度、高度
  * setWidth(double px)
  * setHeight(double px)
* 设置最大宽度、高度
  * setMaxHeight(double px)
  * setMaxWidth(double px)
* 设置最小宽度、高度
  * setMinHeight(double px)
  * setMinWidth(double px)
* 设置窗口大小不可改变
  * setResizable(boolean)
* 如果没有设置窗口的宽和高，需要在Stage的show方法调用之后，获得宽高
* 宽和高变化时添加监听器监听
  * heightProperty().addListener(ChangeListener<? extends Number> changeListener)
  * widthProperty().addListener(ChangeListener<? extends Number> changeListener)
  * ChangeListener的changed方法会在宽和高改变时被调用
* 设置全屏（首先需要有scene填充在stage中）
  * setFullScreen(true)
* 设置透明度
  * setOpacity(double)	值从0-1，0透明，1不透明
* 设置X轴Y轴坐标
  * setX(double)
  * setY(double)

* X轴Y轴坐标变化时添加监听器
  * xProperty().addListener(ChangeListener<? extends Number> changeListener)
  * yProperty().addListener(ChangeListener<? extends Number> changeListener)
* 设置当前窗口总是在最前
  * setAlwaysOnTop(boolean)



## 4、Stage的Style，模式

* initStyle(StageStyle)
  * 设置当前Stage的样式
  * StageStyle.DECORARED: 默认样式，白色背景和平台装饰
  * StageStyle.UNDECORATED: 白色背景，无平台样式
  * StageStyle.TRANSPARENT: 透明背景，无平台样式
  * StageStyle.UTILITY: 白色背景，平台最简样式  （只有关闭按钮）
* initOwner(Window):
  * 将某个Window设置为当前stage的Owner（需要在stage展示之前使用）
* initModality(Modality)
  * 设置当前stage的模态，默认是NONE
  * Modality.NONE: 无
  * Modality.APPLICATION_MODAL: 对于当前应用，如果当前stage未关闭，不可以使用其他stage（设置了APPLICATION_MODAL的stage需要最后显示）
  * Modality.WINDOW_MODAL: 对于当前stage的Owner，如果当前stage未关闭，不可以使用该stage的owner



## 5、PlatForm类

* 关闭JavaFX Application
  * static void exit()
* 设置关闭窗口后程序是否停止
  * static setImplicitExit(boolean)
  * 如果为true，所有窗口关闭程序停止
  * 如果为false，窗口关闭后需要调用Platform.exit()之后才可以关闭
* 多任务
  * static runLater(Runnable)
  * 在未来不确定的时间内调用Runnable的run方法，通过一个队列进行存储，依次调用
  * 该方法运行在GUI线程（JavaFX Application Thread），所以如果大量的调用该方法或者将大量的操作放进该方法，会造成程序停止响应（如果要进行多任务，有对应的其他方法，该方法可以用来进行少量的操作）
* 确定系统是否支持某些东西（如3D，fxml，Swing）
  * isSupported(ConditionalFeature feature)



## 6、Scene类

> 由于Stage上面不可以直接添加控件，需要先设置场景Scene，在Scene上可以继续添加控件

```java
Stage stage = new Stage();
Scene scene = new Scene(new Group());
stage.setScene(scene);
```

用构造方法创建scene对象时，需要添加一个可以布局的类（如果添加的不是可以布局的控件，默认填充整个Scene）

* 设置Scene中鼠标的指针样式（对于Button等控件同样适用）

  * setCursor(Cursor)

  * Cursor.HAND ...

  * 也可以自定义图片设置成Cursor，调用Cursor的static cursor(String identifier)

    ```java
    // 这也是Java中加载图片的常用方式
    URL url = getClass().getClassLoader().getResource("icon/icon.png");
    String path = url.toExtentForm();
    
    scene.setCursor(Cursor.cursor(path));
    ```



## 7、小知识点（自动打开某个网页）

```java
// getHostServices是Application的方法
HostServices host = getHostServices();
host.showDocument("www.baidu.com");
```

Application的getHostServices()方法：获取此应用程序的HostServices提供程序。 这提供了获取此应用程序的代码库和文档库以及访问随附网页的能力。

HostServices的showDocument(String uri)方法会自动打开指定uri所对应的网页

## 8、Group类

* 获取所有的子控件
  * getChildren()
  * 如果想要添加子控件: getChildren().add(Node node)或者addAll(Node... node)
  * 注意：
    * 如果没有设置子控件的x轴和y轴坐标，默认是进行覆盖
    * 默认是对子控件进行auto-size的大小设置，可以使用setPrefWidth()等方法进行修改
* 删除子控件
  * getChildren().remove(Object node)
* 清除所有子控件
  * getChildren().clear()
* 设置是否自动管理子控件的大小
  * setAutoSizeChildren(boolean)
  * 如果是false,默认子控件的宽和高都是0px
* 设置透明度
  * setOpacity(double)
  * 子控件也级联受到影响
* 添加监听器，在子控件的数量发生变化时被调用
  * getChildren().addListener(ListChangeListener<T>)，并覆盖onChanged(Change<? extends T> c)



## 9、Button类

* 设置位置

  * setLayoutX(double)
  * setLayoutY(double)

* 设置大小

  * setPrefSize(double, double)
  * setPrefWidth(double, double)
  * setPrefHeight(double, double)

* 设置Button的显示文本

  * 通过构造方法的参数传递
  * setText(String)

* 设置显示文本的字体

  * setFont(Font)

* 设置背景（可以设置背景颜色或者背景图片）

  * setBackground(Background)

  ```java
  // 设置背景颜色
  // Paint是颜色，使用RGB颜色表，如果再加两位数字，可以表示透明度
  // CornerRadii表示四个顶点的圆弧的半径
  // Insets表示按钮内部离边框的间距
  btn1.setBackground(new Background(new BackgroundFill(Paint.valueOf("#FF0033"), new CornerRadii(20),
                  new Insets(10))));
  
  // 设置背景图片
          URL url = getClass().getClassLoader().getResource("basketball.png");
          btn1.setBackground(new Background(new BackgroundImage(new Image(url.toExternalForm()),
                  BackgroundRepeat.NO_REPEAT, BackgroundRepeat.NO_REPEAT, BackgroundPosition.DEFAULT, BackgroundSize.DEFAULT)));
  ```

* 设置边框

  * setBorder(Border)

  ```java
  // 设置边框
          btn1.setBorder(new Border(new BorderStroke(Paint.valueOf("#FF99CC"), BorderStrokeStyle.DASHED, new CornerRadii(10),
                  new BorderWidths(1))));
  ```

* 设置样式
  * setStyle(String)
  * 注意：该方法调用会覆盖前面一个setStyle方法，所以可以把css样式拼接到一个字符串中
  * css样式可以查询网络
* 设置单击响应事件
  
  * setOnAction(EventHandler)



## 10、Button的双击事件和键盘按键事件

* 双击事件
  * addEventHandler(EventType, EventHandler)

```java
// 双击事件
btn.addEventHandler(MouseEvent.MOUSE_CLICKED, new EventHandler<MouseEvent>() {
            @Override
            public void handle(MouseEvent event) {
                MouseButton button = event.getButton();
                System.out.println("当前鼠标的按键是: " + button.name());

                System.out.println("当前鼠标的点击次数是" + event.getClickCount());
                if (button.name().equals(MouseButton.PRIMARY.name()) && event.getClickCount() == 2) {
                    System.out.println("双击事件");
                }
            }
        });
```

* 键盘事件

  * setOnKeyPressed(EventHandler)
  * setOnKeyReleased(EventHandler)
  * setOnKeyTyping(EventHandler)

  ```java
  btn.setOnKeyPressed(new EventHandler<KeyEvent>() {
              @Override
              public void handle(KeyEvent event) {
                  String ch = event.getCode().getName();
                  if (ch.equals(KeyCode.S.getName()))
                      System.out.println(ch + "被按下");
              }
          });
  ```



## 11、Button设置快捷键

```java
// 第一种
// 第一种测试失败
        KeyCombination kc1 = new KeyCodeCombination(KeyCode.I, KeyCombination.CONTROL_DOWN);
        Mnemonic mnemonic1 = new Mnemonic(btn, kc1);
        scene.addMnemonic(mnemonic1);

        // 第二种
        KeyCombination kc2 = new KeyCodeCombination(KeyCode.I, KeyCombination.SHORTCUT_DOWN);
        scene.getAccelerators().put(kc2, new Runnable() {
            @Override
            public void run() {
                // 使用按钮的代码
                // ...
                System.out.println("按钮被使用");
            }
        });
```



## 12、输入框，密码框，标签

### 12.1 TextField

和Button一样，作为Node的子类，拥有setText(String)，setFont(Font)等方法

* 设置鼠标悬浮时的提示文本

  * setTooltip(Tooltip)

  ```java
  // 设置鼠标悬浮时的提示文本
          Tooltip tTip = new Tooltip();
          tTip.setText("请输入账号");
          tTip.setFont(Font.font(10));
          tf.setTooltip(tTip);
  ```

* 设置失去焦点时文本框的提示文字

  * setPromptText(String)

* 设置失去焦点

  * setFocusTraversable(boolean)

* 添加文本的监听器

  * textProperty().addListener(ChangeListener)

  ```java
  // 使得输入的文本不可以超过六个
  tf.textProperty().addListener(new ChangeListener<String>() {
      @Override
      public void changed(ObservableValue<? extends String> observable, String oldValue, String newValue) {
          if (newValue.length() > 6) {
              tf.setText(oldValue);
          }
      }
  });
  ```

* 添加选中的文本的监听器
  
  * selectedTextProperty().addListener(ChangeListener)
* 单击事件
  
  * setOnMouseClicked(EventHandler)

### 12.2 PasswordField

用法与TextField基本相同

### 12.3 Label

可以添加单击事件setOnMouseClicked(EventHandler)



## 13、AnchorPane

采用绝对定位

与Group不同的是，AnchorPane可以设置样式setStyle()		（see Region类的样式）

* 设置距离AnchorPane的edge的距离（如果AnchorPane设置了内边距setPadding(Insets)，需要将其考虑进去）
  * static setTopArchor(Node, Double)
  * static setLeftArchor(Node, Double)
  * static setRightArchor(Node, Double)
  * static setBottom(Node, Double)
  * 这四个方法的Double是包装类对象，不允许传入int

注：如果同时设置了两个相反方向的anchor，AnchorPane会resize控件

* 设置单击响应
  * setOnMouseClicked()			// Node的方法
* 添加控件
  * getChildren().add(Node)





## 注意（对于控件的宽和高）

在Stage的show方法调用之前，所有的控件如果没有设置宽和高，调用getWidth()和getHeight()方法的结果都是0



## 14、HBox和VBox

* 设置内边距
  * setPadding(Insets)
* 设置子控件的外边距
  * setMargin(Node, Insets)
* 设置子控件的间距
  * setSpacing(int)
* 设置对齐方式
  * setAlignment(Node, Pos)



## 15、setManaged, setVisible. setOpacity的区别

* setManaged是将子控件脱离父控件的管理
* setVisible是将控件设置为不可见，但仍在父控件的管理之下
* setOpacity是设置透明度



## 16、BordderPane

设置内边距外边距与AnchorPane相同

* 添加组件
  * setTop
  * setBottom
  * setLeft
  * setRight



## 17、FlowPane

设置内边距外边距与AnchorPane相同

* 设置水平间距
  * setHgap(int)
* 设置垂直间距
  * setVgap(int)
* 设置排列方式
  * setOrientation(Orientation)
  * Orientation.HORIZONTAL  水平布局
  * Orientation.VERTICAL  垂直布局



## 18、GridPane

设置内边距外边距与AnchorPane相同

* 添加控件
  * add(Node, int column, int row)
  * setConstraints(Node, int column, int row); getChildren().add(Node)
  * setRowIndex(Node, int); setColumnIndex(Node, int); getChildren().add(Node)
* 设置水平间距
  * setHgap(int)
* 设置垂直间距
  * setVgap(int)



## 19、StackPane

与其他Pane类似，先添加的东西放在最下面



## 20、TextFlow



## 21、TilePane

每个控件占据的范围是相同的

可以设置单个Node的对齐方式



## 22、DialogPane

* 设置标题文字
  * setHeaderText(String text)
* 设置内容文字
  * setContentText(String text)
* 添加对话框的按钮
  * getButtonTypes().add(ButtonType.APPLY)
  * APPLY/CLOSE/CANCEL/PREVIOUS/...
* 获取对话框的按钮
  * Node lookupButton(ButtonType.APPLY)
* 设置图标
  * setGraphics(ImageView)
* 设置扩展文字
  * setExpandableContent(String text)
* 设置扩展文字的默认打开或关闭
  * setExpanded(boolean)

### 22.1 JavaFX多任务之ScheduledService

> ScheduledService<V>是一个接口，泛型V是需要处理的值
>
> 实现此类需要实现Task<V> createTask方法，返回一个任务

>Task<V>是一个接口，需要实现V call()方法，即任务调用的方法，返回的值会作为updateValue(V)的参数
>
>因为call方法运行没有处于JavaFX Application线程，所以无法更新UI，需要使用updateValue方法

* 设置延迟多少毫秒之后运行
  * setDelay(Duration.millis(millis))
* 设置运行周期
  * setPeriod(Duration.millis(millis))
* 启动多任务
  * start()

```java
package sample;

import javafx.application.Application;
import javafx.concurrent.ScheduledService;
import javafx.concurrent.Task;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Node;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ButtonType;
import javafx.scene.control.DialogPane;
import javafx.scene.layout.TilePane;
import javafx.stage.Modality;
import javafx.stage.Stage;
import javafx.stage.StageStyle;
import javafx.util.Duration;

import java.text.SimpleDateFormat;
import java.util.Date;

public class L22DialogPane_ScheduledService extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception {

        Button btn = new Button("点击弹出对话框");

        btn.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent event) {
                Stage dialogStage = new Stage();
                dialogStage.setWidth(150);
                DialogPane dialog = new DialogPane();

                dialog.setHeaderText("当前时间");

                // 多任务
                ScheduledService<String> service = new ScheduledService<String>() {

                    SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");

                    @Override
                    protected Task<String> createTask() {
                        return new Task<String>() {
                            @Override
                            protected String call() throws Exception {
                                Date date = new Date();
                                return sdf.format(date);
                            }

                            @Override
                            protected void updateValue(String value) {
                                dialog.setContentText(value);
                            }
                        };

                    }


                };
                // 0秒后允许多任务
                service.setDelay(Duration.millis(0));
                // 运行周期是1秒
                service.setPeriod(Duration.millis(1000));
                service.start();

                // 增加关闭按钮
                dialog.getButtonTypes().add(ButtonType.CLOSE);
                Button close = (Button) dialog.lookupButton(ButtonType.CLOSE);
                close.setOnAction(new EventHandler<ActionEvent>() {
                    @Override
                    public void handle(ActionEvent event) {
                        service.cancel();
                        dialogStage.close();
                    }
                });

                // 设置当前页面为模态
                dialogStage.initOwner(primaryStage);
                dialogStage.initModality(Modality.WINDOW_MODAL);
                // 取消最小化
                dialogStage.initStyle(StageStyle.UTILITY);
                dialogStage.setResizable(false);

                Scene s1 = new Scene(dialog);
                dialogStage.setScene(s1);
                dialogStage.setTitle("Time");
                dialogStage.show();
            }
        });

        TilePane pane = new TilePane();
        pane.getChildren().add(btn);
        Scene scene = new Scene(pane);
        primaryStage.setScene(scene);
        primaryStage.setWidth(400);
        primaryStage.setHeight(400);
        primaryStage.setTitle("主页面");
        primaryStage.show();
    }


}

```



## 23、JDK11中使用JavaFX



## 24、简单的登录窗口

### 24.1 闪烁动画FadeTransistion

* 设置持续时间
  * setDuration(Duration)
* 设置使用动画的节点
  * setNode(Node)
* 设置开始时间
  * setFromValue(double)
* 设置结束时间
  * setToValue(double)
* 启动动画
  * play()



## 25、Hyperlink的使用

超链接被点击时，isVisited方法返回true，并且产生一个单击响应时间

```java
Hyperlink link = new Hyperlink("www.baidu.com");
link.setOnAction(new EventHandler<ActionEvent>() {
    HostServices host = getHostServices();
    host.showDocument(link.getText());
});
```



## 26、MenuBar，Menu，MenuItem

* MenuBar添加Menu
  * menubar.getMenus().add(Menu)
* Menu添加MenuItem
  * menu.getItems().add(MenuItem)
* 给MenuItem设置快捷键，触发单击响应事件
  * item.setAccelerator(KeyCombination.valueOf("ctrl+alt+k"));
* Menu的显示菜单和隐藏菜单事件
  * setOnShowing(EventHandler)
  * setOnHidden(EventHandler)
* 设置Menu或者MenuItem禁用
  * setDisabled(false)



## 27、SeparatorMenuItem，RadioMenuItem，CheckMenuItem

菜单中的分割线SeparatorMenuItem

单选按钮RadioMenuItem（需要配合ToggleGroup一起使用）

```java
ToggleGroup tg = new ToggleGroup();
RadioMenuItem rmi1 = new RadioMenuItem("按钮1");
RadioMenuItem rmi2 = new RadioMenuItem("按钮2");
// 将多个单选按钮关联起来
rmi1.setToggleGroup(tg);
rmi2.setToggleGroup(tg);
```

* 设置默认选中
  * setSelected(true)

复选按钮CheckMenuItem



## 28、CustomMenuItem，MenuButton，SplitMenuButton，ContextMenu

* CustomMenuItem
  * 属于菜单项的一种，可以自定义其中的内容
  * setContent(Node)  将内容设置进入
* MenuButton
  * 不属于菜单项的一种，但作用和菜单一样（类似下拉菜单）
  * 可以直接添加到页面上，并且可以添加MenuItem

* SplitMenuButton
  * 用法和MenuButton相同
  * 区别是SplitMenuButton本身还可以设置单击响应事件
  * 不能直接通过构造方法设置文本，需要使用setText方法
* ContextMenu
  * 在控件上点击右键显示的菜单
  * 给控件设置右键菜单，需要调用其setContextMenu(ContextMenu)方法
  * 还可以设置响应事件setOnContextMenuRequest方法，会在右键的时候触发

```java
package sample;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.AnchorPane;
import javafx.stage.Stage;

public class L28ContentMenuItem_MenuButton_SplitMenuButton_ContextMenu extends Application {
    @Override
    public void start(Stage primaryStage) throws Exception {
        MenuBar menuBar = new MenuBar();
        Menu file = new Menu("File");
        Menu edit = new Menu("Edit");

        menuBar.getMenus().addAll(file, edit);

        MenuItem fileOpen = new MenuItem("open");
        MenuItem fileClose = new MenuItem("close");
        file.getItems().addAll(fileOpen, fileClose);

        // CustomMenuItem
        CustomMenuItem item1 = new CustomMenuItem();
        Button choose = new Button("choose");
        item1.setContent(choose);
        SeparatorMenuItem isolation = new SeparatorMenuItem();
        edit.getItems().addAll(item1, isolation);

        // MenuButton
        MenuButton menuButton = new MenuButton("选择你的国籍");
        MenuItem[] items = new MenuItem[3];
        items[0] = new MenuItem("中国");
        items[1] = new MenuItem("England");
        items[2] = new MenuItem("Deutschland");
        for (MenuItem item : items) {
            menuButton.getItems().add(item);
            item.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    MenuItem btn = (MenuItem) event.getSource();
                    menuButton.setText(btn.getText());
                }
            });
        }


        // splitMenuButton
        SplitMenuButton splitMenuButton = new SplitMenuButton();
        splitMenuButton.setText("选择你喜欢的明星");
        MenuItem[] items2 = new MenuItem[3];
        items2[0] = new MenuItem("石原里美");
        items2[1] = new MenuItem("周杰伦");
        items2[2] = new MenuItem("王嘉尔");
        for (MenuItem item : items2) {
            splitMenuButton.getItems().add(item);
            item.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    MenuItem btn = (MenuItem) event.getSource();
                    splitMenuButton.setText(btn.getText());
                }
            });
        }
        // 单击响应
        splitMenuButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent event) {
                System.out.println(splitMenuButton.getText());
            }
        });

        // ContextMenu
        ContextMenu contextMenu = new ContextMenu();
        contextMenu.getItems().addAll(new MenuItem("a"),
                new MenuItem("b"),
                new MenuItem("c"),
                new MenuItem("d"));
        Label label = new Label("请右键单击此控件");
        label.setContextMenu(contextMenu);

        AnchorPane root = new AnchorPane();
        root.getChildren().add(menuBar);
        root.getChildren().add(menuButton);
        root.getChildren().add(splitMenuButton);
        root.getChildren().add(label);
        AnchorPane.setTopAnchor(menuButton, 100.0);
        AnchorPane.setLeftAnchor(menuButton, 100.0);
        AnchorPane.setTopAnchor(splitMenuButton, 100.0);
        AnchorPane.setLeftAnchor(splitMenuButton, 230.0);
        AnchorPane.setTopAnchor(label, 200.0);
        AnchorPane.setLeftAnchor(label, 100.0);
        
        Scene scene = new Scene(root);
        primaryStage.setScene(scene);
        primaryStage.setTitle("菜单");
        primaryStage.setWidth(480);
        primaryStage.setHeight(360);
        primaryStage.show();


        fileOpen.setOnAction(e -> {
            System.out.println("文件已打开");
        });

        fileClose.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent event) {
                System.out.println("Close the file.");
            }
        });
    }
}
```



## 29、Accordion和TitledPane可折叠组件

TitledPane是一个可折叠组件，有一个title，单击会出现或隐藏一个控件

* 构造方法
  * TitledPane(String)
  * TitledPane(String, Node)
* 设置显示的控件
  * setContent(Node)
* 设置点击的时候，显示或隐藏控件是否需要动画
  * setAnimated(boolean)
* 设置是否禁用组件点击进行收放子控件
  * setCollapsible(boolean)
* 设置是否显示子控件
  * setExpand(boolean)



## 112、Fxml入门

```fxml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<AnchorPane xmlns="http://javafx.com/javafx"
            xmlns:fx="http://javafx.com/fxml"
            prefHeight="400.0" prefWidth="600.0">
    <children>
        <Button id="welcome" text="欢迎詹国志~~" prefWidth="150" prefHeight="100">
            <AnchorPane.topAnchor>100.0</AnchorPane.topAnchor>
            <AnchorPane.leftAnchor>100.0</AnchorPane.leftAnchor>
        </Button>
    </children>
</AnchorPane>

```

```java
FXMLLoader fxmlLoader = new FXMLLoader();
        URL url = fxmlLoader.getClassLoader().getResource("fxml/L112Fxml.fxml");
        // 设置加载器加载资源
        fxmlLoader.setLocation(url);
        // 加载fxml,返回根节点
        AnchorPane root = (AnchorPane)fxmlLoader.load();

        // 得到子控件——按钮
        Button btn = (Button) root.lookup("#welcome");
        btn.setOnAction(e -> System.out.println("王嘉尔真的好帅"));

        Scene scene = new Scene(root);
        primaryStage.setScene(scene);
        primaryStage.setTitle("FXML");
        primaryStage.show();
```



## 113、FX控制器controller

* 在页面中添加controller
  * 在fxml的根节点中添加`fx:controller=<controller的全限定类名>`
  * Controller类中的initialize方法添加@FXML注解

* 在controller中获取页面的控件
  * 首先，先对要获得的控件设置`fx:id`属性
  * 在controller类中，定义相同类型的成员变量，并且变量名和页面控件的id一致
  * 在controller类中，给该变量添加@FXML注解

* 给页面的控件设置单击响应事件
  * 首先，给fxml页面中的控件设置`onAction="#name"`，
  * 然后在controller类中，增加一个与name名称相同的方法，并添加@FXML注解

```java
package controller;

import javafx.fxml.FXML;
import javafx.scene.control.Button;

public class L113Controller {

    @FXML
    private Button welcomeBtn;

    @FXML
    private void initialize() {
        System.out.println("页面加载完毕");
        System.out.println(welcomeBtn.getText());
    }
    
    @FXML
    private void click() {
        System.out.println("按钮的单击响应事件");
    }
}
```

```fxml
<?xml version="1.0" encoding="UTF-8"?>


<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<AnchorPane xmlns="http://javafx.com/javafx"
            xmlns:fx="http://javafx.com/fxml"
            fx:controller="controller.L113Controller"
            prefHeight="400.0" prefWidth="600.0">

    <children>
        <Button fx:id="welcomeBtn" text="欢迎詹国志~~" prefWidth="150" prefHeight="100"  onAction="#click">
            <AnchorPane.topAnchor>100.0</AnchorPane.topAnchor>
            <AnchorPane.leftAnchor>100.0</AnchorPane.leftAnchor>
        </Button>

    </children>

</AnchorPane>
```

