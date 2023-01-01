## nunit的配置

1. 在项目上使用`Tools -> NuGet Package Manager`下载`NUnit`和`NUnit3TestAdapter`以及`Microsoft.NET.Test.Sdk`
   * `NUnit`包需要被test assembly引用
   * `Microsoft.NET.Test.Sdk`
     * The MSbuild targets and properties for building .NET test projects

一种快速的方法，直接创建Nunit项目，并添加要被测试的project的reference】



## demo

```c#
[TestFixture]  // this attribute is optional after nunit 3
class LoanTermShould
{
    [Test]
    public void TestToMonths()
    {
        var sut = new LoanTerm(1);
        Assert.That(sut.ToMonths(), Is.EqualTo(12));
    }
}
```



## NUnit Framework

### NUnit Library

> Attributes
>
> Assertions

#### Attributes

| Attribute      | Description                         |
| -------------- | ----------------------------------- |
| [TestFixture]  | Mark a class that contains tests    |
| [Test]         | Mark a method as a test             |
| [Category]     | Organize tests into categories      |
| [TestCase]     | Data driven test cases              |
| [Values]       | Data driven test parameters         |
| [Sequential]   | How to combine test data            |
| [SetUp]        | Run code before each test           |
| [OneTimeSetUp] | Run code before first test in class |

#### Assertions

**Constraint Model of assertions (newer)**

`Assert.That(TActual actual, IResloveConstraint expression [, string msg]);`

for example

`Assert.That(sut.Years, Is.EqualTo(1));`

 这个等同于

`Assert.That(sut.Years, new EqualConstraint(1));`

**Classic Model of assertions (older)**

`Assert.AreEqual(1, sut.Years);`

`Assert.NotNull(sut.Years)`



##### Assertion on Equality

Assert.That(res, Is.EqualTo(other));

Assert.That(res, Is.Not.EqualTo(other));



##### Assertion on Reference Equality

Assert.That(res, Is.SameAs(other));

Assert.That(res, Is.Not.SameAs(other));



##### Assertion on Tolerance

判断两个值在一个偏差内（可以是百分数）是否相同

Assert.That(res, Is.EqualTo(other).Within(torelance))

Assert.That(res, Is.EqualTo(other).Within(percent).Percent)



##### Assertion on Collection

判断集合有确定个数目的item

`Assert.That(collection, Has.Exactly(num).Items);`

判断集合内的对象是否是unique

`Assert.That(collection, Has.Exactly(num).Items);`

判断集合内是否有某个对象

`Assert.That(collection, Does.Contain(expectedObj))`

判断集合内的几个对象是否有确定的属性和对应的属性值

```c#
Assert.That(collection, Has.Exactly(num)
            .Property(属性名).Equals(属性值)
           .And
           .Property(属性名).GreaterThan(属性值));

// 另一种写法
Assert.That(collection, Has.Exactly(num)
           				.Matches<对象的类型>(
                        item => item.属性名 == 属性值 &&
                        		item.属性名 > 属性值));
```



##### Assert that Exception is thrown

`Assert.That((输入参数) => 语句, Throws.TypeOf<异常类型>());`

```c#
Assert.That((输入参数) => 语句, Throws.TypeOf<异常类型>()
           						.With
           						.Property("Message")
           						.EqualTo(错误消息名称));
```



##### Assert on Null Values

Assert.That(res, Is.Null)

Assert.That(res, Is.Not.Null)



##### Assert on string values

Assert.That(str, Is.Empty)

Assert.That(str, Is.Not.Empty)

Assert.That(str, Is.EqualTo(str).IgnoreCase)

Assert.That(str, Does.StartWith("s"))

Assert.That(str, Does.EndWith("S"))

Assert.That(str, Does.Contain("s"))



##### Assert on bool values

Assert.That(boolVal)

Assert.That(boolVal == true)

Assert.That(boolVal, Is.True)



##### Assert with Ranges

Assert.That(res, Is.GreaterThan(1))

Assert.That(res, Is.LessThan(1))

Assert.That(res, Is.GreaterThanOrEqualTo(1))

Assert.Than(res, Is.LessThanOrEqualTo(1))

Assert.That(res, Is.InRange(low, high))

对于日期，还可以

Assert.That(date_1, Is.EqualTo(date_2).Within(num).Days));





### Test Runner

## Recognize Different Testing Scenarios

* Business Logic
* Exercise all code branches
  * branch converage
* Bad data/input

## Quality of good tests

* fast
* repeatable
* isolated
* trustworthy
* valuable



## Ignore Tests

使用`Ignore(string reason)`attribute添加到class或者method上

其中的test method就会在执行的时候被跳过



## Category

当几个test method属于同一个类别，并需要一起被执行

可以使用`[Category(string categoryName)]`attribute来标注class或者是method

一个class或method可以被多个不同的category attribute标注

在test explorer界面选择traits



## Test Execution Cycle

NUnit每次运行的时候都会为test class创建一个instance

`Constructor -> One-time setup -> Setup -> method1 -> Teardown`

`-> Setup -> method2 -> Teardown -> ... -> One-time tear down -> Dispose`

