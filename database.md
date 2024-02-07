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
   
## SQL
### 常用命令
- 登录  
  `mysql -u[id] -p[password]`  
- 登录（无密码）  
  `mysql -uroot`  
- 退出  
  `exit;`  
- 数据库一览  
  `SHOW DATABASES;`  
- 选择数据库  
  `USE [dbname];`  
- 查看table设置  
  `DESC [tablename]; or DESCRIBE [tablename];`  
- 查看更详细的table设置  
  `SHOW CREATE TABLE [tablename];`  
- 获取特定栏数据  
  `SELECT [tablename1,tablename2...] FROM [tablename] WHERE [conditions];`  
- 获取整个table数据  
  `SELECT * FROM [tablename] WHERE [conditions];`  
- 删除数据（无法恢复:warning:）  
  `DELETE FROM [tablename] WHERE [conditions];`  
- 插入数据  
  `INSERT INTO [tablename] (column1, column2, column3, ...) VALUES ([column1 value], [column2 value], ...);`  
- 变更数据  
  `UPDATE [tablename] SET [colunm name] = [value] WHERE [conditions];`  
- 删除table  
  `DROP TABLE [tablename];`  
- 创建新表  
  `CREATE TABLE IF NOT EXISTS [tblname] (col_name data_type, ...);`  
- 增加栏  
  `ALTER TABLE [tblname] ADD [col_name] [data_type];`  
- 删除栏  
  `ALTER TABLE [tblname] DROP [col_name];`  
- 现在时间  
  `NOW()` or`CURRENT_TIMESTAMP()`  
- 调整时间  
  `ADDTIME(NOW(),'06:00:00') // 6小时後` `SUBTIME(NOW(),'06:00:00') // 6小时前`    
- IN / NOT IN  
  `SELECT * FROM UserInfo WHERE id IN (1,2,3);`  
- GROUP BY  
  将拥有同样值得数据集合起来，一般与SUM, COUNT, AVG, MAX, MIN配合使用   
  ```sql
  SELECT AGE, AVG(SALARY) as avg_salary
  FROM CUSTOMERS
  GROUP BY age

  -- 需要两栏值一样的情况 
  SELECT gender, age, SUM(salary) AS total_salary
  FROM Custome
  GROUP BY gender, age
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
