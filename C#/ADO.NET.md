## SQL Server

### 主键

业务主键

​	有业务意义

逻辑主键

推荐使用逻辑主键

```
当建立主键时，会默认建立索引，而实际数据在磁盘上存储的顺序和主键列的顺序是一致的，因为实际的物理顺序只能有一种，所以一个表中只能有一个主键。
```

### 类型

* bit类型只能用1或0

char(num)

varchar(num)

num最多是8000

nchar(num)

nvarchar(num)

num最多是4000

varchar(max)和nvarchar(max)一般存储较大的文本

### 创建数据库

```sql
create database [数据库名称]
on primary
(
	-- 配置主数据文件的选项
    name="MyDatabaseOne", -- 主数据文件的逻辑名称，一般与数据库名一致
    filename='C:\xxx', --主数据文件的实际保存路径
    size=5MB,
    maxsize=150MB,  -- 不设置默认是不限制增长
    filegrowth=20%
)
log on 
(
	-- 配置日志文件的选项
)
```

### 创建表

自动编号

`EmpId int identity(1,1) primary key`

###  增删改查

启用向自动编号列手动插入值的功能

`set IDENTITY_INSERT [表名] ON`

如果在执行插入的时候，返回自动增加的编号

```sql
-- 返回Person表中自动编号的autoId字段
insert into Person 
output inserted.autoId values (N'周七', 33, 130, 1);
```



truncate删除数据的效率比delete要高

truncate删除时不会触发delete触发器



top获取前几条数据，一般与order by连用，写在列名前面

如果是表达式，要使用括号括起来

top还可以使用`30 percent`得到前30%的数据（进一法取整）



**快速复制一张表的数据**

```sql
select * into NewStudent from student;
-- 将student的表数据复制到NewStudent，如果NewStudent表不存在，则自动创建（约束不会进行复制）
-- 不能重复执行，因为每次执行都会创建表，如果已存在，就报错

select top 0 * into NewTable from OldTable;
-- 只复制表的结构

insert into NewTable
select * from OldTable\
-- NewTable需要提前创建好
```



### 模糊查询

`_`  任意的单个字符

`%`  任意的多个字符

`[...]`  范围，匹配方括号中的任意一个即可  `[a-z]`表示字母

`^`  非

**转义字符**

* 将字符放入`[]`中
* `like '%\%_' ESCAPE '\'`
  * 显式指定转义字符

### SQL语句执行顺序

1. from
2. join
3. on
4. where
5. group by
6. with cube 或 with rollup
7. having
8. select
9. distinct
10. order by
11. top

### 约束

* 非空约束

  * 只有非空约束是通过alter column添加的，其余都是add constraint

  ```sql
  alter table [表名]
  alter column [列名] 数据类型 not null;
  ```

* 主键约束 pk

  ```sql
  alter table [表名]
  add constraint PK_表名_列名 primary key([列名]);
  ```

* 唯一约束 uq

  ```sql
  alter table [表名]
  add constraint UQ_表名_列名 unique([列名]);
  ```

* 默认约束  df

  ```sql
  alter table [表名]
  add constraint DF_表名_列名 default(默认约束的表达式) for [列名];
  ```

* 检查约束  ck

  * 范围以及格式限制

  ```sql
  alter table [表名]
  add constraint CK_表名_列名 check(检查约束表达式)
  ```

* 外键约束  fk

  * 可以设置是否级联删除

  ```sql
  alter table [表名]
  add constraint FK_外键表_主键表 foreign key([列名]) references [主键表]([主键表的主键列]) [on delete cascade];
  ```

* 删除约束

  * 可以一次删除多个约束

  ```sql
  alter table [表名]
  drop constraint [约束名1], [约束名2];
  ```

  

添加一列

```sql
alter table [表名]
add [列名] 数据类型;
```

修改表中一列的数据类型

```sql
alter table [表名]
alter column [列名] 新数据类型;
```



删除表中一列

```sql
alter table [表名] 
drop column [列名];
```

### union

union用来合并相同数目的结果集或表达式，默认会去除重复数据，而union all不会

推荐使用union all

### 聚合函数

