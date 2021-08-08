# XML

## 一. XML介绍

XML（Extensible Markup Language 可扩展标记语言）

标签是自定义的，该标签负责数据存储（HTML负责数据展示）

* 功能
  * 配置文件
  * 在网络中传输
* 语法严格

## 二. 语法

1. xml第一行必须定义为文档声明 XML declaration

```xml
<?xml version="1.0" encoding="utf-8" ?>
```

2. xml文档中有且只有一个根标签root element
3. 标签必须正确关闭
4. XML标签对大小写敏感 case-sensitive，即Letter和letter是两个标签
5. XML用LF（Line Feed）`\n`存储换行

## 三、组成部分

1. 文档声明

   1. 格式：`<?xml 属性列表 ?>`
   2. 属性列表
      * version ：版本号	// 必须属性（1.1版本不向下兼容）
      * encoding ：编码格式，默认值是IS0-8859-1，告知解析引擎使用该种编码格式去解析xml
      * standalone ：是否依赖于其他文件，取值为“yes”和“no“

2. 指令 ：结合css

   1. 现在基本上不再使用，一开始为了展示数据，但是由于xml的功能变成了存储数据，所以用处不大
   2. 格式 ：`<?xml-stylesheet type="text/css" href="a.css" ?>`

3. 标签

   1. 命名规则
      * 名称可以包含字母、数字、以及其它字符
      * 名称不能以数字或标点符号开始
      * 名称不能以字母xml（或者XML、Xml等等）开始
      * 名称不能包含空格
      
      Remark ： XML文档一般会对应一个数据库，其中的文档会对应xml中的元素。推荐使用数据库的命名规则来命名元素
      
   2. 可以把一个标签看作是元素对象Element

      * 从开始标签（包括）到结束标签（包括）的部分
      * 元素可以包含元素、文本或者两者的混合物
      * 元素可以拥有属性

4. 属性

   1. id属性值唯一

5. 文本

   1. CDATA区：将该区域的数据原样展示

      * 格式 ：

        ```xml
        <![CDATA[
        	if (a < b && a > c) {}
        ]]>
        ```

## 四、约束

规定xml文档的书写规则

* 作为框架的使用者（程序员） ：
  1. 能够在xml中引入约束文档
  2. 能够简单的读懂约束文档

* 分类 ：
  1. DTD ： 一种简单的约束技术
  2. Schema ： 一种复杂的约束技术

### 4.1 DTD

* 外部约束student.dtd

```dtd
<!ELEMENT students (student*) >
<!ELEMENT student (name,age,sex) >
<!ELEMENT name (#PCDATA) >
<!ElEMENT age (#PCDATA) >
<!ElEMENT sex (#PCDATA) >
<!ATTLIST student number ID #REQUIRED >
```

解读：

第一行定义了一个students标签，其子标签是student，并且子标签允许出现多次

第二行定义了student标签，其子标签是name，age，sex，并且按照该顺序出现一次

第三行第四行第五行定义了三个标签name，age，sex，其数据类型是字符串

第六行定义了student标签的属性number 是ID类型，即number唯一，并且是必须显示出现的

* 引入dtd文件到xml文档中

  * 内部dtd：将约束规则定义在xml

    ```xml
    <!DOCTYPE students [
    	<!ELEMENTS students (student+)>
    	<!ELEMENT student (name,age,sex) >
        <!ELEMENT name (#PCDATA) >
        <!ElEMENT age (#PCDATA) >
        <!ElEMENT sex (#PCDATA) >
        <!ATTLIST student number ID #REQUIRED >
    ]>
    ```

    

  * 外部dtd：将约束规则定义在外部的dtd中（如：student.dtd）

    * 本地 ：`<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">`
    * 网络 ：`<!DOCTYPE 根标签名 PUBLIC "dtd文件的名称" "dtd的URL">`

* student.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE students SYSTEM "student.dtd">

<students>
	<student number="s001">
    	<name>zhangsan</name>
        <age>18</age>
        <sex>male</sex>
    </student>
