### Scrum

> An agile project management methodology
>
> idea of adding value to  a software product in an iterative manner
>
> These iterations are called ***sprints***
>
> All work is prioritized on  the product backlog and, at the start of each sprint, the development team commits to the work that  they will complete during the new iteration by placing it on the sprint backlog. (每个sprint会有一个排好优先级的work list)
>
> The unit of work with Scrum is **story**

![image-20211129213506486](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211129213506486.png)

Scrum involves a mixture of documentation artifacts, roles played by people inside and outside  the development team, and ceremonies—meetings that are attended by appropriate parties. 

Agile emphasizes working software, while waterfall methods emphasize documentation

Wikis are common tools in Scrum tools

Scrum also refine the development process iteratively



#### Roles and responsibility

* Product owner (PO)
  * Deciding which features are built
  * Setting the priority of the features in terms of business value
  * Accepting or rejecting "completed" work
  * 不影响开发团队工作，只是在最后确认features是否按照要求交付
  * owns the product
* Scrum master (SM)
  * responsibility: ensure that the process is followed by the team
  * 是开发团队与外界交流的中介
  * 但是不能干预开发团队怎样去implement a story
  * owns the process (also owns the `scrum meeting`)
* Development team
  * generalizing specialists
  * 什么都懂，但是在某一方面专精



#### Artifacts

> Documentation is still important to an Agile project, but its importance does not supersede that of working software or communication.

##### Scrum Board

* Central to the daily workings of a Scrum project
* A physical scrum board is a must (better than a digital one)

##### Cards

* cards是在scrum board上
* 可以用color表示重要程度

![image-20211201144026988](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211201144026988.png)

Minimum viable product (MVP)

Minimum marketable feature (MMF)

feature = epic

3 types of features

* required
* preferred
* desired

这三种type也代表了优先级（required > preferred > desired）

**User story**

```
As a [user role], I want to [verb-centric behavior], so that [user value added].
```

user story是从user的角度，最好不要从developer的角度

developer与user进行讨论，理解需求之后，开始design怎么去实现(interfaces mockup 或 UML)

design完成后，可以将story拆分成多个tasks进行实现

实现之后，最后一步交给QA进行核实是否满足需求

总结：

```
With a user story for guidance, developers gathered requirements in an analysis phase, generated a design, implemented a working solution, and then tested this against the acceptance criteria.
```

**Task**是比user story小一级

**Technical debt** is a metaphor for the design and architectural compromises that have been made during a story's journey across the Scrum board.

technical debt 没有user story point

可以分为四种情况

* Reckless 鲁莽的, deliberate
  * the most poisonous
* Reckless, inadvertent 无意的
  * This type of debt is most likely created by a lack of experience. It is  the result of not knowing best practices in modern software engineering.
  * Education is the answer here
* Prudent 谨慎的, inadvertent
  * This occurs when you follow best practices but it turns out that there  was a better way of doing something, and “now you know how you should have done it.” 
* Prudent, deliberate
  * the most acceptable
  * 与“ship now and deal with the consequences”决定有关

因为technical debt没有直接激励，但也需要repay。最好的做法是将technical debt与story关联起来，在处理story的同时，两个一并处理

**Defect** is created whenever acceptance criteria are not yet met on a previously complete user story.

Defect 没有user story point

Defects can be broadly categorized as A, B, or  C: apocalyptic defects 启示性缺陷, behavioral errors 行为错误, and cosmetic issues 美容问题

* Apocalyptic defects
  * result in an outright crash of the application or otherwise prevent the continuation of user's work
* Behavioral errors
  * 比如业务逻辑错误
* Cosmetic issues
  * 美容问题

##### Swimlanes

Scrum boards have vertical lines called swimlanes. Each swimlanes con contain multiple user story cards to denote the progress of that story throughout its development life cycle.

从左到右是`Backlog`, `In Progress`, `QA`, `Done`

The scrum board can be further split by using `horizontal swimlanes`. These swimlanes can be used to group the stories by feature.

One special swimlane at the top of the board is the Fast-Track lane, into which any very high-priority tasks can be placed.



##### Definition of Done

The definition of Done is depended  from your team.

```
A standard DoD is:
* Unit-test all code to cover its success and failure paths, with all tests passing
* Ensure that all code is submitted to the Continuous Integration builds and compiles -- without errors -- with all tests passing
* Verify behavior against the acceptance criteria with the product owner
* Have a developer who did not work on the story peer-review code
* Document just enough to communicate intent
* Reject reckless technical debt
```



