# Swing
## 1 Java Swing API

默认不显示JFrame，需要调用setVisible(true);

默认关闭窗口之后 程序不会结束

如果要设置关闭窗口，程序结束  需要调用setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

设置默认居中setLocationRelativeTo(null);

添加MenuBar 调用setJMenuBar(new JMenuBar());

JMenuBar或者JMenu添加MenuItem，使用add(JMenuItem)

### 1.1 标签JLabel
* setText() : 设置文本
* getText() : 获取文本
* setForeground() :设置文本颜色
* setToolTipText() :设置工具提示
* setOpaque(boolean) :设置背景是否为不透明
* setBackground(Color) :设置背景颜色
* setForeground(Color) :设置字体颜色
* setPreferredSize(Dimension) :设置最佳尺寸
* setHorizontalAlignment(SwingConstants.CENTER) :设置水平对齐
### 1.2 文本框JTextField
* new JTextField(16) : 16表示文本框占据的列数
* setText() : 设置文本
* getText() : 获取文本
* setFont() : 设置字体
* setEnabled(boolean): true时文本框可用，false时文本框禁用
* setEditable(boolean) :true时文本框可编辑，默认为true
* ### 1.3 文本框JTextArea
* append(String str): 将str添加到当前文本框的显示文字的最后
### 1.4 JOptionPane
* 用来显示alert/errors
* static showMessageDialog(parentComponent, String message) :显示message的消息提示框
* static int showConfirmDialog(Component parentComponent, Object message) :显示确认框，选择是返回0，否返回1，取消返回2
* public static int showConfirmDialog(Component parentComponent, Object message, String title, int optionType, int messageType) :显示确认框，optionType是需要显示的选项， messageType是消息类型
### 1.5 复选框JCheckBox
* isSelected() : 获取选中状态
* setSelected() :设置选中状态
* setText() : 选项文字
* 事件 addActionListener
### 1.6 下拉列表JComboBox
1. 创建JComboBox
```
    // 显示的时候根据泛型类型的toString方法进行显示
    JComboBox<String> colorList = new JComboBox<>();
```
2. 添加数据项
```
    colorList.addItem("red");
    colorList.addItem("blue");
    colorList.addItem("green");
```
3. 按索引访问
    * getSelectedIndex() :获取选中项的索引
    * setSelectedIndex() :设置选中项
    * remove(index) :按索引删除
4. 按数据项访问
    * getSelectedItem()
    * setSelectedItem()
### 1.7 窗口JFrame
* dispose() : 消除当前页面
* setResizable(boolean) :用户是否可以调整大小
* setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE) :设置关闭时退出程序
### 1.8 面板JPanel
* 默认是FlowLayout
### 1.9颜色选择器JColorChooser
* static createDialog(Component c, String title, boolean modal, JColorChooser chooserPane, ActionListener okListener, ActionListener cancelListener) :创建并返回一个包含指定的 ColorChooser窗格的新对话框以及“确定”，“取消”和“重置”按钮。 
### 1.10文件选择器JFileChooser
* File getSelectedFile() :返回选中的文件
* showOpenDialog(Component parent) : 显示一个打开文件的对话框，返回值为CANCEL_OPTION（取消操作），APPROVE_OPTION（确认操作），ERROR_OPTION（发生错误或对话框关闭）
* showSaveDialog
### 1.11滚动条面板JScrollPane
* JScrollPane() :创建一个 JScrollPane ，它显示指定组件的内容，当组件的内容大于视图时，水平和垂直滚动条都会显示。
### 1.12边框Border
* 在javaw.swing.border.*下
* Border是一个接口
* BorderFactory是一个工具类，用来创建各种边框
```
    Border border = BorderFactory.createLineBorder(Color.BLUE);
    root.setBorder(border);
```
* 几大类型的边框(具体使用时可以参照[官方文档](https://docs.oracle.com/javase/tutorial/uiswing/components/border.html))
    * 简单边框Simple
    * 特种边框Matte
        * 分别指定四个方位的粗细: BorderFactory.createMatteBorder(top, left, bottom, right, Color)
    * 带标题的边框Titled
        * BorderFactory.setTitledBorder("标题")
    * 复合边框Compound
        * BorderFactory.createCompoundBorder(Border outer, Border inner)
* 给界面设置边框的方法： 创建一个JPanel，然后设置边框，再用setContentPane()设置到界面上
* 设置边距
    * 内边距padding
    * 外边距margin
    * 通过BorderFactory.createEmptyBorder(3, 3, 3, 3)设置空白粗边框设置边距，再可以通过复合边框实现内边距和外边距
### 1.13JButton
* setContentAreaFilled(boolean): 如果为true将按钮画出内容区域。默认为true
* setFoucsPainted(boolean): 如果为true焦点状态可见
## 2 布局管理器LayoutManager
### 2.1 流式布局管理器FlowLayout
* 控件.setPreferredSize() :设置控件的显示最佳宽度和高度
* FlowLayou(int align, int hgap, int vgap)
    * align :设置对齐，CENTER、LEFT、RIGHT
    * hgap :设置控件之间的行间距
    * vgap :设置控件之间的垂直间距
### 2.2 边界布局管理器BorderLayout
* PAGE_START :上边界
* PAGE_END :下边界
* LINE_START :左边界
* LINE_END :右边界
* CENTER :中间
### 2.3 卡片式布局CardLayout
* 窗体的方法add(component, string) : 加入控件
* cardLayout的方法show(component, string) :显示名为string的控件到component上
## 3 自定义布局器
### 3.1 窗口坐标
* 左上角为(0, 0)，横坐标向右增长，纵坐标向下增长，单位是像素
* 取消布局器，子控件默认不显示
* frame窗口的标题栏也占据了大小
* 通过setBounds直接设置子控件的大小及位置
### 3.2 布局管理器LayouManager
* 负责对子控件的布局，当窗口变化的时候，动态调用整个子控件的位置和大小
* 布局器的运行机制
    * 给容器设置一个布局器 root.setLayout(layoutManager)
    * 当容器大小改变的时候，自动调用布局器重新布局 layoutManager.layoutContainer(...)  // Swing内部自动调用
* 自定义一个布局管理器实现LayoutManager接口
    * 实现其layoutContainer方法，在内部对控件动态布局
## 4 图标
### 4.1 图标的使用
* 图标Icon（接口），ImageIcon（实现类）
* 默认的，JLabel，JButton都可以设置图标
* 支持的格式jpg/png/jpeg格式
```
    JLabel label = new JLabel();
    URL url = getClass().getResource("images/icon.png");
    Icon icon = new ImageIcon(url);
    label.setIcon(icon);
```
* 图标按照原来的尺寸显示