</students>
```

### 4.2 Schema

DTD约束不可以限制标签的文本的一些规则

* student.xsd

```xml
<?xml version="1.0" ?>
<xsd:schema xmlns="http://www.w3school.com.cn/xml"
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"
            targetNamespace="http://www.w3school.com.cn/xml" 
            elementFormDefault="qualified">
    <!--定义一个element 名称为students  类型是studentsType-->
    <xsd:element name="students" type="studentsType"/>
    <xsd:complexType name="studentsType">
        <!--xsd:sequence中定义的element之后要按照顺序出现-->
    	<xsd:sequence>
        	<xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>
    <xsd:complextType name="studentType">
    	<xsd:sequence>
        	<xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType"/>
            <xsd:element name="sex" type="sexType"/>
        </xsd:sequence>
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complextType>
    <xsd:simpleType name="sexType">
    	<xsd:restriction base="xsd:string">
            <!--定义sexType的值为male或female-->
        	<xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="ageType">
    	<xsd:restriction base="xsd:integer">
            <!--定义ageType的范围从0-256-->
        	<xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="numberType">
        <!--numberType的基础类型是string，格式为s后面加上三个数字-->
    	<xsd:restriction base="xsd:string">
        	<xsd:pattern value="s\d{3}"
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```

* 引入schema文件
  1. 填写xml文档的根标签
  2. 引入xsi前缀  `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`
  3. 引入xsd文件的命名空间  `xsi:schemaLocation="http://www.w3school.com.cn/xml student.xsd"`
  4. 为每一个xsd约束声明一个前缀，作为标识  `xmlns:a="http://www.w3school.com.cn/xml"`
* student.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<a:students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.w3school.com.cn/xml student.xsd"
          xmlns:a="http://www.w3school.com.cn/xml">
	<a:student number="s001">
    	<a:name>zhangsan</a:name>
        <a:age>11</a:age>
        <a:sex>female</a:sex>
    </a:student>
</a:students>
```

## 五、操作XML文档

### 5.1 解析（读取）

将文档中的数据读取到内存中

* 解析xml的方式

  1. DOM ：将标记语言文档一次性加载进内存，在内存中形成一颗dom树
     * 优点 ： 操作方便，可以对文档进行CRUD的所有操作
     * 缺点： 比较消耗内存

  2. SAX ： 逐行读取，基于事件驱动的
     * 优点 ： 不占内存（读取一行释放一行）
     * 缺点 ： 只能读取

  总结：服务器端使用DOM，移动端（如安卓）使用SAX

* xml常见的解析器
  1. JAXP ： sun公司提供的解析器，支持dom和sax两种思想（性能差）
  2. DOM4J ： 一款非常优秀的解析器
  3. Jsoup ： 一款Java 的HTML解析器，可以直接解析某个URL地址、HTML文本内容。提供了一套非常省力的API，可以通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据
  4. PULL ： Android操作系统内置的解析器，sax方式

#### 5.1.1 Jsoup使用

* 快速入门

  * 步骤：

    1. 导入jar包

    ```xml
    <dependency>
        <groupId>org.jsoup</groupId>
        <artifactId>jsoup</artifactId>
        <version>1.13.1</version>
    </dependency>
    ```

    2. 获取Document对象

    3. 获取对应的标签Element对象

    4. 获取数据

```java
// 1.获取Document对象 根据xml文档获取
// 1.1 获取student.xml的path
String path = JsoupDemo1.class.getClassLoader().getResource("student.xml").getPath();
// 1.2 解析文档，加载进内存，获取dom树--->Document
Document document = Jsoup.parse(new File(path), "utf-8");
// 2. 获取标签名为name的Element对象
// 根据标签的名字进行查询
// Elements的类型是ArrayList<Element>
Elements name = document.getElementsByTag("name");
System.out.println(name.size());
System.out.println(name.get(0).text());  
```

* 对象的使用
  1. Jsoup ： 工具类，可以解析html或xml文档，返回Document
     * parse ： 解析html或xml文档，返回Document
       * parse(File in, String charsetName) : 解析xml或html文件
       * parse(URL url, int timeoutMillis) : 通过网络路径获取指定的html或xml文档对象
  2. Document （extends Element）： 文档对象，代表内存中的DOM树
     * 获取Element对象
       * Element getElementById(String id) : 根据id属性值获取唯一的element对象
       * Elements getElementsByTag(String tagName) : 根据标签名称获取元素对象集合
       * Elements getElementsByAttribute(String key) : 根据属性名称key获取元素对象集合
       * Elements getElementsByAttributeValue(String key, String value) : 根据对应的属性名和属性值获取元素对象集合
  3. ELements ： 元素Element对象的集合，可以当作`ArrayList<Element>`来使用
  4. Element （extends Node） ： 元素对象
     * 获取Element对象
       * Element getElementById(String id) : 根据id属性值获取唯一的element对象
       * Elements getElementsByTag(String tagName) : 根据标签名称获取元素对象集合
       * Elements getElementsByAttribute(String key) : 根据属性名称key获取元素对象集合
       * Elements getElementsByAttributeValue(String key, String value) : 根据对应的属性名和属性值获取元素对象集合
     * 获取属性值
       * String attr(String key) : 根据属性名称获取属性值
     * 获取文本内容
       * String text() : 根据属性名称获取属性值
       * String html() : 获取标签体的所有内容（包括子标签的字符串内容）
  5. Node ： 节点对象

