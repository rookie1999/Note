## C# optimization strategies

### 1, Bunching

使用场景：When mongoose is calling for an arbitrarily large data set, whether for a UI collection or a report

1. orderby clause需要有列能唯一标识一行记录

   1. ISortOrder
   2. 如果没有，需要增加column
   3. 如果列存在decimal或者Guid，需要添加默认值

2. 给隔离的CRUD方法添加loadType，recordType参数和相应默认值

   1. ```c#
      public ICollectionLoadResponse GetResult(LoadType loadType = LoadType.First, int recordCap = 5000)
      ```

3. 通过IRecordBunch接口实现bunching

   1. ```c#
      public ICollectionLoadResponse GetResult(LoadType loadType = LoadType.First, int recordCap = 5000)
      {
             ICollectionLoadResponse Data = null;
             var Coitem1LoadRequest = collectionLoadRequestFactory.SQLLoad(columnExpressionByColumnName: new Dictionary<string, string>()
             {
                 {"co_num","co_num"},
                 {"co_line","co_line"},
                 {"co_release","co_release"},
                 {"item","item"},
             },
             loadForChange: false,
             lockingType: LockingType.None,
             tableName: "coitem",
             fromClause: collectionLoadRequestFactory.Clause(""),
             whereClause: collectionLoadRequestFactory.Clause(""),
             orderByClause: collectionLoadRequestFactory.Clause("co_num, co_line, co_release"));
       
             Dictionary<string, SortOrderDirection> dicSortOrder = new Dictionary<string, SortOrderDirection>();
             dicSortOrder.Add("co_num", SortOrderDirection.Ascending);
             dicSortOrder.Add("co_line", SortOrderDirection.Ascending);
             dicSortOrder.Add("co_release", SortOrderDirection.Ascending);
             var sortOrder = sortOrderFactory.Create(dicSortOrder);
       
             using (IRecordBunch recordBunch = recordBunchFactory.Create(appDB, queryLanguage, getVariable, defineProcessVariable,
                mgSessionVariableBasedCache, collectionLoadRequestFactory, Coitem1LoadRequest, sortOrder, bookmarkFactory,
                SQLPagedRecordBunchBookmarkID.BunchingBookmark, BunchType.Load, loadType, recordCap, true))
             {
                 if (recordBunch.ReadPage())
                     Data = recordBunch.CurrentPage;
             }
       
             return Data;
      }
      ```

4. 移除InitSessionContext和CloseSessionContext接口

5. 如果方法被report form调用，还需要一些额外的处理

   1. Record Cap Override = 0
   2. Report Batch Mode = Sequential
   3. Report Batch Size = 5000



### 2，Stream Bulk Load

使用场景：1. single load返回大批数据  2. 游标

优化内存，但是会影响性能

1. 返回的接口是IRecordStream

2. 添加loadType和pageSize和相应的默认值

   1. ```c#
      public IRecordStream CoitemSelect(LoadType loadType = LoadType.First, CachePageSize pageSize = CachePageSize.XLarge)
      ```

3. 确认orderbyClause的列能唯一确定一行记录吗

   1. 遇到Guid或者decimal，需要添加默认值

4. 创建IRecordStream实例

5. 关于loop

   1. 使用using语句和IRecordStream

   2. 移除在loop之前的代码，以及loop内检查数据是否存在的代码，替代为IRecordStream.Read()

   3. ```c#
      // Create the steam instance inside using block. So it can be disposed right after used.
      // in this example streamingDemoCRUD.CoitemSelect() is the method returning the IRecordStream described in the previous steps.
       
      using (IRecordStream coitemRecordStream = streamingDemoCRUD.CoitemSelect(bunchedLoadCollection.LoadType))
      {
          // Call the Read method to move to next row.
          // This is just like cursor fetch.
          while (coitemRecordStream.Read())
          {
              // Get the current row from Current property.
              IRecordReadOnly coitemRow = coitemRecordStream.Current;
              // This is just like fetch cursor into variable.
              CoNum = coitemRow.GetValue<string>(0);
              CoLine = coitemRow.GetValue<int?>(1);
              CoRelease = coitemRow.GetValue<int?>(2);
              Item = coitemRow.GetValue<string>(3);
       
              // Original logic in loop here
          }
      }
      ```



### 3，Caching

使用场景：当相同的scalar data从数据加载多次，缓存起来之后继续用

创建一个实现相同接口的缓存类

1. 属性：IMemoryBasedCache

2. 实现一个私有的GetKey方法

3. 创建一个内部类CacheValue（实现ICachable ）保持数据

4. 添加CompositionRoot

   1. ```c#
      sc.AddScoped<IsFeatureActive>();
      sc.AddScoped<IIsFeatureActive>(s => new IsFeatureActiveCache(s.GetService<IsFeatureActive>(), s.GetService<IMemoryBasedCache>()));
      ```

5. UT使用`FakeIDOMethodCallBoundedMemoryCache fakeIDOMethodCallBoundedMemoryCache`



### 4，Unscheduled Code-Out

使用场景：When a SQL function or SP is being called in a loop resulting in too many round trips to the database



### 5，Remove temp table update/delete

使用场景：When data in a **temp table**, which has data that was recently calculated in the current algorithm, is being updated or deleted



### 6，Remove separate load

使用场景：When we are looping on a record set, and data from that record set is used as the criteria to load an additional record

* 使用join等条件将多次load合并，最好是一次解决



### 7，Remove separate aggregation

使用场景：Aggregate scalar values are being created from a temp table by selecting from it to create totals, etc. 

The following conditions must be met to be able to apply this strategy:

- There is a cursor loop prior to the load request.
- Load request has aggregate and is outside of cursor loop.
- Load request can be combined with main cursor statement.
- Columns involved must not be calculated inside cursor loop.



### 8，Collection data generation (Stream data generation)

使用场景：When a cursor loop is inserting, or a loop is processing data to be inserted into a temp table, and is used only to store processed data for output. And all rows are being processed every cycle but only part (bunch) of the processed data is being returned.



### 9，Replace single-column temp table

使用场景：When data in a temp table, which only for joining with other table(s) to filter out the data that meets the condition(s)

* 使用IList类型的变量存储temp table



### 10，Non-Trigger Pattern

使用场景：When CRUD operations are being used in temp tables or tables that does not involve triggers.

The following conditions must be met to be able to use this pattern:

- The query must not have a function call.
- The query does not have a UNION statement.

1. 找出non-trigger的table