`[SetUp]` : 每次执行test method前执行被SetUp标注的方法

`[TearDown]` ：每次执行test method后执行TearDown标注的方法

`[OneTimeSetUp]` ： 在执行第一个SetUp方法和test method之前执行

`[OneTimeTearDown]` ： 在执行最后一个test method和TearDown方法之后执行



## TestCase

我认为是Data-driven test的核心

通过`[TestCase]`attribute将测试数据传入测试方法

```c#
[Test]
[TestCase(1, "nihao")]
public void TestMethod(int a, string str)
{
    ...
}
```

还有一种方式可以不使用Assert

```c#
[Test]
[TestCase(1, ExpectedResult=2)]  
// 会比较测试方法的返回值与expectedresult是否一致
public int TestMethod(int a)
{
    return CallMethod(a);
}
```



## TestCaseSource

使用TestCaseSource可以让多个test method共享相同的test cases

```c#
public class CaseSource
{
    public static IEnumerable TestCases
    {
        get
        {
            yield return new TestCaseData(1);
            yield return new TestCaseData(2);
            yield return new TestCaseData(3);
        }
    }
}

public class TestClass
{
    [Test]
    [TestCaseSource(typeof(CaseSource), "TestCases")]
    public void TestMethod(int i) {}
}
```

也有一种不需要使用Assert的方法

```c#
public class CaseSource
{
    public static IEnumerable TestCases
    {
        get
        {
            yield return new TestCaseData(1).Returns(1);
            yield return new TestCaseData(2).Returns(1);
            yield return new TestCaseData(3).Returns(1);
        }
    }
}

public class TestClass
{
    [Test]
    [TestCaseSource(typeof(CaseSource), "TestCases")]
    public int TestMethod(int i) 
    {
        return (int)...
    }
}
```



## 生成测试数据

### Value & Sequential

Values可以生成测试数据

```c#
[Test]
public void Calculate(
			[Values(1, 2, 3)]int a,
            [Values(2, 3, 4)]int b,
			[Values(3, 4, 5)]int c){}
// 会生成3*3*3=27种不同组合的测试数据
```

Sequential可以指定测试数据是有顺序的

```c#
[Test]
[Sequential]
public void Calculate(
			[Values(1, 2, 3)]int a,
            [Values(2, 3, 4)]int b,
			[Values(3, 4, 5)]int c){}
// 会生成3种不同组合的测试数据
```



### Range

```c#
// a从start开始到end结束，以step为步长这么多测试数据
[Test]
public void TestMethod([Range(start, end, step)]int a) {}
```



## Custom Category

如果想要让一些测试方法属于同一个分类，除了直接使用Category attribute

还可以自定义attribute

比如对于`[Category("first")]`

```c#
public class FirstCategory : CategoryAttribute
{
    
}
```

然后在原本需要写`[Category("first")]`的地方写上`[FirstCategory]`



## Custom Constraint

如果需要custom Constraint

```c#
public class CustomConstraint : Constraint
{
    // 属性
    
    // 构造方法
    
    public override ConstraintResult ApplyTo<TActual>(TAcutal actual)
    {
        T t = actual as t;
        if (t is null)
        {
            return new ConstraintResult(this, actual, ConstraintStatus.Error);
        }
        // ...
     	return new ConstraintResult(this, actual, ConstraintStatus.Success);    
        
        // ...
        return new ConstraintResult(this, actual, ConstraintStatus.Failure);    
    }
}
```



## Mocking with Moq

> Install `Moq` library
>
> 用处：isolation for testing
>
> 让原本需要被测试的方法使用的real dependency变成mock dependency

Mocking: Replacing the actual dependecy that would be used at production time, with a test-time-only version that enables easier isolation of the code we want to test.

**好处**

* Reduce test complexity
* Improve test execution speed
* Non-deterministic dependencies
* Support parallel development streams
* Improve test predictability / reliability
* Reduce test costs

### fakes,dummies, stubs and mocks

| Name    | Use                                                 | Example                        |
| ------- | --------------------------------------------------- | ------------------------------ |
| Fakes   | Working implementation, not suitable for production | EF Core in memory provider     |
| Dummies | Pass around, never used, Satisfy parameters         |                                |
| Stubs   | Provide answers to calls                            | Property gets, method returns  |
| Mocks   | Verify interaction                                  | Properties used, method called |



### Mock的使用

```c#
var obj = new Mock<Type>();
Console.WriteLine(obj.Object);  // Object属性是Type的类型
```



### Configuring mock method return values

```c#
var mockObj = new Mock<Type t>();
// Method是t类型的方法
mockObj.Setup(x => x.Method(方法参数)).Returns(返回值);
```

**Configuring mock methods to return null**

```c#
var mock = new Mock<Type t>();
mock.Setup(x => x.Method(方法参数).Returns<返回值类型>(null));
返回值类型 mockReturnValue = mock.Object.Method();
Assert.That(mockReturnValue, Is.Null);
```



### Argument matching with Moq

如果对于方法参数没有限制，可以使用`It.IsAny<Type t>`来允许传入t类型的所有值



### Mocking methods with out/ref parameters

**out**