#### 5.1.2 快捷查询方式（Jsoup续）

1. selector选择器

   * 使用的方法 ： Elements select(String cssQuery)
     * 语法 ： 参考Selector类中定义的语法
     * 主要参考css的选择器

2. XPath ： XPATH即为XML路径语言，它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言

   * XPath语法定义了如何查询指定的标签或标签组

   ```java
   // 1. 导入JsoupXpath的jar包
   // 2. 根据document对象，创建JXDocument对象
   JXDocument jxDocument = new JXDocument(document);
   
   // 3.1 查询所有student标签
   // "//student"表示xml文档中的所有的student标签 
   List<JXNode> jxNodes = jxDocument.selN("//student");
   
   // 3.2 查询所有student标签下的name标签
   // "//student/name"表示xml文档中的所有的student标签下的所有的name子标签
   List<JXNode> jxNodes2 = jxDocument.selN("//student/name");
   
   // 3.3 查询student标签下带有id属性的name标签
   List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id]");
   
   // 3.3 查询student标签下带有id属性的name标签，并且id属性为“zhangsan”
   List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id='zhangsan']");
   ```

   



### 5.2 写入

将内存中的数据保存到xml文档中，持久化的存储

## 六、SAX

> Simple API for XML
>
> 事件驱动，边读边解析
>
> 优点：无需将整个文档加载到内存中，适合大的文档解析

解析步骤：

1. 创建解析工厂

   1. 通过new Instance方法获取

   ```java
   // 创建解析器工厂
   SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
   ```

2. 创建解析器

   ```java
   SAXParser saxParser = saxParserFactory.newSAXParser();
   ```

3. 执行parser方法，传入两个参数：xml路径、事件处理器

   ```java
   // 第一个参数可以是File对象，也可以是文件的路径
   // 第二个参数是DefaultHandler类型
   saxParser.parser("person.xml", new MyDefaultHandler());
   ```



### Example

#### MySAXParser.java

```java
package cn.zhanguozhi.xml.sax;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.File;
import java.io.IOException;

public class MySAXParser {
    public static void main(String[] args) throws ParserConfigurationException, SAXException, IOException {
        // 创建解析器工厂
        SAXParserFactory parserFactory = SAXParserFactory.newInstance();
        // 创建解析器
        SAXParser saxParser = parserFactory.newSAXParser();
        // 解析xml
        String file = MySAXParser.class.getClassLoader().getResource("information_systems/exer1/course.xml").getPath();
        saxParser.parse(new File(file), new MyDefaultHandler());
    }


}
```

##### MyDefaultHandler.java

```java
class MyDefaultHandler extends DefaultHandler {

    /**
     * 解析遇到开始标签时会调用本方法
     * @param uri 名称空间
     * @param localName 本地名称
     * @param qName 标签名称（限定名称）
     * @param attributes 标签中的属性（内有键和值）
     * @throws SAXException
     */
    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        String startEle = "<" + qName + " ";
        for (int i = 0; i < attributes.getLength(); i++) {
            startEle += attributes.getQName(i) + "=\"" + attributes.getValue(i) + "\" ";
        }
        startEle += ">";
        System.out.println(startEle);
    }

    /**
     * 解析遇到文本内容时调用本方法
     * @param ch 文本的字符数组形式
     * @param start
     * @param length 文本的长度
     * @throws SAXException
     */
    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        System.out.println(new String(ch, start, length));
    }

    /**
     * 解析遇到结束标签时会调用本方法
     * @param uri
     * @param localName
     * @param qName
     * @throws SAXException
     */
    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        System.out.println("</" + qName + ">");
    }
}
```



## 七、DOM

> Document Object Model 文档对象模型
>
> 对于XML应用开发来说，DOM就是一个对象化的XML数据接口，一个与语言无关、与平台无关的标准接口规范

在应用程序中，基于DOM的XML分析器将一个XML文档转换成一个对象模型的集合（通常称DOM树），应用程序正是通过对这个对象模型的操作，来实现对XML文档数据的操作。

通过DOM接口，应用程序可以在任何时候访问XML文档中的任何一部分数据。

因此，这种利用DOM接口的机制也被称为**随机访问机制**

* DOM接口提供了一种通过分层对象模型来访问XML文档信息的方式，这些分层对象模型一句XML的文档结构形成了一颗节点树

* 缺点1：消耗内存多

  * DOM将整个DOM树放进了内存中

