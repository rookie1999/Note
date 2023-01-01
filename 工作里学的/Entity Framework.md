## 比较EF Core和EF6

* EF6 designed for .NET Framework, but support .NET
* EF6是stable and supported产品，但是不在actively developed



## EF6

### 安装Entity Framework

如果是SQL Server，只需要安装`Microsoft.EntityFrameworkCore.SqlServer`

安装的同时还会安装`Microsoft.EntityFrameworkCore.Relational`和`Microsoft.Data.SqlClient`等



### 优点

* 不在需要编写SP
* 不在需要manage DB Connections
* 不在需要manual mapping



### WorkFlows

#### Database First

Step:

1. Design tables
2. EF generates domain classes



第一步  design tables

```sql
create table Posts(
PostID int identity primary key,
DatePublished smalldatetime,
Title varchar(500),
Body varchar(8000)
);
```

Tip: 一般来说，在schema change的时候，需要在test db、production db做sync changes，最好的做法是每次进行修改的时候，保存script到git repo。这样如果需要最新的schema，直接运行对应的script（`在MSSSMS中右键对应的table -> Script table as -> Drop to and Create to -> File`）





#### Code First (recommend)

Step:

1. Create domain classes
2. EF generates db tables

优点：

* Full version of DB
* Productivity (use keyboard more)

#### Model First (not recommend)

Step:

1. Create a UML diagram
2. EF generates domain classes and db tables