```c#
var mockObj = new Mock<Type t>();
// Method是t类型的方法
mockObj.Setup(x => x.Method(方法参数, ..., out 返回参数));
```

**ref**

```c#
var mockObj = new Mock<Type t>();
mockObj.Setup(x => x.Method(方法参数, ..., ref It.Ref<引用参数的类型>.IsAny))
    	.Callback(new XXXCalback(方法参数, ref 引用参数类型 引用参数名) => 给引用参数赋值);
// 会判断两个函数最终的引用参数是否相等
```



### Configure a method property to return a specified value

```c#
var mockObj = new Mock<Type t>();
mockObj.Setup(x => x.属性).Returns(属性的值);
```



### Mocking property hierarchies

#### manual

```c#
var mockInnerObj = new Mock<InnerObj>();
mockInnerObj.Setup(x => x.属性).Returns(返回值);
var mockMiddleObj = new Mock<MiddleObj>();
mockMiddleObj.Setup(x => x.属性).Returns(mockInnerObj.Object);
var mockOuterObj = new Mock<OuterObj>();
mockOuterObj.Setup(x = > x.属性).Returns(mockMiddleObj.Object);
```

#### Automatic

```c#
var mockOuterObj = new Mock<OuterObj>();
// 此时MiddleObj.InnerObj是virtual
// InnerObj.属性也是virtual
mockOuterObj.Setup(x => x.MiddleObj.InnerObj.属性).Returns(返回值);
```

另一种方法是使用DefaultValue

```c#
var mockObj = new Mock<OuterObj>{ DefaultValue = DefaultValue.Mock };
```



#### Configuring mock properties to track changes

```c#
var mockObj = new Mock<Type t>();
// 使用SetupProperties追踪属性的change
mockObj.SetupProperty(x => x.属性 [, initialValue]);

// 使用SetupAllProperties追踪属性的change
// 但是这种方式同时还会调用每个属性的setter
mockObj.SetupAllProperties();
```



### Verifying

#### Verifying a method was called

```c#
mockObj.Verify(x => x.要验证的方法(方法参数));
```



#### Verifying a method was called a specific number of times

```c#
// 验证指定方法被调用一次
mockObj.Verify(x => x.方法(), Times.Once);
// 验证指定方法被调用指定次数
mockObj.Verify(x => x.方法(), Times.Exactly(int times));
// 验证指定方法从未被调用
mockObj.Verify(x => x.方法(), Times.Never);
```



#### Verifying property setters and getters were called

```c#
// 验证属性的Get是否被调用
mockObj.VerifyGet(x => x.属性 [, Times.Once]);

// 验证属性的Set是否被调用
mockObj.VerifySet(x => x.属性 = 被设置的值(可以是It.IsAny<int>()) [, Times.Once]);
```



#### Verifying that no other unexpected calls are made

```c#
// 验证除了其他Verify方法中的属性和方法调用，没有别的调用
mockObj.VerifyNoOtherCalls();
```



## Advanced Mock Techniques

### Strict Mock and Loose Mock

默认情况下是loose mock，即`MockBehavior.Loose`

```c#
var mockObj = new Mock<MockObj>(MockBehavior.Strict);
// 当设置为strict mock的时候，需要set up当前所有被调用到的方法
```

### 让Mock Throw Exception

```c#
var mockObj = new Mock<MockObj>();
mockObj.Setup(x => x.Method(方法参数).Throws<XxxException>());

// 或者
mockObj.Setup(x => x.Method(方法参数).Throw(new XxxException(string erroMsg)));
```



## InSequence

让方法之间的调用有顺序

mock对象创建的时候必须是严格模式，只有在严格模式`MockBehavior.Strict`下，调用未setup的方法会报错

```c#
public class TestClass
{
    [Test]
    public void TestMethod()
    {
        // Arrange
        Mock<IInnerClass> mockInnerClass = new(MockBehavior.Strict);
        var seq = new MockSequence();
        mockInnerClass.InSequence(seq).Setup(x => x.MethodA(It.IsAny<int>()));
        mockInnerClass.InSequence(seq).Setup(x => x.MethodB());
        mockInnerClass.InSequence(seq).Setup(x => x.MethodC());
        OuterClass outerClass = new(mockInnerClass.Object);

        // Act
        outerClass.Method();

        // Assert
        Assert.Multiple(() =>
        {
            mockInnerClass.Verify(x => x.MethodA(It.IsAny<int>()), Times.Once);
            mockInnerClass.Verify(x => x.MethodB(), Times.Once);
            mockInnerClass.Verify(x => x.MethodC(), Times.Once);
        });
    }

public class OuterClass
{
    private IInnerClass _innerClass;
    public OuterClass(IInnerClass innerClass)
    {
        _innerClass = innerClass;
    }
    public void Method()
    {
        _innerClass.MethodA(1);
        _innerClass.MethodB();
        _innerClass.MethodC();
    }
}
public interface IInnerClass
{
    void MethodA(int a);
    void MethodB();
    void MethodC();
}
public class InnerClass : IInnerClass
{
    public void MethodA(int a) { }
    public void MethodB() { }
    public void MethodC() { }
}
```