* min
* max
* avg
* sum
* count

聚合函数不对null值进行计算



### SQL 函数

| 函数                                      | 左右                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| len(str)                                  | 获取str的字符个数                                            |
| datelength(str)                           | 返回字符的字节个数（英文占1个，中文字符占两个）（如果是N申明的Unicode字符串，统一占两个） |
| upper(str), lower(str)                    | 将字符串转换为大写，小写                                     |
| ltrim(str), rtrim(str)                    | 去掉左右两端空格                                             |
| left(str, num), right(str, num)           | 从左/右边开始截取num个字符                                   |
| substring(str, start, len)                | 将字符串从start位置开始截取len个字符（起始索引是1）          |
| getdate                                   | 获取当前日期                                                 |
| sysdatetime()                             | 获取当前系统时间（精度更高）                                 |
| dateadd(datepart,num,date)                | 计算增加以后的日期（dateadd(DAY,3,date) 返回date三天后的日期） |
| datediff(datepart, '1999-3-1', getdate()) | 根据计量单位计算两个日期的差                                 |
| datepart(year, date)                      | 获取date的年部分（根据不同的计量单位获取不同的部分）         |
| datename(month, date)                     | 与datepart函数作用相同，但是返回name形式（比如datename(month, '20090212'）的返回值是February，返回年份就是2009的字符串形式） |
| year(getdate())                           | 获取当前时间的年部分(还有month和day)                         |
| replace(str1,str2,str3)                   | 将字符串str1中匹配str2的子串替换为str3                       |
| cast(表达式 as 数据类型)                  | 将表达式转换为指定的数据类型                                 |
| convert(数据类型, 表达式[, style])        | 将表达式转换为指定的数据类型（style在转换日期的时候会用到）  |
| coalesce(str1, str2, ...)                 | 返回第一个非NULL字符串                                       |
| concat(str1, str2, ...)                   | 将字符串连接起来，如果为NULL，则用''代替                     |

### 连接

#### 内连接

```sql
select * from A inner join B on A.id = B.id;
```

#### 外连接

* 左外连接
  * left outer join
* 右外连接
  * right outer join



### Case查询

**语法**

```sql
case (expression)  --expression是optional
when value_1 then returnVar_1
when value_2 then returnVar_2
when value_3 then returnVar_3
else defaultVal
end
```

* 要求then之后的数据类型需要一致

### 索引

> 索引的目的：提高查询效率
>
> 增加索引后，会增加额外的存储空间，同时，也降低了增加新纪录，修改，删除的效率

* 聚集索引

  * 索引本身的存储顺序和磁盘上的存储顺序一致（物理）

  ```sql
  create unique clustered index [索引名] on [表名](列名);
  ```

  * 

* 非聚集索引

  * （逻辑）

  ```sql
  create nonclustered index [索引名] on [表名]([列名]);
  ```

  * 可以指定多个列

一个表中只能有一个聚集索引，但是可以有多个非聚集索引

**删除索引**

```sql
drop index [索引名];
```

#### 读取

* 逻辑读取
  * 从缓存中读取数据
* 物理读取
  * 从磁盘上读取数据
* 预读取
  * 一种性能优化机制，在执行查询之前先预测执行“查询计划”所需的数据和索引页，然后在查询实际使用这些页之前读入缓冲区高速缓存

### 子查询

* 相关子查询

  ```sql
  select * from TblStudent as ts 
  where exists (select * from TblClass as tc where ts.tClassId=tc.tClassId and tc.tClassName='高三二班')
  ```

* 独立子查询

  ```sql
  select * from TblStudent where tSClassId=(select * from TblClass where tClassName='高三二班')
  ```

### 分页

> sql server 2000以前使用top实现
>
> 之后使用`Row_Number()`实现

row_number不可以使用在where子句中

```sql
-- 返回student表中按照sId升序排列后的第6-10行数据
select * 
from (select row_number() over (order by sId asc) as num, * from student) as s
where s.num between 6 and 10 
order by sId asc;
```



### 视图

> 视图是一张虚拟表，表示一张表的部分数据或多张表的综合数据，其结构和数据是建立在对表的查询基础上
>
> 视图在操作上和数据表没有什么区别，但两者的差异是：数据表是实际存储数据的地方，然而视图并不保存任何记录
>
> 视图的目的是方便查询，所以一般情况下不对视图进行增删改
>
> 调用视图，是调用存储在视图内的查询语句

**优点**

* 筛选表中的行、降低数据库的复杂程度
* 防止未经许可的用户访问敏感数据

**创建视图**

```sql
create view VW_视图名
as
(查询语句)
```

注意事项

* 如果视图中的查询语句中包含了重名的列，此时必须为重名的列起别名
  * 所有查询的列，必须有列名，且列名唯一
* 视图中的查询不能使用order by子句，除非指定了top语句
  * 视图被认为是一个虚拟表，表是一个集合，是不能有顺序的，而order by则返回的是一个有顺序的，是一个游标
  * 在视图中使用select top percent + order by问题

* create view 后不能跟 begin end

索引视图（对视图建立索引）会在视图上创建唯一聚集索引，数据会保存在数据库，而不是引用表的数据

索引视图注意事项：对视图创建的第一个索引必须是唯一聚集索引

### T-SQL编程

**声明局部变量**

变量的初始值是Null，所以必须赋初值

```sql
declare @变量名 数据类型[=默认值]
-- 一次声明多个变量
declare @变量名 数据类型, @变量名 数据类型
```

**声明全局变量**

全局变量由系统定义和维护，只能读取，不能修改全局变量的值

```sql
@@全局变量名
```

| 变量              | 含义                         |
| ----------------- | ---------------------------- |
| @@ERROR           | 最后一个T-SQL错误的错误号    |
| @@IDENTITY        | 最后一次插入的标识值         |
| @@LANGUAGE        | 当前使用的语言的名称         |
| @@MAX_CONNECTIONS | 可以创建的同时连接的最大数目 |
| @@ROWCOUNT        | 受上一个SQL语句影响的行数    |
| @@SERVERNAME      | 本地服务器的名称             |

**赋值**

```sql
set @变量名=值 			-- set用于普通的赋值
select @变量名=值		-- 用于从表中查询数据并赋值
```

**Example**

```sql
declare @name varchar(20)
declare @id int
set @name='张三'
set @id=1
select @name=sName from student where sId=@id
```

**while循环**

```sql
declar @i int=1
while @i<=100
begin
	print 'Hello'
	set @i=@i+1
end
```

* break跳出当前循环
* continue跳过本次循环

**if-else**

```sql
declare @n int=10
if @n>10
begin
	print '@n大于10'
end
else if @n>5
begin
	print '@n大于5'
end
else
begin
	print '@n小于5'
end
```

### 事务

```sql
begin transaction
declare @sum int=0
  要执行的sql语句1
  set @sum=@sum+@@error
  要执行的sql语句2
  set @sum=@sum+@@error
  ...
  if @sum = 0
  begin
  	commit
  end
  else
  begin
  	rollback
  end
```

**三种事务**

* 自动提交事务

  * 当执行一条sql语句时，数据库自动帮我们打开一个事务，当语句执行成功的时候，数据库自动提交事务，执行失败，数据库自动回滚事务
  * SQL server默认的事务状态

* 隐式事务

  * 每次执行sql语句，数据库自动打开一个事务，但此时需要手动提交或者回滚

  ```sql
  set implicit_transactions {on | off}
  ```

* 显式事务

  * 需要手动打开事务，手动提交或者回滚

acid

* 原子性 atomicity
* 一致性 consistency
* 隔离性 isolation
* 永久性 durability



### 存储过程

> 类似于编程语言的函数，由存储过程名/存储过程参数组成/可以有返回结果
>
> 存储过程可以返回多个结果

**优点**

* 执行速度更快
  * 在数据库中保存的存储过程语句都是编译过的
* 允许模块化程序设计
  * 类似方法的复用
* 提高系统安全性
  * 防止SQL注入
* 减少网络流通量
  * 只需要传输存储过程的名字

**系统的存储过程**

* 由系统定义，存放在master数据库
* 名称以`sp_`开头或者是`xp_`开头，自定义的存储过程可以用`usp_`开头

| 系统存储过程         | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| sp_databases         | 列出服务器上所有的数据库                                     |
| sp_helpdb            | 报告有关指定数据库或者所有数据库的信息                       |
| sp_renamedb          | 更改数据库的名称                                             |
| sp_tables            | 返回当前环境下可查询的对象的列表                             |
| sp_columns           | 回某个表列的信息 exec sp_columns '表名'                      |
| sp_help              | 查看某个表的约束                                             |
| sp_helpconstraint    | 查看某个表的约束                                             |
| sp_stored_procedures | 列出当前环境中的所有存储过程                                 |
| sp_password          | 添加或修改登录账户的密码                                     |
| sp_helptext          | 显示默认值、未加密的存储过程、用户定义的存储过程、触发器或视图的实际文本  exec sp_helptext 'sp_databases' |

**创建/修改存储过程**

```sql
create(alter) procedure usp_存储过程名
@参数1 数据类型[=默认值],
@参数2 数据类型[=默认值],
@参数3 数据类型 output -- 输出参数
as
begin
	存储过程体
end
```

调用带有输出参数的存储过程的时候，需要定义变量，将变量传递给输出参数，在存储过程中使用的输出参数，其实就是你传递进来的变量

**执行存储过程**

```sql
exec 存储过程名 参数1, 参数2
-- 另一种形式
exec 存储过程名 @参数名=参数值, @参数名=@输出参数变量
```

**删除存储过程**

```sql
drop procedure 存储过程名
```

**Example**

```sql
-- 分页的存储过程
create procedure usp_person_pagination
@pagesize int=3,
@pageindex int=1
as
begin
	select * from (select *, rn=(row_number() over (order by autoId)) from Person) as temp
	where temp.rn between (@pageindex - 1) * @pagesize + 1 and @pageindex * @pagesize 
end
```

### 触发器

> 触发器的作用：自动化操作，减少手动操作以及出错的几率

触发器是一种特殊类型的存储过程，它不同于一般的存储过程（在sql内部把触发器当做是存储过程但是不能传递参数），在SQL Server里面是对某一个表的一定的操作，触发某种条件，从而执行的一段程序

一般的存储过程通过存储过程名直接调用，而触发器主要是通过事件进行触发而被执行

触发器是一个功能强大的工具，在表中数据发生变化时自动强制执行

触发器可以用于SQL Server约束，默认值和规则的完整性检查，还可以完成普通约束难以实现的复杂约束

#### 常见的触发器

* DML触发器
  * Insert、delete、update（不支持select）
  * after触发器，instead of触发器（不支持before触发器）
* DDL触发器
  * Create table、create database、after、drop...

**after触发器**

在语句执行完毕之后触发

按语句触发，而不是受影响的行数，无论影响多少行，只触发一次

只能建立在常规表上，不能建立在视图和临时表上

可以递归触发，最高可达32级

update(列)，在update语句触发时，判断某列是否被更新，返回布尔值

**instead触发器**

用于替换原本的操作

不会递归触发

可以在约束被检查之前触发

可以建在表或视图上

#### inserted和deleted

* inserted表包含新数据
  * insert、update触发器会用到
* deleted表包含旧数据
  * delete、update触发器会用到

#### 常用语法

```sql
create trigger [触发器名] on [表名]
after(for) -- for与after都表示after触发器
-- 或者可以是
instead of 
Update | Insert | Delete
-- 如果三种操作都有
(insert, update, delete)
as
begin

end
```

Example

```sql
create trigger trigger_insert_backup on person
after insert
as
begin
	insert into person_backup(uName, age, height, gender) 
	select uName, age, height, gender from inserted
end
```



#### 触发器使用建议

* 尽量避免在触发器中执行耗时操作，因为触发器会与sql语句认为在同一事务中（事务不结束，就无法释放锁）
* 避免在触发器中做复杂操作，影响触发器性能的因素比较多（如：产品版本、所使用的架构等），要想编写高效的触发器考虑因素比较多（编写触发器容易，编写复杂高性能的触发器难）
* 触发器编写时注意对多行触发时的处理（一般不建议使用游标，性能问题）



### 用户定义函数

> 优点
>
> * 允许模块化编程设计
> * 执行速度更快
> * 减少网络流量

> 用户定义函数分为两类
>
> * 标量函数
>   * 返回标量值，即返回一个单一数据值
> * 表值函数
>   * 返回表值，返回值不是单一数据值，而是由一个表值代表的记录集，即返回table数据类型
>   * 分类
>     * 内联表值函数：在return子句中包含单个select语句
>     * 多语句表值函数：在begin-end语句块中包含多个select语句

#### 标量函数

**定义**

```sql
create function [schema_name.]function_name   -- 函数名
([@parameter_name [AS] [type_schema_name.]parameter_type [=default] [Readonly]] [,...]) -- 形式参数
returns return_data_type   -- 返回参数的类型
[with <function_option> [, ...]]  -- 函数选项定义
[AS]
begin
	函数体
 	return scalar_expression    -- return必须出现在函数体的最后一句
end

-- 其中function_option
<function_option>::=
{
	[Encryption] | [schemabinding] | [return null on null input | called on null input]
}

-- 调用1
select 架构名.函数名(实参1, ...)
-- 调用2
exec 变量名=架构名.函数名 实参1,...
```

**example**

```sql
create function HelloWorld() 
returns nvarchar(22)
as
begin
	return 'Hello World'
end

print dbo.HelloWorld()  -- 调用的时候需要指定dbo(built-in functions可以不用dbo.)

create function AddTwoNums(
 @num_1 int
,@num_2 int)
returns int
as
begin
	return @num_1 + @num_2
end
```



#### 内联表值函数

```sql
create function [schema_name.]function_name  -- 函数名
([{@parameter_name [AS] [type_schema_name.]parameter_type[=default]}] [, ...])  				-- 定义参数部分
returns table			-- 返回值为table类型
[with <function_option> [,...]]  -- 定义函数的可选项
[AS]
return [(] select语句 [)]   		-- 通过select语句返回内嵌表
         
-- 内联表值函数的调用
select * from [schema_name.]function_name(参数)
```

example

```sql
create function stu_sco(@pr char(12))
returns table
as
return (select a.stname, a.stsex, b.cno, b.grade
        from student a, score b
       where a.stno = b.stno and a.speciality=@pr)
```



#### 多语句表值函数

```sql
create function [schema_name.]function_name  -- 函数名
([{@parameter_name [AS] [type_schema_name.]parameter_type[=default]}] [, ...])  				-- 定义参数部分
returns @return_variable Table<table_type_definition>  -- 定义作为返回值的表
[with <function_option> [,...]]
[as]
begin
	function_body
	return
end

-- 其中table_type_definition是定义表结构
({column_definition <column_constraint>} 
 [table_constraint])
```

example

```sql
create function stu_sco2(@num char(6))
returns @tn table (average float)
as
begin
	insert into @tn
	select avg(score.grade)
	from score
	where score.stno = @num
end
```





## 系统的一些函数、视图、表

### 内置函数

| Func                          | Description                                 |
| ----------------------------- | ------------------------------------------- |
| iif(condition, expr_1, expr2) | condition为true，返回expr_1，否则返回expr_2 |
|                               |                                             |
|                               |                                             |



### sys.objects

| Type | Description                |
| ---- | -------------------------- |
| C    | Check约束                  |
| D    | Default约束                |
| F    | Foreign Key约束            |
| FN   | 标量函数                   |
| IF   | 内嵌表函数                 |
|      | Primary Key或者 Unique约束 |
| L    | 日志                       |
| P    | 存储过程                   |
| R    | 规则                       |
| RF   | 复制筛选存储过程           |
| S    | 系统表                     |
| TF   | 表值函数                   |
| TR   | 触发器                     |
| U    | 用户表                     |
| V    | 视图                       |
| X    | 扩展存储过程               |





## ADO.NET

### 对象

| 对象        | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| Connection  | 连接数据库                                                   |
| Command     | 执行sql语句                                                  |
| DateReader  | 只读、只进的结果集（一条一条读取数据，与StreamReader，XmlRe4ader类似） |
| DateAdapter | 一个封装了上面三个对象                                       |
| DateSet     | 临时数据库                                                   |

### 连接数据库

```c#
// 连接数据库的步骤
// 1. 创建连接字符串  连接字符串的写法有多种
// Data Source=服务器/实例  (默认实例不需要写)
// Initial Catalog=数据库名称
// Integrated Security=True (True表示Windows身份验证)
// string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;Integrated Security=True";
// 如果使用用户名密码
string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;User ID=sa;Password=InforOn1y";

// 2. 创建连接对象
using (SqlConnection conn = new SqlConnection(config))
{
    // 3. 打开连接
    conn.Open();

    // 4. 关闭连接，释放资源
    // conn.Close();  // Close方法在Dispose方法里隐式调用了
    // conn.Dispose();  using语句会隐式调用dispose
}
```

### 增删改查

SqlCommand对象

* cmd.ExecuteNonQuery()
  * 执行insert/update/delete操作
  * 返回值是int类型，表示受影响的行数（但执行其他语句，返回-1）
* object cmd.ExecuteScalar()
  * 返回单行单列的结果，使用该方法
* SqlReader cmd.ExecuteReader()
  * 返回多行，多列的结果
  * SqlReader需要关闭
  * 如果希望关闭SqlReader的同时，也关闭SqlConnection，需要传入参数`System.Data.CommandBehavior.CloseConnection`

```c#
// 1. 连接字符串
string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;" +
    "User Id=sa;Password=。。。";
// 2. 创建连接对象
using (IDbConnection conn = new SqlConnection(config))
{
    // 3. 编写sql语句
    string sql = "insert into Person values (N'张三', 20, 170, 1)";
    // 4. 创建执行sql语句的command对象
    using (IDbCommand cmd = conn.CreateCommand())
    {
        cmd.CommandText = sql;
        // 5.打开连接
        conn.Open();
        // 6. 执行sql
        int rows = cmd.ExecuteNonQuery();
        Console.WriteLine("Succeed in inserting {0} row", rows);
    }
}
```

### SqlDataReader

> 通过调用ExecuteReader()方法，将给定的sql语句在服务器端执行
>
> 执行完毕后，服务器端就已经查询出了数据。但是此时数据保存在数据库服务器的内存中，并没有返回给应用程序
>
> 只是返回给应用程序一个reader对象，这个对象就是用来获取数据的对象
>
> 默认情况下，DataReader需要独占一个Connection对象

DataReader是索引器

```c#
string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;" +
    "User Id=sa;Password=。。。";
using (SqlConnection conn = new SqlConnection(config))
{
    string sql = "select * from Person";
    using (SqlCommand cmd = new SqlCommand(sql, conn))
    {
        conn.Open();
        using (SqlDataReader reader = cmd.ExecuteReader())
        {
            if (reader.HasRows)
            {
                while (reader.Read())  
                {
                    // 开始指向第一条数据的上一条，每次调用Read方法向下移动一次，如果有数据返回true
                    // FieldCount  获取当前查询语句查询出的列的个数
                    for (int i = 0; i < reader.FieldCount; i++)
                    {
                        // DataReader是索引器，返回该列的结果
                        Console.Write(reader[i] + "\t");
                    }
                    Console.WriteLine();
                }
            }
            else
            {
                Console.WriteLine("No records found");
            }
        }
    }
}
```

DataReader对象

可以通过索引来访问field



有两种索引器

* string name  根据列名来获取
  * 内部实现是调用`GetValue(this.GetOrdinal(name))`
  * `GetOrdinal`方法根据列名获取索引

* int i  根据列的序号来获取
  * 内部实现是调用`GetValue(int i)`方法

通过索引器获取的值是object类型，如果想要获取具体的类型需要调用`GetXxx方法`

但是，如果获取到的值是Null，会产生异常

* reader.ISDBNull(int index)
  * 返回index列对应的数据是否是Null

**注意**

数据库的null值对应的不是C#的Null值，而是`DBNull`类型的Null值，该类型的ToString方法会返回`""`



### 返回多结果集

对于一个command运行之后应该有多个SqlDataReader reader

使用`reader.NextResult()`就可以更新reader为下一个reader



### 带参数的sql

```c#
string config = "...";
using (SqlConnection conn = new SqlConnection(config))
{
    string sql = "select * from users where loginId=@loginId and password=@password";
    using (SqlCommand cmd = new SqlCommand(sql, conn))
    {
        // 创建两个参数对象
        SqlParameter paramLoginId = new SqlParameter("@loginId", SqlDbType.VarChar, 50) {Value="zhangsan"};
        SqlParameter paramPwd = new SqlParameter("@password", SqlDbType.VarChar, 50) {Value="123"};
        // 将参数对象添加到command对象
        cmd.Parameters.Add(paramLoginId);
        cmd.Parameters.Add(paramPwd);
        
        // ... 打开连接，执行sql
    }
}
```

也可以使用`AddParameter(string variable, object value)`，但此时有时会产生错误，会调用另一个重载方法，程序以为是传入了`SqlDbType`值

### 连接池

> 由于每次正常连接数据库，都至少会执行3个操作
>
> 1. 登录数据库服务器
> 2. 执行操作
> 3. 注销用户
>
> 因此，申请连接都比较耗时
>
> ADO.Net默认使用了连接池

如果需要禁用连接池，需要在连接字符串中添加

`Pooling=false`

一般使用“池”的地方会满足两个条件

1. 创建对象比较费时
2. 对象使用比较频繁

作用：提高了创建对象的效率

**连接复用的前提：连接字符串相同**

### 封装增删改查方法

User

```c#
class User
{
    public int Id
    {
        set;
        get;
    }
    public string Username
    {
        set;
        get;
    }
    public string Password
    {
        set;
        get;
    }
    public User(int id, string username, string password)
    {
        this.Id = id;
        this.Username = username;
        this.Password = password;
    }
}
```

RowMapper

```c#
public interface IRowMapper<T>
{
    T GetRowMapper(SqlDataReader reader);
}

class UserRowMapper : IRowMapper<User>
{
    public User GetRowMapper(SqlDataReader reader)
    {
        return new User(reader.GetInt32(0), reader.GetString(1), reader.GetString(2));
    }
}
```

SqlUtils

```c#
public static class SqlUtils
{
    // 读取App.Config配置文件中的连接字符串
    public static readonly string config = ConfigurationManager.ConnectionStrings["configStr"].ConnectionString;

    public static int ExecuteNonQuery(string sql, params SqlParameter[] parameters)
    {
        using (SqlConnection conn = new SqlConnection(config))
        {
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddRange(parameters);
                conn.Open();
                return cmd.ExecuteNonQuery();
            }

        }
    }

    public static object ExecuteScalar(string sql, params SqlParameter[] parameters)
    {
        using (SqlConnection conn = new SqlConnection(config))
        {
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddRange(parameters);
                conn.Open();
                return cmd.ExecuteScalar();
            }
        }
    }

    public static List<T> ExecuteReader<T>(string sql, IRowMapper<T> rowMapper, params object[] parameters)
    {
        List<T> res = new List<T>();
        using (SqlConnection conn = new SqlConnection(config))
        {
            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.Parameters.AddRange(parameters);
                conn.Open();
                SqlDataReader reader = cmd.ExecuteReader();
                if (reader.HasRows)
                {
                    while (reader.Read())
                    {
                        T obj = rowMapper.GetRowMapper(reader);
                        res.Add(obj);
                    }
                }
                return res;
            }
        }
    }
}
```

App.config

配置文件名称必须为`App.config`，如下配置

使用`System.Configuration.ConfigurationManager.ConnectionStrings['名称'].ConnectionString`

```c#
<connectionStrings>
    <add name="configStr" connectionString="Data Source=CNSHNBZHAN01;Initial Catalog=demo;User Id=sa;Password=InforOn1y"/>
</connectionStrings>
```

### SqlDataAdapter

与DataSet和DataTable一起连用

```sql
DataTable dt = new DataTable();

            string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;User id=sa;Password=InforOn1y";
            string sql = "select * from person";
            using (SqlDataAdapter sda = new SqlDataAdapter(sql, config))
            {
                sda.Fill(dt);
            }
            this.dataGridView1.DataSource = dt;
```

### DataSet & DataTable

> DataSet类是ADO.NET的核心成员之一，支持ADO.NET断开式、分布式数据方案的核心对象，也是各种开发基于.Net平台程序语言开发数据库应用程序最常接触的类
>
> DataSet是创建在内存中的集合对象，可以包含任意数量的数据表以及所有表的约束、索引和关系，相当于一个在内存中的小型关系型数据库，如果数据量太大，会非常消耗内存
>
> 每一个DataSet包含一组DataTable对象和DataRelation对象
> 其中每个DataTable对象都由DataColumn、DataRow和Constrain这些对象组成
>
> DataSet可以看做是数据库容器，它将数据库中的数据复制一份放到了用户本地的内存中，供用户在不连接数据库的情况下读取数据，以便充分利用客户端资源，降低数据库服务器的压力

```c#
// 创建一个名为school的内存数据库
DataSet ds = new DataSet("school");

// 创建一个名为student的表
DataTable dt = new DataTable("student");
// 创建一些列
DataColumn dcAutoId = new DataColumn("AutoID", typeof(int));
// 设置自动编号
dcAutoId.AutoIncrement = true;
dcAutoId.AutoIncrementSeed = 1;
dcAutoId.AutoIncrementStep = 1;
dt.Columns.Add(dcAutoId);
// 增加姓名列
DataColumn dcUserName = dt.Columns.Add("UserName", typeof(string));
dcUserName.AllowDBNull = false;  // 不允许为空
dt.Columns.Add("Age", typeof(int));

// 向表中增加一些数据
// 创建DataRow对象
DataRow dr1 = dt.NewRow();
dr1["Username"] = "zhangsan";
dr1["Age"] = 21;
dt.Rows.Add(dr1);
DataRow dr2 = dt.NewRow();
dr2["Username"] = "lisi";
dr2["Age"] = 19;
dt.Rows.Add(dr2);

// 将表添加到数据库中
ds.Tables.Add(dt);

// 遍历所有的表
for (int i = 0; i < ds.Tables.Count; i++)
{
    Console.WriteLine("Table Name：{0}", ds.Tables[i].TableName);
    for (int j = 0; j < ds.Tables[i].Rows.Count; j++)
    {
        DataRow dr = ds.Tables[i].Rows[j]; // 获取表中的每一行
        for (int k = 0; k < ds.Tables[i].Columns.Count; k++)
        {
            Console.Write(dr[k] + "\t");
        }
        Console.WriteLine();
    }
}
```



### ADO执行存储过程

```c#
DataTable dt = new DataTable();

string config = "Data Source=CNSHNBZHAN01;Initial Catalog=demo;" +
    "User id=sa;Password=InforOn1y";
using (SqlDataAdapter sda = new SqlDataAdapter("usp_person_pagination", config))
{
    // 设置CommandType为存储过程
    sda.SelectCommand.CommandType = CommandType.StoredProcedure;
    SqlParameter[] sqlParams =
    {
        new SqlParameter("@pagesize", SqlDbType.Int) {Value=2},
        new SqlParameter("@pageindex", SqlDbType.Int) { Value = 1 },
        // 设置输出参数
        new SqlParameter("@recordcount", SqlDbType.Int){Direction=ParameterDirection.Output}
    };
    sda.SelectCommand.Parameters.AddRange(sqlParams);
    sda.Fill(dt);

    // 获取procedure的输出参数
    object val = sqlParams[2].Value;
    MessageBox.Show(val.ToString());
}
this.dataGridView1.DataSource = dt;
```

注意事项

如果是通过调用Command对象的ExecuteReader()方法执行存储过程，那么想要获取输出参数，就必须等到关闭reader对象之后，才能获取输出参数

### set与select赋值区别

当通过set变量赋值的时候，如果查询语句返回的不止一个值，会报错

当通过select为变量赋值的时候，如果查询语句不止一个值，那么会将最后一个结果赋值给该变量