#### Charts and metrics

> indicate the health and historical progress of a Scrum project, in addition to predicting probable future achievement

作用：Diagnose problems with the project as a whole

不要试着去measure anything on a personal level，会引起团队的错误行为（sacrifice team progress for their personal progress）

避免 observer effect观察者效应  （人在知道的前提下会过度表现）

##### Story points

> intended to incentivize the team to add business value with every sprint
>
> are assigned to user stories by the whole development team during sprint planning meeting

A story point is a measure of relative effort (requirement analysis, technical design, code implementation with unit testing, QA against acceptance criteria, deployment) required to implement the behavior that the user story represents

不同team的“one-point story”是不同的

story point直到story is done才claim

##### Velocity

可以根据前几个sprint完成的story points估算team's velocity

* 用来确定team在一个sprint内最多完成的story points
  * 如果一个team在一个sprint内平均是10 points，这时把goal设置在9或10都是可以接受的（让team meet or exceed the goal）
* 用来analyze problems with delivery
  * 如果当前sprint的story points完成的很少，something bad 发生了
    * story too big
    * 很多员工请假
    * code is not adaptive (花太多时间在refactor，unit test)

##### Sprint Burndown Chart

> Tracks progress at story level

![image-20211211113109506](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211211113109506.png)

在Daily Scrum meeeting上，昨天完成的story的point会被current remaining total减去

##### Feature Burnup Chart

> Track progress at completed features level

目标是让feature burnup图 increase linearly over time



#### Backlog

> A backlog is a list of pending items that are yet to be addressed

##### Product Backlog

Features' Priority是由其business value决定的，而business value是由PO决定的

如果priority相同，则比较required relative effort （低的排在前面，风险小，时间少且ROI高）

Product backlog也包含defect，但是其effort是不可估算的（因为产生的原因不明）

##### Sprint Backlog

> Contains all of the user stories that are to be completed in the coming sprint

Sprint backlog and all of the time estimation are owned by the team.



#### Sprint

Sprints are generally referred to index number, starting with 0.

Sprint 0一般是为整个team准备开发环境，并且为Sprint 1做一些Planning meeting

**接下来是在sprint内按照时间排序的不同meeting**

##### 1  Release Planning

* Decide on a release date
* prioritize and size the features that are to be included

##### 2 Feature estimation

* Predict the amount of effort
* in Common T-shirt sizes:
  * XL、L、M、S、XS

##### 3 Feature priority

* Features type
  * Required, Preferred, Desired
* Business stakeholders should number the features so that the development team can be sure to implement each feature in priority order

##### 4 Sprint planning

* Expected outcome: to estimate user stories

* Planning Poker

  * 用纸牌估计每个story的story point
  * 包含开发团队，PO，Scrum Master

* Affinity estimation

  * A quicker way to estimate the relative sizes of stories

  ```
  将所有的story根据size进行从小到大排序，然后根据斐波那契数列给每个story按序赋值，先完成small size的story
  ```

##### 5 Daily scrum

* Answer 3 questions
  * What did you do yesterday?
  * What will you do today?
  * What impediments do you face?
* 不要跑题

##### 6 Sprint demo

* 收集所有完成的user stories，描述story的scope，想要达到的效果，以及最后在生产环境中展示

##### 7 Sprint retrospective

* 回顾整个sprint，并回答下列问题
  * What went well?
  * What went badly?
  * What do we need to start doing?
  * What do we need to stop doing?
  * What do we need to continue doing?
  * Did we experience any surprises during the sprint?

##### 8 Story point triangulation

* 对于那些estimated user story point和实际开发时长相差很大的story进行重新衡量

##### 9 Scrum Calendar

* Shows typical Scrum meetings
* Shows the timing of daily Scrum



#### Agile in the real world

下列这些features会导致code maladaptive

* Rigidity
* Lack of abstractions
* Too many abstractions
* Mixed responsibility on some levels such as methods, classes, modules
* Untestability
  * 这些是难测试的
    * Static methods
    * Static classes
    * Object construction that uses new
    * Extension methods
  * 上面的可以变成
    * Interfaces
    * Dependency injection
    * Inversion of control
    * Factories

* Metrics
  * Reduce the complexity of code down to numbers that indicate the health of the project
* Unit test coverage
  * A measurement of the percentage of the code that is covered by unit tests
  * 80% is minimum acceptable level
  * UT coverage一般和CI一起完成（有些时候可以设定当UT coverage倒退了，则CI build直接失败）
