## 可编程对象

### 变量和批

* Go命令是客户端命令，不是TSQL服务器命令
* Go n 表示将batch执行n次
* batch是一个单元分析和执行的命令组
  * batch中出现了语法错误，则整个batch都不会执行
* batch是一个解析单元
  * 当用户在同一个batch中进行架构更改并操作该对象，会报错，因为此时SQL Server还不知道架构的更改

### 流元素

#### if else

注意：TSQL是三值逻辑，在判断条件为false或unknown的时候，执行else语句

```sql
if year(sysdatetime()) <> YEAR(DATEADD(day, 1, sysdatetime()))
	print N'今天是一年的最后一天';
else 
	print N'今天不是一年的最后一天';
```

```sql
if year(sysdatetime()) <> year(dateadd(day, 1, sysdatetime()))
	print N'今天是一年的最后一天';
else
	if month(sysdatetime()) <> month(dateadd(day, 1, sysdatetime()))
		print N'今天是月的最后一天，但不是年的最后一天';
	else
		print N'今天不是月的最后一天';
```



#### while

* 跳出循环使用break
* 跳过当前循环使用continue

```sql
declare @i as int = 1;
while @i < 10
begin
    if @i > 6 break;
	print @i;
	SET @i = @i + 1;
end
```



### 游标 cursor

使用游标的步骤

1. 基于查询声明游标
2. 打开游标
3. 从第一个游标记录提取属性值给变量
4. 遍历游标记录，直至到达游标的末尾（@@FETCH_STATUS函数值为0时）；在循环的每次迭代中，从游标的当前记录中提取属性值给变量，并为行执行所需的处理
5. 关闭游标
6. 释放游标

template

```sql
-- 1. 声明游标
declare [cursor_name] cursor
for select_statement
-- 2. 打开游标
open [cursor_name]
-- 3. 使用游标
fetch next from [cursor_name] into [variable_name, ...]
-- 4. 关闭游标
close [cursor_name]
-- 5. 释放游标
deallocate [cursor_name]
```



example

```sql
SET NOCOUNT ON;

DECLARE @Result TABLE -- 声明表变量
(
	custid INT,
	ordermonth datetime,
	qty int,
	runqty int,
	primary key(custid, ordermonth)
);
declare
	@custid as int,
	@prvcustid as int,
	@ordermonth as datetime,
	@qty as int,
	@runqty as int;

declare c cursor fast_forward /* read only, forward only*/ for
	select custid, ordermonth, qty
	from Sales.CustOrders
	order by custid, ordermonth
open c;
fetch next from c into @custid, @ordermonth, @qty;
select @prvcustid = @custid, @runqty = 0;

while @@FETCH_STATUS = 0
begin
	if @custid <> @prvcustid
		select @prvcustid = @custid, @runqty = 0;
	set @runqty = @runqty + @qty;
	insert into @Result values(@custid, @ordermonth, @qty, @runqty);
	fetch next from C into @custid, @ordermonth, @qty;
end

close c;
deallocate c;

select custid, convert(varchar(7), ordermonth, 121) as ordermonth, qty, runqty
from @Result
order by custid, ordermonth;
```

但是，使用游标有以下的问题：

* 游标完全违反了基于集合理论的关系模型
* 游标的性能比基于集合的查询差

上面的example可以使用开窗函数来代替

```sql
select custid, ordermonth, qty, SUM(qty) over (partition by custid
                                              order by ordermonth
                                              ROWS UNBOUNDED PRECEDING) as runqty
from Sales.CustOrders
order by custid, ordermonth;
```

