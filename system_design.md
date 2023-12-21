### What is LOAD BALANCING?
当需要处理大量请求时，一台服务器不足以应对，所以需要设置多台服务器。LOAD BALANCING用于解决客户端应该向哪台服务器发送请求的问题，使得各台服务器的负载均衡  
当请求增加时，需要增加服务器来应对。移动服务器数据时，应该尽可能保持原有状态，使得原有的缓存可以继续使用，所以需要使用constant hashing  
假设本来有4台服务器，需要增设到5台    
⭕：S1 S2 S3 S4各自减少5%，合并起来给新服务器S5（5+5+5+5+5）  
❌：S1减少5%合并到S2，S2减少10%合并到S3，S3减少15%合并到S4，S4减少20%构成S5（5+5+10+10+15+15+20+20）  

---

### Horizontal vs. Vertical Scaling
应对更多请求的方法，也就是scalability（可扩展性）  
1. 买台更大的电脑（Vertical Scaling or scale up）   
2. 买更多电脑（Horizontal Scaling or scale out）

trade-off
| |Horizontal|Vertical|
|:-------------:|:-------------:|:-------------:|
|Load Balancing|need|not need|
|lost of machine|resilient⭐|single point of failure|
|IO cost|network calls|inter process communication⭐|
|data consistency（数据传输时的损耗or丢失）|inconsist|consist⭐| 
|scale well as users increse|no⭐|yes|

Real world development:combine both methods benefits⭐　

---

###  System Design Primer ⭐️: How to start with distributed systems? 
scale 扩展  
1. Vertical scaling   
2. Cron job: Preparing before hand at non-peak hours  
     
Resilience 冗余  
4. Backup: keep backups and avoid single point of failure(master -> slave)  
5. Horizontal scaling   

expansion  
6. Microservices 划分任务使得特定服务器只需要应对特定任务  
7. Distributed Systems 为了防止宕机之类的问题使得业务停摆又或者使得不同区域的用户都可以低延迟使用，需要将系统分散开  
8. Load balancing 根据服务器负载情况将请求发送到指定服务器以达到负载均衡  
9. Decoupling  
10. Logging and metric  
11. Extensibility  

High level design:服务器怎么布置，请求跟回复流向  
Low level design:具体需要什么class，什么数据结构    

###  What is a MICROSERVICE ARCHITECTURE and what are its advantages?   
Monolith VS Microservices  
- Monolith
  将服务放于较大的机器上，Monolith也可以横向拓展不一定只能有一台
  好处  
  1. 适合小团队  
  2. 相对简单  
  3. 代码无需多次测试  
  4. 更快（procedure call）

  坏处  
  1. 理解整个系统并作以修改比较难   
  2. 各个部分的耦合造成开发困难  
  3. 容易造成单点故障  

- Microservices
  将服务尽可能分割  
  好处  
  1. 易于拓展  
  2. 适合团队合作开发，开发者不需要理解系统里每个功能  
  3. 易于并行开发，不用等待A功能开发完了才能开发B功（只要不是依附关系）
  4. 容易检测与理解

  坏处   
  1. 不容易设计  

###  What is DATABASE SHARDING?  
- 水平切分（horizontal partitioning）  
  将列表里的行分割，每个分区都有相同的模式与列但是不同的行   
- 垂直切分（vertical partitioning）  
  列被分离出来，并放入新的不同的表中  

要求  
- consistency 数据一致性  
- availability 服务器不宕机数据可读
  使用master-slave构造   

问题  
1. joins  
   当读取来自不同分区的数据，需要将他们合并，因为会很慢  
2. FIXED NUMBER OF SHARDS  
   分区后添加或者减少DB不自由，所以需要consist hashing     

### What is a MESSAGE QUEUE and Where is it used?   
用户向服务器发送请求时，服务器会把需要做的事情放入列表然后回复用户收到请求，在任务完成后再次联系并发送处理结果，使得不需要一直占用用户（异步处理asynchronous processing）  
Fault tolerance: 某一台服务器宕机时，虽然无法继续接受请求，但可以把已经接收到的请求发送给其他服务器处理。因为不可能宕机后再发送出去，所以必须先保存再DB  
MESSAGE QUEUE：每隔一段时间给处理服务器发送请求监测是否宕机，如果宕机会把任务发送给其他服务器。有load balancing功能。QUEUE会记录各个任务完成状态  
实际例子：Rabbit MQ、Zero MQ、JMS  