* Cyclomatic complexity  循环复杂度
  * A measure of the number of paths that exist in the code
  * 数量上表现为独立线性路径条数，也可理解为覆盖所有的可能情况最少使用的测试用例数
  * Cyclomatic Complexity = #Edges - #Nodes + 2



## Dependencies and layering

### Code Characteristic

* Maintainable
* Readable
* Testable
* Adaptive



### Dependencies

> A dependency is a relationship between two distinct entities whereby one cannot perform some function without the other.

* 对于first-party dependency，如果只是添加using statement并没有使用，则CLR不会load dll
* .Net Framework永远会加载
* 第三方类库
  * 最简单的方法是使用visual studio项目下的Dependencies文件夹可以方便地添加dependency，并且可以将其加入source control
  * 推荐使用NuGet依赖管理工具

不能循环依赖

#### Managing dependencies

Assembly Loading Process

![image-20211221153832659](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211221153832659.png)



#### Managing Services

* Known endpoints (Know binding type and address)
  * Add reference using Visual Studio
  * Use `ChannelFactory`
* Service discovery (Know binding type but not know address)
  * managed (通过单一的注册中心去发现服务)
    * single point of failure
  * ad hoc (所有service监听一个ip地址，获取请求并响应)
* RESTful services
  * Representational State Transfer (表现层状态转移)
  * Low dependencies



#### Dependency management with NuGet

* 依赖管理工具
* 下载依赖以及其所需要的其他传递依赖
* 控制依赖的版本



### Layering

* solution的复杂度决定层数，问题的复杂度决定solution的复杂度

* Two layers
  * UI layer
    * 提供一种让用户与软件交互的方式
    * 展示数据
    * 接收用户请求
    * 验证用户输入数据
  * Data Access layer
    * 查询数据
    * 序列化和反序列化object models to and from a relational model
  
* Three layers
  * UI layer
  * Business logic layer
    * Handle commands from UI
    * Model the business domain to capture business processes, rules and workflow
  * Data Access layer
  * 虽然是三层架构，但是一般部署在两台机器上（two tiers）
    * 一台UI和logic
    * 一台 Data access layer
  * 当业务逻辑比较复杂或者多变时，推荐使用三层架构

* 对于一些cross-layering的function，比如权限、安全、日志等
  * 推荐使用AOP，防止重复代码污染项目
  
* Asymmetric layering  不对称分层

  * 不同请求的路径（经过的layer）是不同的

  * Command / Query Responsibility Segragation (CQRS)

    * all object methods are one of : Command or Query
    * Command / Query Separation

    ```
    Command: Commands are imperative calls to action, requiring the code to do something. These methods are allowed to change the state of a system but should not also return a value.
    Query: Queries are requests for data, requiring the code to get something. These methods return data to calling clients but should not also change the state of a system.
    ```

    * Query的call顺序可以调整，因为其不会影响系统状态，但是Command的call顺序是需要额外注意的
    * Query是fast，只需要少量事务（一般是文档存储数据库，允许脏读或幻影读）
    * Command需要事务一致性（数据库满足ACID）
    * 一般来说，是Command的数据库通过event异步更新文档存储数据库

如果domain model层出现Exception，不能直接让service抛给controller，这样会导致controller了解domain model的细节，从而让它们有了依赖。**比较好的做法是，让domain model抛出DomainException，service抛出ServiceException**



## Interfaces and Design Patterns

### Adaptive design patterns

#### Null Object Pattern

不同于返回null值，而是返回一个Null Object（实现了接口，但是方法体是空的），避免重复的null reference check

Null Object的方法调用返回还是其他的Null Object

C#6可以使用`?.`运算符

不要使用`boolean IsNull`来指示一个对象是否为空



#### Adapter Pattern

把一个类的接口变换成客户端所期待的另一种接口，从而使原本接口不匹配而无法一起工作的两个类能够在一起工作

##### Class Adapter Pattern

![image-20211223210838238](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211223210838238.png)

##### Object Adapter Pattern

![image-20211223210913540](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211223210913540.png)



#### Strategy Pattern

根据对象的state转变对象的行为



### Duck-typing

> When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck.

As long as an object exhibits the behavior of a certain interface, it should be treated as that interface.

使用`dynamic`关键字来表示方法参数，这样拥有相同方法签名（不一定实现同一接口）的对象都可以被传入

或者使用`Impropmptu.ActLike<IDuck>(Swam)`(将Swam作为属性，IDuck的方法都委托给Swam)  （运行时的Object Adapter  Pattern）