* 缺点2：对机器的性能要求高

  * 对结构复杂的树的遍历非常耗时

* DOM的功能

  ```txt
  DOM定义了HTML和XML文档的逻辑结构，给出了一种访问和处理HTML文档和XML文档的方法。
  利用DOM，程序开发人员可以动态地创建文档，遍历文档结构，添加、删除、修改文档内容，改变文档的显示方式等
  ```

### 7.1 DOM接口

* 四个基本的接口

  * Document
    * getXmlEncoding()
    * getXmlVersion()
    * getXmlStandalone()
    * getDocumentElement()  获得根元素节点
  * Element createElement(String tagName)
    * Text createTextNode(String data)
    * Attr createAttribute(String name)
    * NodeList getElementsByTagName(String tagName)
  
```
  Document接口代表了整个XML/HTML文档，因此，它是整颗文档树的根，提供了对文档中的数据进行访问和操作的入口
	由于元素、文本节点、注释、处理指令等都不能脱离文档的上下文关系而独立存在，所以在Document接口提供了创建其他节点对象的方法，通过该方法创建的节点对象都有一个ownerDocument属性，用来表明当前节点是由谁创建的，并且节点和Document之间的关系
```

  * Node
  
  * getFirstChild()
    * getLastChild()
    * getChildNodes()
      * 节点之间的空格也是节点：文本节点
  * hasChildNodes()  是否有子节点
    * getNodeName()  获得节点的name
    * getNodeValue()  获得节点的值
    * getNodeType()  获取节点类型
      * ELEMENT_NODE  1
    * TEXT_NODE       3
    * appendChild(Node newChild)
    
    | type                    | nodename                     | nodevalue                  | Attribute    |
  | ----------------------- | ---------------------------- | -------------------------- | ------------ |
    | Comment                 | "#comment"                   | 注释内容                   | null         |
    | Document                | "#document"                  | null                       | null         |
    | DocumentType            | DocumentType.name            | null                       | null         |
  | Element                 | Element.name                 | null                       | NamedNodeMap |
    | Processing Instructions | ProcessingInstruction.target | ProcessingInstruction.data | null         |
  | Text                    | "#text"                      | 文本内容                   | null         |
    | Attr                    | Attr.name                    | Attr.value                 |              |
    
    

  ```
  Node接口在整个DOM树中具有举足轻重的地位，DOM接口中有很大一部分是从Node接口继承过来的
  	例如，Element、Attr、CDATASection等接口，都是从Node继承过来的
  在DOM树中，Node接口代表了树中的一个节点。
  ```

  * NodeList
  * int getLength()  NodeList中节点的个数
    * item(int index)  根据索引取得节点

  ```
  NodeList接口提供了对节点集合的抽象定义，它并不包含如何实现这个节点集的定义。
  NodeList用于表示有顺序关系的一组节点，比如某个节点的子节点序列。
  另外，它还出现在一些方法的返回值中，例如getElementsByTagName
  
  在DOM中，NodeList的对象是“live”的，对文档的改变，会动态反映到相关的NodeList对象中。
  NodeList中的每个item都可以通过一个索引来访问，该索引值从0开始。
  ```

  * NamedNodeMap
    * 主要用在属性节点的表示上
    * Node item(int index)   获取属性节点

  ```
  实现了NamedNodeMap接口的对象中包含了可以通过名字来访问的一组节点的集合。
  但是NamedNodeMap并不是通过NodeList继承来的，它所包含的节点集中的节点是无序的，尽管这些节点也可以通过索引进行访问，但这仅是提供了遍历NamedNodeMap中所包含的节点一种简单方法，并不表明在DOM规范中为NamedNodeMap中的节点规定了一种排列顺序。
  
  与NodeList相同，在DOM中，NamedNodeMap对象是“live”的
  ```



### 7.2 DOM解析

```java
// 1.获得dom解析器工厂
// 用来创建具体的解析器
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();

// 2.获取具体的解析器
DocumentBuilder builder = factory.newDocumentBuilder();

// 3.解析一个xml文档，获得Document对象
Document document = builder.parse(new File("person.xml"));

// 4.根据标签名获取要解析的NodeList
NodeList list = document.getElementsByTagName("person");

// 5.遍历节点集
for (int i = 0; i < list.getLength(); i++) {
    Element element = (Element) list.item(i);
    String content = element.getElementsByTagName("name").item(0).getFirstChild().getNodeValue();
}
```

### 7.3 递归打印xml

