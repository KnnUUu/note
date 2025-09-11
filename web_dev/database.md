## 概要
### 数据库种类
1. 关系数据库 Relational databases  
   MySQL、MariaDB、Percona Server（MySQL的代替品·）、PostgreSQL、Microsoft Access、Microsoft SQL Server、Google Fusion Tables、FileMaker、Oracle数据库、Sybase、dBASE、Clipper、FoxPro、foshub
   popular, historically, worked well.
   
2. 非关系型数据库（NoSQL） Non-Relational databases   
   BigTable（Google）、Cassandra、MongoDB、CouchDB  

   Non-relational databases might be the right choice if:  
   - Your application requires super-low latency.  
   - Your data are unstructured, or you do not have any relational data.    
   - You only need to serialize and deserialize data (JSON, XML, YAML, etc.).  
   - You need to store a massive amount of data.  

3. 键值（key-value）数据库  
   Apache Cassandra（为Facebook所使用）：高度可扩展、Dynamo、LevelDB（Google）  

### Object Relational Mapping (ORM)
使得OOP可以像使用SQL一样操作数据库的库  

### binary large object(BLOB)
a varying-length binary string that can be up to 2,147,483,647 characters long.
   
## SQL
### 常用命令
- 登录  
  ```sql
  mysql -u[id] -p[password]
  ```
- 登录（无密码）  
  ```sql
  mysql -uroot
  ```
- 退出  
  ```sql
  exit;
  ```
- 数据库一览  
  ```sql
  SHOW DATABASES;
  ```
- 选择数据库  
  ```sql
  USE [dbname];
  ```
- 查看table设置  
  ```sql
  DESC [tablename];
  -- 或
  DESCRIBE [tablename];
  ```
- 查看更详细的table设置  
  ```sql
  SHOW CREATE TABLE [tablename];
  ```
- 注释  
  ```sql
  -- 单行注释

  /*
   多行注释
  */
  ```
- 创建table
  ```sql
  CREATE TABLE celebs (
    id INTEGER PRIMARY KEY, 
    name TEXT UNIQUE,
    date_of_birth TEXT NOT NULL,
    date_of_death TEXT DEFAULT 'Not Applicable'
  );
  ```
  PRIMARY KEY：唯一标识表中的每一行，且不能为空。  
  UNIQUE：保证该列中的所有值都唯一，允许null。  
  NOT NULL：该列的值不能为空。可以接在UNIQUE后面使用  
  DEFAULT：为该列设置默认值，如果插入时未指定则使用该值。  
- 获取特定栏数据  
  ```sql
  SELECT [tablename1,tablename2...] FROM [tablename] WHERE [conditions];
  ```
- 除查询结果中的重复值  
  ```sql
  SELECT DISTINCT city FROM users;
  ```
- 数据分页  
  ```sql
  SELECT * FROM users LIMIT 10 OFFSET 20;
  ```
  跳过前 20 行，从第 21 行开始，返回 10 条记录  
- 获取整个table数据  
  ```sql
  SELECT * FROM [tablename] WHERE [conditions];
  ```
- 删除数据（无法恢复:warning:）  
  ```sql
  DELETE FROM [tablename] WHERE [conditions];
  ```
- 插入数据  
  ```sql
  INSERT INTO [tablename] (column1, column2, column3, ...) VALUES ([column1 value], [column2 value], ...);
  ```
- 变更数据  
  ```sql
  UPDATE [tablename] SET [colunm name] = [value] WHERE [conditions];
  ```
- 删除table  
  ```sql
  DROP TABLE [tablename];
  ```
- 创建新表  
  ```sql
  CREATE TABLE IF NOT EXISTS [tblname] (
    [column_1] [data_type], 
    [column_2] [data_type], 
    [column_3] [data_type]
    );
  ```
- 增加栏  
  ```sql
  ALTER TABLE [tblname] ADD [col_name] [data_type];
  ```
- 删除栏  
  ```sql
  ALTER TABLE [tblname] DROP [col_name];
  ```
- 现在时间  
  ```sql
  NOW()
  -- 或
  CURRENT_TIMESTAMP()
  ```
- 调整时间  
  ```sql
  ADDTIME(NOW(),'06:00:00') -- 6小时后
  SUBTIME(NOW(),'06:00:00') -- 6小时前
  ```
- IN / NOT IN  
  ```sql
  SELECT * FROM UserInfo WHERE id IN (1,2,3);
  ```
- GROUP BY  
  将拥有同样值得数据集合起来，一般与SUM, COUNT, AVG, MAX, MIN配合使用   
  ```sql
  SELECT AGE, AVG(SALARY) as avg_salary
  FROM CUSTOMERS
  GROUP BY age
  ```

  -- 需要两栏值一样的情况 
  ```sql
  SELECT gender, age, SUM(salary) AS total_salary
  FROM Custome
  GROUP BY gender, age
  ```
  一般情况下使用where筛选数据，但group by时候则使用having
  ```sql
  GROUP BY group_condition
  HAVING having_condition
  ```
- 获取字符串长度
  ```sql
  LENGTH(name_of_string)
  ```
- 重命名col
  ```sql
  SELECT old_name FROM database_name WHERE XXX as new_name
  ```
- 升降序排列
  ```sql
  ORDER BY column_name
  ORDER BY column_name DESC
  ```
- join
  ```sql
  SELECT table1.column1, table2.column2, ...
  FROM table1
  XXX JOIN table2 ON condition;
  ```
  - Inner Join  
    查找两个表同时存在的行  
  - Left Join / Right Join  
    返回左（右）表里所有的数据，如果右（左）表也有同样的数据则会一起返回。
  - Full Join  
    返回两个表所有记录，关联起来的数据会合并到一起  
  - Left / Right Join Excluding Inner Join  
    返回左（右）表有但是右（左）表没有的数据  
  - Full Outer Join Excluding Inner Join  
    返回两个表不关联的数据      
- 函数
  ```sql
  CREATE FUNCTION 函数名(参数列表)
  RETURNS 返回类型
  BEGIN
      -- 函数体
      RETURN 返回值;
  END;
  ```
  函数变量  
  ```sql
  DECLARE x INT;
  SET x = 5;
  SET x = x + 2;
  ```
### 性能优化
1. 连接配置优化  
   1. 服务端配置  
      - 增加可用连接数  
      - 及时释放不活动的连接  
   2. 客户端优化  
      - 尽量减少和服务端建立连接的次数，尽量用已经建立的连接  
2. 架构优化
   1. 使用缓存  
   2. 读写分离  
      **<img src="https://github.com/KnnUUu/note/assets/44579350/a77ed85f-810c-448d-9bb8-dc5c65c7979e"  width="50%" height="50%" />**  
      如何复制master操作到slave上？  
      master节点会将所有的写操作记录到binlog中，slave节点会有专门的I/O线程读取master节点的binlog，将写操作同步到当前所在的slave节点。  
   3. 分库分表