### AWS
- EC2(Elastic Compute Cloud)  
  在aws上构建的虚拟服务器，类似本地服务器  
- Aurora  
  aws提供的关系型数据库，基本功能操作都和mysql一样  
- ElasticCache  
  redis跟Memcached  
  在内存上运行的数据库  

### Redis VS Memcached
https://aws.amazon.com/cn/elasticache/redis-vs-memcached/?nc1=h_ls  
- 除了支持多线程以外，Memcached并没有比redis好  
- Memcached只支持key/value，redis还支持string,list,dict,set,zset,hyperloglog   
- Memcached无法备份，redis可以  
- Memcached只有单体，redis可以有Replication  
- redis支持pub/sub（发布-订阅）

### 服务器种类
- web server  
  主要处理http请求，表示静态网页    
- app server  
  也可以处理http但主要是处理app内部协议  
- batch server  
  不跟客户端链接，每个一段时间执行特定程序（cron）  
- socket server  
  使用[websocket](https://www.ruanyifeng.com/blog/2017/05/websocket.html)实现长时间的链接，不同与一般http的单向请求，可以在有需要时让服务器主动通知客户端   
- CDN(Content delivery network)  
  用于发送大容量且变化少的资源，比如web网页、视频、游戏内资源  
  为了降低延迟，一般会在各个各个地区or国家设置  
- load balancer  
  将客服端的请求平均分配给各个服务器分散避免拥堵  
- database server  
  用于记录需要持久存在的数据  
  用户不会直接访问，而是app或者web server访问  
  每隔一段时间会备份：master → slave  
  master用于读写，slave只用于读  
  备份的好处：
  1. 更好性能（Better performance）    
  2. 可靠（Reliability）：某个数据库坏也可以继续工作
  
  如果slave坏了，会损失一点性能但是没影响  
  如果master坏了，某个slave会变成master  
- cache server  
  记录DB常被访问的数据，减少DB的负荷  
  记录不需要被DB记录的临时数据  

### 数据中心 Data center
也就是一整个系统(除了load balancer)  
为了使系统更Resilience以及减少不同地区访问延迟  
可以将一Data center部署在多个地区  

### session 会话
指在等于或多余2台的设备间进行的双向的、有时间限制的。通常是有状态的、多余1条信息的  
由于http协议本身没有状态，所以必须使用额外的存储  
会话状态是指服务器与浏览器在会话过程中产生的状态信息  
- 存储在浏览器  
  也就是常说的cookie  
- 存储在服务器  
  存放在数据库里  

### Message queue 消息队列
一种异步通信方式  
producer发出的请求会被存储在mq里，consumer会从mq里取出信息进行处理  
因为Decoupling 解耦，所以使得系统scalable跟resilience  
![image](https://github.com/KnnUUu/note/assets/44579350/1def309b-04d3-4aef-9f20-cb41e66a531e)

### Logging, monitoring, metrics, automation tools
记录日志、监控服务器各项指标（cpu、内存）、CICD  

### 常见API架构  
![famous_api_architecture](https://github.com/KnnUUu/note/assets/44579350/492002d2-6e3b-4466-9b48-12483a0ca50d)

websocket：又http升级而来，实现长时间以及双向的链接  

## System Design: TINDER as a microservice architecture   
- 如何存储用户照片？  
  file vs Blob（binary large object）  
- 数据库功能  
  1. mutability 可变性 ❌  
     修改数据库内数据，这里指的是修改用户上传的图片，但实际上只需要上传一张新的，所以并不需要修改图片的功能  
  2. transaction ACID  
     跟上面一样的理由不需要  
     Atomicity：某一个修改失败的时候，命令无效，数据库会维持在命令发出前的状态  
     Consistency：事务开始之前和事务结束以后，数据库的完整性没有被破坏。写入的资料必须完全符合所有的预设约束、触发器、级联回滚等  
     Isolation 隔离：允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致  
     Durability 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。  
  3. index
     图片内部数据是0与1没有意义  
  4. access control
- 发送信息
  http每次请求都要链接并且只是单向的，不适合用于DM功能
  这里使用XMPP协议  