### Mixins

将一个对象拥有多个其他对象的方法

使用扩展方法  （不易mock，不能有实例属性）

使用Re-motion



### Fluent Interfaces

An interface is said to be fluent if it returns itself from one or more of its methods.



## Testing

### UT

#### Arrange Act Assert



#### TDD

Red, Green, Refactor!

1. Write a failing test that targets the expected behavior of the SUT. 
2. Implement just enough of the SUT so that the new test passes without breaking existing  successful tests. 
3. If any refactoring can be done on the SUT to improve its design or overall quality, now is the  time to do so.



#### Testing all control flows



### UT patterns

#### Writing maintainable tests



#### Maintain a reasonable test fixture per class ratio

##### Good naming objects and methods

需要可读性高，主要是开发团队内部维护



#### Builder pattern for tests

把SUT的建造通过Builder Pattern去构造

The builder has provided test writers with a domain-specific language (DSL)  with which to construct tests.



#### Writing tests first

##### Test-driven design

1. Write exactly one new test. It should be the smallest test which seems to point in the direction  of a solution.
2. Run the test to make sure it fails.
3. Make the test from (1) pass by writing the least amount of implementation code you can IN  THE TEST METHOD
4. Refactor to remove duplication or otherwise as required to improve the design. Be strict about  the refactorings. Only introduce new abstractions (methods, classes, etc) when they will help to improve the design of the code. Specifically:
   1. ONLY Extract a new method if there is sufficient code duplication in the test methods.  When extracting a method, initially extract it to the test class (don’t create a new class yet).
   2. ONLY create a new class when a clear grouping of methods emerges and when the test  class starts to feel crowded or too large
5. Repeat the process by writing another test (go back to step 1)

防止Over-engineering



##### Test-first development

TDD produces just enough design; TFD produces whatever design is in the  developer’s head.

TFD与TDD不同之处在于，TDD包含设计，不只是在编写测试的过程中，只注重如何实现



#### Further testing

##### Testing pyramid

> Shows the relative number of tests that you should aim to maintain

![image-20211225110326417](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211225110326417.png)

Manual tests: 少量，因为costly、费时，一般是真实用户的测试

Acceptance tests验收测试: 模拟用户交互，并assert，自动化的，测试环境接近真实环境

Integration tests集成测试: 测试API是否正常，黑盒测试



##### Testing quadrant

> Shows that tests can be useful to different stakeholders and for different  reasons

![image-20211225111338446](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211225111338446.png)



#### Testing for prevention and cure

![image-20211225112053736](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211225112053736.png)

Increase the confidence and decrease the efforts.

MTBF (Mean time before failure)

MTTR (Mean time to recovery)

focus on MTTR more than MTBF

To decrease MTTR, logging (passive) and alerting (active).

Alert在metric触碰到一些预先设置的边界时，报警（比如service throughout服务吞吐量, response time响应时间，machine resource usage 机器资源使用率）



## Refactoring

> Refactoring is the process of incrementally improving the design of existing code.

不改变程序行为，而改变程序的内部arrangement of code

#### Replacing "magic numbers" with constants

给一些常量定义一些好的变量名去指向它们

#### Replacing a conditional expression with polymorphism

把if语句或者switch语句的每一段分成一个子类

#### Replacing a constructor with a factory method

静态工厂模式

#### Replacing a constructor with a factory class

工厂方法模式

#### Replacing inheritance with delegation



### Aggressive Refactoring

#### Red, green, refactor...redesign



#### Making legacy code adaptive

legacy code: 没有UT覆盖的代码

可以编写一些capture main behavior的UT

* Characterization tests
  * 对一些有限的输入进行测试得到输出
* A golden master
  * 将Characterization tests的输入输出对作为期望值编写测试



## SOLID原则

### 单一职责原则SRP

1. Extract methods  (Refactor for clarity)
2. Split each responsibilities into different classes
3. Place them behind interfaces (Refactor for abstraction)



#### Decorator Pattern装饰者模式

![image-20211226090623928](C:\zhan\Note\编程语言\image-20211226090623928.png)

```c#
public interface IComponent
{
 void Something();
}
// . . . 
public class ConcreteComponent : IComponent
{
 public void Something()
 {
 
 }
}
// . . .
public class DecoratorComponent : IComponent
{
 private readonly IComponent decoratedComponent;
 public DecoratorComponent(IComponent decoratedComponent)
 {
 this.decoratedComponent = decoratedComponent;
 }
 public void Something()
 {
 SomethingElse();
 decoratedComponent.Something();
 }
 private void SomethingElse()
 {
 } 
}
// . . .
class Program
{
 static IComponent component;
 static void Main(string[] args)
 {
 component = new DecoratorComponent(new ConcreteComponent());
 component.Something();
 }
}
```



