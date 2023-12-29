## API GUIDE 
### Resource names
- 避免使用过于笼统的名词
- 复数形式，驼峰命名
  > Must be in plural form with lowerCamel case.
  
  lowerCamel case: *l* owerCamelCase  
  upperCamel case: *U* pperCamelCase
### Filtering 
> ?limit=10

指定返回记录的数量  

> ?offset=10

指定返回记录的开始位置  

> ?page=2&per_page=100

指定第几页，以及每页的记录数  

> ?sortby=name&order=asc

指定返回结果按照哪个属性排序，以及排序顺序  

> ?animal_type_id=1

指定筛选条件  

### Status Codes
- 100 Continue  
  收到请求但需要后续请求  
- 101 Switching Protocols  
  收到并同意切换协议请求  
- 200 OK  
- 201 Created  
  请求被完成并且有一个或多个资源被创造  
- 202 Accepted  
  请求收到并且接受，但是还在处理中  
- 203 Non-Authoritative Information  
- 400 Bad Request  
  错误请求，不会被执行  
- 401 Unauthorized  
  没有认证  
- 403 Forbidden  
  有认证但是没有权限  
- 404 Not Found  
  找不到目标资源  
- 406 Not Acceptable  
  用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）  
- 410 Gone  
  用户请求的资源被永久删除，可能永远无法获得  
- 500 Internal Server Error  
  服务器遇到意外状况无法满足请求  
    
参考：https://www.rfc-editor.org/rfc/rfc9110.html#name-status-codes  
## HTTP methods
安全性：不会改变服务器状态  
冪等性：相同命令不管执行几次结果都一样

|   |作用|安全性|冪等性|Request body|HTTP status codes|
|---|---|---|---|---|---|
|GET|获取数据|⭕|⭕|❌|获取成功：200|
|POST|追加数据|❌|❌|按需|创建资源成功：200/201/202|
|PUT|更新整个数据<br>如果指定的数据不存在则会新建|❌|⭕|按需|创建资源成功：201/202<br>更新资源成功：200|
|PATCH|更新一部分数据|❌|⭕|按需|更新资源成功：200|
|DELETE|删除数据|   |   |❌|删除成功：200<br>资源不存在：204|
