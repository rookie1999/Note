## Chapter 1: IDO .NET API and class library

开发者使用IDO.NET类库（IDO）去访问Mongoose对象。

与IDO的交互是通过请求与响应。

调用者创建IDO请求，发送给IDO runtime service。

IDO runtime service返回响应，该响应返回请求动作的结果。

IDO runtime service主要的职责有：

* 查询数据集（LoadCollection）
* 保存数据（UpdateCollection）
* 调用函数（Invoke）

每一个IDO请求会带有一个request type，并带有一个可选的request  payload（比如OpenSession request type的payload会带有一些logon information）

* IDO .NET 类库
  * namespace   `Mongoose.IDO`
  * 实现了 IDO command interface
    * LoadCollection
    * UpdateCollection
    * Invoke
  * Additional commands
    * OpenSession
    * GetConfigurations
    * CloseSession
* IDO .NET protocol 类库
  * 包含对应IDOrequest和response中每个层级的类(这些类是用来创建request和response)
  * namespace   `Mongoose.IDO.Protocol`
  * IDOProtocal顶层类被IDORequestEnvelope类所实现
  * IDO request（XML的请求头）被IDORequest所实现
  * 而payload的实现根据request type的不同也不同
    * OpenSession -> OpenSessionRequestData
    * LoadCollection -> LoadCollectionRequestData
  * 响应的层级结构与Request一一对应，顶级类是IDOResponseEnvelope

### 创建执行IDO runtime invoke call的流程

1. 创建`IDORequestEnvelope`实例
2. 设置`IDORequestEnvelope.SessionID`属性
3. 创建`IDORequest`实例
4. 设置`IDORequest.Type`的属性为Invoke
5. 创建`InvokeRequestData`的实例
6. 设置`InvokeRequestData.IDOName`属性
7. 设置`InvokeRequestData.MethodName`属性
8. 对`InvokeRequestData.Parameters`集合添加参数
9. 将`IDORequest加到IDORequestEnvelope.Requests`集合
10. 使用`IDORequest.SetPayload`方法将InvokeRequestData`加入IDORequest`
11. 将`IDORequestEnvelope`加入IDO runtime service，并等待接收响应

### IDO commands interface

框架提供了在IDO commands interface中的wrapper methods来封装IDO interaction（将创建IDORequestEnvelope和IDORequest和payload的实例变成a single method call）

限制：一次只能发送一个请求并接受一个响应

如果使用IDORequestEnvelope，可以一次封装多个请求

顶级接口是：`IIDOCommands`

* GetPropertyInfoResponseData GetPropertyInfo(string idoName)
  * 返回一个IDO的a list of properties and their attributes
  * LoadCollectionRequestData的属性有IDOName和Include
* LoadCollectionRequestData LoadCollection(LoadCollectionRequestData requestData)
* UpdateCollectionResponseData UpdateCollection(UpdateCollectionRequestData requestData)
* InvokeResponseData Invoke(InvokeRequestData requestData)
* string[] GetIDONames()
* LoadCollectionResponseData LoadCollection(string idoName, string propertyList, string filter, string orderBy, int recordCap)
* LoadCollectionResponseData LoadCollection(string idoName, PropertyList propertyList, string filter, string orderBy, int recordCap)
* InvokeResponseData Invoke(string idoName, string methodName, params object[] parameters)

当需要对IDO protocol class的属性或者参数赋值为null value，最好的方法是使用`IDONull.Value`静态属性

当需要检查是否为null value时，最好的方法是使用`IDONull.IsNull`静态方法。如果要检查IDO protocol class内的参数或者属性值，可以使用`IsNull`属性值进行检查

### ApplicationDB类

namespace：`Mongoose.IDO.DataAccess`

提供对application database的直接访问

提供对common framework features比如`application message`和`session variables`的访问

`AppDB db = Mongoose.IDO.IDORuntime.Context.CreateApplicationDB();`

**属性**

* connection
  * provides an opened SqlConnection

**方法**

* static IDbCommand CreateCommand()
  * 还需设置CommandType和CommandText
  * 添加参数到`Parameters`属性
    * `ApplicationDB.AddCommandParameterWithValue`
    * `ApplicationDB.AddCommandParameter`
  * 当command被执行，需要使用`ApplicationDB.ExecuteScalar`和`ApplicationDB.ExecuteNonQuery`和`ApplicationDB.ExecuteReader`

访问SessionVariable

* public void SetSessionVariable(string name, string varValue)
* public void GetSessionVariable(string name)
* public string GetSessionVariable(string name, string defaultValue)
  * 不存在就返回defaultValue
* public string GetSessionVariable(string name, string defaultValue, bool delete)
* public void DeleteSessionVariable(string name)

访问ProcessVariable