```java
private static void parseElement(Element element) {
        System.out.print("<" + element.getTagName());

        // 添加属性
        NamedNodeMap attributes = element.getAttributes();
        if (attributes != null) {
            for (int i = 0; i < attributes.getLength(); i++) {
                Attr attr = (Attr) attributes.item(i);
                System.out.print(" " + attr.getNodeName() + "=\"" + attr.getNodeValue() + "\"");
            }
        }
        System.out.print(">");

        // 遍历childNode
        NodeList childNodes = element.getChildNodes();
        for (int i = 0; i < childNodes.getLength(); i++) {
            Node item = childNodes.item(i);
            if (item.getNodeType() == Node.ELEMENT_NODE) {
                parseElement((Element) item);
            } else if (item.getNodeType() == Node.COMMENT_NODE) {
                Comment comment = (Comment) item;
                System.out.print("<!--" + comment.getNodeValue() + "-->");
            } else if (item.getNodeType() == Node.TEXT_NODE) {
                System.out.print(item.getNodeValue());
            }
        }

        System.out.print("</" + element.getTagName() + ">");
    }
```

### 7.4 修改xml

```java
// 1.获取dom解析器工厂
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        // 2.获取具体的解析器
        DocumentBuilder builder = factory.newDocumentBuilder();
        // 3.解析文档
        String path = DOMParser.class.getClassLoader().getResource("student.xml").getPath();
        System.out.println("文件的位置：" + path);
        File file = new File(path);
        Document document = builder.parse(file);


// 使用xpath进行访问节点
        XPathFactory xPathFactory = XPathFactory.newInstance();
        XPath xPath = xPathFactory.newXPath();
        String expression = "//student[1]/name";
        Node node = (Node) xPath.evaluate(expression, document, XPathConstants.NODE);
        System.out.println(node.getFirstChild().getNodeValue());
        // 修改数据
        node.getFirstChild().setNodeValue("凯菇发病");
        System.out.println(node.getFirstChild().getNodeValue());

        // 上面的修改只是对内存中的dom树中的数据进行了修改，需要写入到文件中
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        // 设置编码
        transformer.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
        // 输出到文档
        DOMSource source = new DOMSource(document);
        StreamResult sr = new StreamResult(file);
        transformer.transform(source, sr);
```

student.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<students>
    <!--这里是洞庭湖直播间-->
    <student id="1">
        <name>凯菇</name>
        <age>22</age>
        <sex>female</sex>
    </student>
    <student id="2">
        <name aka="胡凯莉">骚人</name>
        <age>18</age>
        <sex>male</sex>
    </student>
</students>
```



## 八、XSLT

> XSL指扩展样式表语言（eXtensible Stylesheet Language)

XSLT是指对XSL转换。

* 根据xsl把xml代码转换成html或其他任意代码
* xslt是一种渲染技术
* xslt对于xml相当于css对于html
* xsl分为三部分
  * XSLT
  * XPath
  * XSL-FO

### 8.1 例子

class.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--引入xsl-->
<?xml-stylesheet type="text/xsl" href="index/xsl"?> 
<class>
	<student>
    	<name>小明</name>
        <age>12</age>
        <height>153</height>
    </student>
</class>
```

index.xsl

```xsl
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
	<xsl:template match="/">
		<html>
			<head></head>
			<body>
				<ul>
					<xsl:for-each select="class/student">
					<li>
						<xsl:value-of select="name"/>
					</li>
					</xsl:for-each>
				</ul>
			</body>
		</html>
	</xsl:template>
</xsl:stylesheet>
```

如果xmlns:xsl中使用了w3的网址，version必须是1.0

### 8.2 语法介绍

* XSL样式表由一个或多套被称为模板（template）的规则组成
* 每个模板含有当某个指定的节点被匹配时所应用的规则
* `<xsl:template>`元素用于构建模板
* match属性用于关联xml元素和模板，match属性也可用来为整个文档定义模板，属性的值为XPath表达式（例如，match="/"定义整个文档）
* XSL样式表本身也是一个XML文档，因此总是由XML声明起始
* `<xsl:stylesheet>`定义此文档是一个XSLT样式表文档（连同版本号和XSLT命名空间）
* select中也是XPath表达式，相对于上一级的节点（比如第一个select是相对于match、第二个select是相对于第一个select）



`<xsl:for-each select=“student[age &lt; 12]`

`<xsl:value-of select="name">`

`<xsl:sort select="age">`   按照年龄进行排序

`<xsl:if test="price &gt; 10>`   判断price是否大于10

```xsl
<xsl:choose>
	<xsl:when test="expression">
		...输出
	</xsl:when>
	<xsl:otherwise>
		...输出
	</xsl:otherwise>
</xsl:choose>
```

`xsl:apply-templates`元素可把一个模板应用于当前的元素或者当前元素的子节点