#### Composite Pattern组合模式

![image-20211226090759300](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211226090759300.png)

Composite拥有一个`List<IComponent>`字段，并且也实现了`IComponent`，这样客户端可以通过操作一个Composite而去操作多个Leaf

```c#
public interface IComponent
{
 void Something();
}
// . . .
public class Leaf : IComponent
{
 public void Something()
 {
 
 }
}
// . . .
public class CompositeComponent : IComponent
{
 public CompositeComponent()
 {
 children = new List<IComponent>();
 }
public void AddComponent(IComponent component)
 {
 children.Add(component);
 }
 public void RemoveComponent(IComponent component)
 {
 children.Remove(component);
 }
 public void Something()
 {
 foreach(var child in children)
 {
 child.Something();
 }
 }
 private ICollection<IComponent> children;
}
// . . .
class Program
{
 static void Main(string[] args)
 {
 var composite = new CompositeComponent();
 composite.AddComponent(new Leaf());
 composite.AddComponent(new Leaf());
 composite.AddComponent(new Leaf());
 component = composite;
 component.Something();
 }
 static IComponent component;
}
```

![image-20211226091429409](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211226091429409.png)



#### Predicate Decorators

```c#
public class DateTester
{
 public bool TodayIsAnEvenDayOfTheMonth
 {
 get
 {
 return DateTime.Now.Day % 2 == 0;
 }
 }
}
// . . .
class PredicatedDecoratorExample
{
 public PredicatedDecoratorExample(IComponent component)
 {
 this.component = component;
 }
 public void Run(DateTester dateTester)
 {
 if (dateTester.TodayIsAnEvenDayOfTheMonth)
 {
 component.Something();
 }
 }
 private readonly IComponent component;
}
```



#### Branching Decorators

是Predicate Decorators的延伸，即在条件为True的时候执行TrueComponent，为false的时候执行FalseComponent



#### Lazy Decorators

内部被装饰的接口直到使用的使用才会被创建

使用类型`Lazy<T>`



#### Logging Decorators

在方法外面包一层logging的代码

更好的方式是AOP



#### Profiling Decorators

使用StopWatch测量被装饰方法的运行时间`StopWatch.ElapsedMilliseconds`



#### Decorating properties

```c#
public class ComponentDecorator : IComponent
{
 public ComponentDecorator(IComponent decoratedComponent)
 {
 this.decoratedComponent = decoratedComponent;
 }
public string Property
 {
 get
 {
 // We can do some mutation here after retrieving the value
 return decoratedComponent.Property;
 }
 set
 {
 // And/or here, before we set the value
 decoratedComponent.Property = value;
 }
 }
 private readonly IComponent decoratedComponent;
}
```



#### Decorating Events

```c#
public class ComponentDecorator : IComponent
{
 public ComponentDecorator(IComponent decoratedComponent)
 {
 this.decoratedComponent = decoratedComponent;
 }
 public event EventHandler Event
 {
 add
 {
 // We can do something here, when the event handler is registered 
 decoratedComponent.Event += value;
 }
 remove 
 {
 // And/or here, when the event handler is deregistered
 decoratedComponent.Event -= value;
 }
 }
 private readonly IComponent decoratedComponent;
}
```



### 开闭原则 OCP

> Software entities should be open for extension, but closed for modification.
>
> “Open for extension.” This means that the behavior of the module can be extended.  As the requirements of the application change, we are able to extend the module  with new behaviors that satisfy those changes. In other words, we are able to  change what the module does.
>
> “Closed for modification.” Extending the behavior of a module does not result  in changes to the source or binary code of the module. The binary executable  version of the module, whether in a linkable library, a DLL, or a Java .jar, remains  untouched.

#### Extension points

##### Virtual methods

任何virtual的成员都是open for extension

##### Abstract methods

与Virtual methods用法相同，只是依赖于抽象

可以与Template Method Pattern模板方法一起使用

##### Interface inheritance



#### protected variation

> 受保护的修改
>
> Identify points of predicted variation and create a stable interface around them.

##### Predicted variation

he predicted variation guideline states that you should be explicit in what you allow and disallow to be extended.



##### Speculative generality