* public void SetProcessVariable(string name, string varValue)
* public void SetProcessVariable(string name, string varValue, bool ignoreError)
* public string GetProcessVariable(string name)
* public string GetProcessVariable(string name, string defaultValue)
* public string GetProcessVariable(string name, string defaultValue, bool deleteVariable)
* public void DeleteProcessVariable(string name)

#### 在IDO extension class method中使用ApplicationDB

```c#
using (ApplicationDB appDB = this.CreateApplicationDB()) 
{
    
}
```

### Application Message

访问message的功能是`IMessageProvider` interface

This example shows how to use the ApplicationDB class to retrieve message

```c#
IMessageProvider messageProvider = null;
using (ApplicationDB appDB = this.CreateApplicationDB()) 
{
    messageProvider = appDB.GetMessageProvider();
    Infobar = messageProvider.AppendMessage(Infobar, "I=CmdMustPerform", "@:PostPendingMaterialTransactions");
}
```

如果是写继承自`IDOExtensionClass`的IDO class，可以直接调用

```c#
IMessageProvider messageProvider = null;
messageProvider = this.GetMessageProvider();
```



### 数据格式

IDO protocol class所有的属性和参数都是string类型的

#### 数字的格式

以小数点进行分隔，并且不包含任何digit-grouping字符

#### 日期的格式

`YYYYMMDD HH:MM:SS.mmm` for date and time

`YYYYMMDD` for date

`HH:MM:SS.mmm` for time

### IDO protocol class的property和parameter values

设置parameter values使用Add方法或者SetValue方法 (这两个方法内部完成了type conversion)

```c#
InvokeRequestData invokeRequest = new InvokeRequestData();
invokeRequest.Parameters.Add("Mongoose");
invokeRequest.Parameters[0].SetValue(100);
```

设置property直接使用set方法

```c#
InvokeRequestData invokeRequest = new InvokeRequestData();
invokeRequest.IDOName = "MyIDO";
```

获取property和parameter values使用GetValue方法

需要传入一个.Net类型作为泛型

```c#
LoadCollectionResponseData loadResponse;
long userId = 0;
loadResponse = this.Context.Commands.LoadCollection("UserNames", "UserId, UserName, RecordDate, RowPointer", "", "", 0);
userId = loadResponse[0, "UserId"].GetValue<long>();
```

GetValue方法如果获取到Null值的时候，会抛出异常

三种处理方法：

1. 在调用GetValue之前检查IsNull属性

   ```c#
   if (!loadResponse[0, "UserId"].IsNull) {
       userId = loadResponse[0, "UserId"].GetValue<long>();
   } else {
       userId = -1;
   }
   ```

2. 调用GetValue重载的方法，传入一个默认值

   ```c#
   userId = loadResponse[0, "UserId"].GetValue<long>(-1);
   ```

3. 使用.Net nullable类型

   ```c#
   Nullable<long> userId = loadResponse[0, "UserId"].GetNullableValue<long>();
   if (!userId.HasValue) {
       userId = -1;
   }
   ```

### Create filter strings

sql filter（where子句）

SQLLiteral会把.Net CLR类型的值转换为一个literal（可用在filter string中）

```c#
string filter = null;
filter = string.Format("SessionId = {0} AND trans_num = {1}", SQLLiteral.Format(SessionID), SQLLiteral.Format(TransNum))
```

### Transaction

IDO runtime自动运行所有的UpdateCollection在一个transaction

被标记为`transactional`的方法也会运行在一个transaction

其他的都不在transaction中运行

如果一个方法不是transactional但是它被另一个transactional方法调用了，则这两个方法都在一个caller's transaction

对于transactional IDO methods，返回值<5，transaction会提交，否则回滚

inner transaction只有在outer transaction提交之后才能提交

### Logging

The log is available in the IDO Runtime Development Server (IDORuntimeHost.exe)

```c#
public static void LogUserMessage(string messageSource, UserDefinedMessageType messageType, string message) {}

public static void LogUserMessage(string messageSource, UserDefinedMessageType messageType, string format, params object[] args) {}
```

Examples

```c#
IDORuntime.LogUserMessage("SLCustomers", UserDefinedMessageType.UserDefined0, "Method A() called.");

IDORuntime.LogUserMessafe("SLCustomers", UserDefinedMessageType.UserDefined0, "Parameter 1 is {0}.", p1);
```

## Chapter 2: IDO extension classes

> An IDO extension class 是一个运行开发者扩展已有的IDO的功能（增加方法或者event handler）的.Net类
>
> IDO extension class会编译成.Net class library assembly并存储在IDO metadata database
>
> An extension class的生命周期是始于request，终于response
>
> 因此，不能存储任何state information

### Mongoose.IDO.IIDOExtensionClass

