### Apache（httpd）
世界使用排名第一的开源服务器软件。  
将电脑变成一台服务器，开放特定的网络端口，用以接收来自网络上发送到这台机器的HTTP请求，对请求的内容进行处理并作出相应的响应  

GET和POST的对象  
- HTML、CSS、JS、RAR、TXT等文件  
  直接把文件的内容发回客户端  
- PHP文件  
  PHP.EXE程序，把文件、调用的参数传递给PHP.EXE，然后把PHP.EXE执行的结果反馈给客户端  
  PHP可以调用各种库执行各类功能，最典型的就是查询数据库  

### URL符号
- `#` 在一个网页中，URL 不变的情况下，通过添加“#anchor_name”的字符在 URL 最后可以跳转到当前网页中已经定义好的锚点（id=“anchor_name”）位置  
  例：`https://zhan.leiue.com/fanly-mip.html#buy`  
- `?` 向网页传递参数，常用于动态网站，实现不同的参数值而生成不同的页面或者返回不同的结果  
  例：`https://i.leiue.com/avatar/?size=100`  
- `&` 连接不同的参数，一般与`?`结合使用  
  例：`https://i.leiue.com/avatar/?size=100&time=20171120`  

参考：https://blog.csdn.net/lucky541788/article/details/81836466  

### APC - Alternative PHP Cache - 可选PHP缓存  
一种对PHP有效的开放源高速缓冲储存器工具，可用于缓存和优化Web服务器上的PHP代码，改善服务器性能  

### APCu - APC User
> APCu is APC stripped of opcode caching.

APCU是从APC剥离出来的用户数据缓存功能，去掉了apc的opcode cache    
提供用户数据缓存功能，和redis/memcache类似  

> OPcache improves PHP performance by storing precompiled script bytecode in shared memory, thereby removing the need for PHP to load and parse scripts on each request. 

将PHP代码编译之后所产生的bytecode暂存在共享内存内供重复使用，以提升应用的运行效率

自PHP5.5之后，**APC已经被Zend OPcache + APCu的组合取代**

### Memcached
一种基于内存的key-value存储，用来存储小块的任意数据（字符串、对象）。这些数据可以是数据库调用、API调用或者是页面渲染的结果。 
缓存数据库查询结果，减少数据库访问次数，以提高动态Web应用的速度、提高可扩展性。 

### node.js
一种Javascript的运行环境。  
原本js只能在浏览器上运行，node.js使得其可以服务端上执行操作

### 几种服务器
- Web Server
  handles mainly HTTP protocols
- Application Server
  deals with several different protocols, including, but not limited, to HTTP
- Proxy server
  act as a bridge between a host server and a client server. 
- Virtual machine (VM)
  virtual machines store and connect data strictly through virtual space.
- File transfer protocol (FTP) server
  receiving, sending, deleting files
- Database server
参考：https://www.indeed.com/career-advice/career-development/types-of-servers