The speculative generality guideline states that you should beware of trying to  preempt how a class might apply to a general problem, lest you create a leaky abstraction.



### 里氏替换原则 LSP

> If S is a subtype of T, then objects of type T may be replaced with objects of type S,  without breaking the program.

#### Liskov contract rules

* Preconditions cannot be strengthened in a subtype.
  * 卫语句（方法开始）
  * 通过`System.Diagnostics.Contracts`的`Contract.Requires<XxxException>(bool)`替代卫语句，不满足时会抛出XxxException
* Postconditions cannot be weakened in a subtype. 
  * 卫语句（方法返回值前）
  * `Contract.Ensure(Contract.Result<ReturnType>() != -1)`  确保当前method的返回值不等于-1
* Invariants of the supertype must be preserved in a subtype
  * A data invariant is a predicate that remains true for the  lifetime of an object; it is true after construction and must remain true until the object is out of scope. 
  * 对象的状态不变
    * 通过构造函数控制
    * 通过setter控制
    
  * 通过`Contract.Invariant(bool, errorMsg)`和`[ContractInvariantMethod]属性`

#### Covariance and contravariance

covariance: 用子类代替父类

contravariance: 用父类代替子类

* There must be contravariance of the method arguments in the subtype
* There must be covariance of the return types in the subtype
* No new exceptions are allowed



### 接口隔离原则 ISP

#### Reason to split interfaces

* Decorator
* Client need
* Architectural design

一种选择是接口隔离，但是实现类将一系列接口都实现（因为面对接口编程，client只能看到该接口的实现方法）



### 依赖倒置原则 DIP

> * High-level modules should not depend on low-level modules. Both should depend on abstractions
>
> * Abstractions should not depend on details. Details should depend on abstractions.

#### Entourage anti-pattern

#### Stairway pattern

![image-20220102090552610](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20220102090552610.png)



## Apply Adaptive Code

### Dependency Injection

#### Constructing object graph

* Poor Man's Dependency Injection

  * 手动在entry point处一次性创建所有需要的对象
  * 适用于小的程序
  * Constructor injection
  * Method injection
    * 当依赖项只需要使用一次时
  * Property injection
    * 当依赖项需要在runtime中进行转变

* Inversion of Control

  * IOC container: Link the interfaces of app to their implementations and retrieve an instance of a class by resolving all its dependencies

* Responsibility Owner Factory

  * 使用`try/finally`代替`using`语句块

* Factory Isolation Pattern

  * 将对象的作用域放至lambda表达式内

  ```c#
  public class IsolationConnectionFactory : IConnectionIsolationFactory
  {
   public void With(Action<IDbConnection> do)
   {
   using(var connection = CreateConnection())
   {
   do(connection);
   }
   }
  }
  ```



#### Composition Root

the only location to have knowledge of all interfaces and classes

一个类似的名词是`Resolution Root`

这个是将所有的top-level object registered到一起的container

> ASP.NET MVC的resolution root是controller



#### Convention over configuration

一次性register所有的实现类以及对应的具体接口

```c#
public partial class App : Application
{
 private void OnApplicationStartup(object sender, StartupEventArgs e)
 {
     CreateMappings();
     container = new UnityContainer();
     container.RegisterTypes(
     AllClasses.FromAssembliesInBasePath().Where(type => 
     type.Assembly.FullName.StartsWith(MatchingAssemblyPrefix)),
     UserDefinedInterfaces, 
     WithName.Default
     );
     MainWindow = container.Resolve<TaskListView>();
     MainWindow.Show();
     	((TaskListController)MainWindow.DataContext).OnLoad();
 }
 private IEnumerable<Type> UserDefinedInterfaces(Type implementingType)
 {
     return WithMappings.FromAllInterfaces(implementingType)
     .Where(iface => iface.Assembly.FullName.StartsWith(MatchingAssemblyPrefix));
 }
}
```



![image-20220102153813218](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20220102153813218.png)



#### Illegitimate Injection

有构造函数注入的同时存在一些构造函数，并且它们使用new创建对象

is anti-pattern



### Coupling, cohesion, and connascence

> 耦合、内聚和关联

Adaptive code: Low coupling and high cohesion

Coupling determines the effect that changes to software elements have on one another.

Cohesion measures the contextual relationship between variables in a method, methods in a class,  classes in a module, modules in a solution, solutions in a system, and subsystems in a system.

Connascence is the complexity of dependencies in an object-oriented software system. 



## Continuous Integration

![image-20220102193621217](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20220102193621217.png)