```c#
void setContext(Mongoose.IDO.IIDOExtensionClassContext);
```

Mongoose提供了一个base class叫做`Mongoose.IDO.ExtensionClass`，该类实现了`IIDOExtensionClass`接口

### Mongoose.IDO.IIDOExtensionClassContext

```c#
Mongoose.IDO.IVirtualIDO IDO {get;}
Mongoose.IDO.IIDOCommandHelper Commands {get;}
```

### IDO extension class starter template

```c#
using Mongoose.IDO; 
using Mongoose.IDO.Protocol; 
using Mongoose.IDO.DataAccess; 
using Mongoose.Core.Common;

[IDOExtensionClass("class-name")] 
public class class-name : IDOExtensionClass
{ 
 // optionally override the SetContext method 
 // public override void SetContext( 
 // IIDOExtensionClassContext context ) 
 // { 
 // call base class implementation 
 // base.SetContext(context); 
// 
 // Add event handlers here, for example:
 // context.IDO.PostLoadCollection += 
// new IDOEventHandler( IDO_PostLoadCollection ); 
 // } 
 // ...Implementation goes here... 
} 
```

#### Adding method

只有在IDO metadatabase中定义为hand-coded方法可以被IDO Runtime运行

一般在实现一个hand-coded method需要添加`IDOMethod` attribute

```c#
[IDOMethod(MethodFlags.None, "Infobar")]
public int DoWork(string SessionID, long TransNum, ref string Infobar) { // ...Implementation goes here
}
```

`IDOMethod` attribute需要两个可选参数`flags`和`messageParm`

**Flags**

| MethodFlags value   | Description                                 |
| ------------------- | ------------------------------------------- |
| None                | Standard method call                        |
| CustomLoad          | Custom load method                          |
| RequiresTransaction | Method runs in the context of a transaction |

**MessageParm**

string type

是返回值的name，并且该parameter需要声明为`ref string`

**注意**

当使用`custom load method`时，在IDO上面定义的filters不会自动使用，开发者需要`explicitly implement the filtering in the custom method`

#### Event Handlers

当IDO在处理LoadCollection或者UpdateCollection请求时，会触发一些事件

| Event                | Description                                               |
| -------------------- | --------------------------------------------------------- |
| PreLoadCollection    | Fires when a LoadCollection request is received           |
| PostLoadCollection   | Fires after a LoadCollection request has been processed   |
| PreUpdateCollection  | Fires when an UpdateCollection request is received        |
| PostUpdateCollection | Fires when an UpdateCollection request has been processed |

**Define an IDO event handler**

```c#
private void HandlePreLoadCollection(object sender, IDOEventArgs args) {}
```

第一个参数是触发event的对象，等价于`IIDOExtensionClassContext.IDO`

第二个参数是`IDOEventArgs` class

对于request，可以获取到`RequestPayload`

对于response，可以获取到`ResponsePayload`

```c#
public class IDOEventArgs : EventArgs {
    public PayloadBase RequestPayload {get;}
    public PayloadBase ResponsePayload;
}
```

| Event                | RequestPayload                                               | ResponsePayload                                              |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| PreLoadCollection    | Contains an instance of the LoadCollectionRequestData class that the IDO is processing |                                                              |
| PostLoadCollection   | Contains an instance of the LoadCollectionRequestData class that the IDO processed | Contains an instance of the LoadCollectionResponseData class that will be returned to the caller |
| PreUpdateCollection  | Contains an instance of the UpdateCollectionRequestData class that the IDO is processing |                                                              |
| PostUpdateCollection | Contains an instance of the UpdateCollectionRequestData class that the IDO processed | Contains an instance of the UpdateCollectionResponseData class that will be returned to the caller |

**Example**

```c#
private void HandlePreLoadCollection(object sender, IDOEventArgs args) 
{
    // Get the original request
    LoadCollectionRequestData loadRequest = (LoadCollectionRequestData) args.RequestPayload;
    // ...
}
```



**把event handler添加到event上**

需要override `IDOExtensionClass.SetContext`方法

```c#
public override void SetContext(IIDOExtensionClassContext context) 
{
    base.SetContext(context);
    this.Context.IDO.PreUpdateCollection += this.PreUpdateCollection;
}
```

**Application event**

在ExtensionClassBase类中，用来generate an application event

```c#
protected bool FireApplicationEvent(string eventName, bool synchrnous, bool transactional, ref string result, ref ApplicationEventParameter[] parameters)
```

### IDO Extension Class Disposable

让IDO extension class继承`IDisposable` interface，override`Dispose`方法

### 在本地测试newly-added IDO extension class

将新编写的extension classcopy到folder：`appName/idoxca/configName`

`appName`是application root folder

`idoxca`是一个directory，set up specifically for testing your application and configurations

`configName`是configuration name