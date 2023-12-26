# System Design Interview An Insider’s Guide  
## CHAPTER 1: SCALE FROM ZERO TO MILLIONS OF USERS  
- 原始状态  
  所有服务数据都在服务器上，只有客户端/服务器/DNS  
  网络应用 - 后端语言(Java, Python, etc.)加前端语言(HTML and JavaScript)   
  app - 基于http获取json    

- DB  
  将网络层(web tier)和数据层(data tier)分开  
  关系型数据库与NoSql  
  - 什么时候用NoSql？  
    需要超低延迟  
    数据没有整理  
    只需要序列化反序列化数据  
    需要存储大量数据  

- Scaling 服务器拓展 + Load Balancer  
  Vertical scaling vs horizontal scaling  
  
- Database replication  
- Cache  
- CDN  
- Stateless web tier  
  把会话信息存放在数据库里使得服务器容易拓展  
- Data centers                
  布置多个数据中心以满足不同区域用户请求  
- Message queue  
  实现异步数据交流以实现解耦decouple  
  比如：RabbitMQ, Kafka  
- Logging, metrics, automation      
  log记录关键信息   
  metrics 观测服务器性能,运营数据    
  automation 自动化比如CICD  
- Database scaling 数据库拓展  
  - Vertical scaling  
    更好性能服务器  
  - Horizontal scaling  
    将数据分割并存储与不同DB上  
    问题  
    - Resharding data  
      某个DB满了  
    - Celebrity problem  
      某些数据比其他数据更频繁被访问（比如社交网络的名人账号）所以需要平均存放  
    - Join and de-normalization  
      需要一次性取出来自不同服务器的数据  
    
how we scale our system to support millions of users:  
- Keep web tier stateless
- Build redundancy at every tier
- Cache data as much as you can
- Support multiple data centers
- Host static assets in CDN
- Scale your data tier by sharding
- Split tiers into individual services
- Monitor your system and use automation tools

## CHAPTER 2: BACK-OF-THE-ENVELOPE ESTIMATION
### Power of two
2**10 = 1KB  
2**20 = 1MB  
2**30 = 1GB  
2**40 = 1TB  
2**50 = 1PB  

2**30 / 2*2**10 = 2**20/2 = 2**19

### Latency numbers
![image](https://github.com/KnnUUu/note/assets/44579350/cd8a9006-3056-4509-b3b1-4f82ca960082)

1 ns = 10^-9 seconds  
1 μs= 10^-6 seconds = 1,000 ns  
1 ms = 10^-3 seconds = 1,000 μs = 1,000,000 ns  

### Availability numbers
![image](https://github.com/KnnUUu/note/assets/44579350/f9b3027c-5586-4d9b-9334-a3b712f581f3)

### 计算
Query per second (QPS) = 每日活跃用户 * 每日发送请求数量 / 24小时 / 60分钟 / 60秒  
Peek QPS = 2 * QPS  

### Tips
- Rounding and Approximation  
  没有必要精确到小数点后几位，大致数据就可以了   
- Write down your assumptions  
- Label your units
  不要忘记数据的单位
- Commonly asked back-of-the-envelope estimations
  - QPS  
  - peak QPS  
  - storage  
  - cache
  - number of servers

## CHAPTER 3: A FRAMEWORK FOR SYSTEM DESIGN INTERVIEWS
不止考察设计分布式系统的能力，也考察沟通能力  
1. Understand the problem and establish design scope: 3 - 10 minutes  
   问问题搞清楚需求再回答  
   有可能直接告诉你也有可能让你自己推断，后者的话记得写下你的假设  
   问题例子  
   - What specific features are we going to build?  
   - How many users does the product have?    
   - How fast does the company anticipate to scale up? What are the anticipated scales in 3 months, 6 months, and a year?  
   - What is the company’s technology stack? What existing services you might leverage to simplify the design?  
   - Is this a mobile app? Or a web app?   
   - What is the traffic(流量) volume?  
2. Propose high-level design and get buy-in: 10 - 15 minutes  
   - Come up with an initial blueprint for the design  
   - Draw box diagrams with key components on the whiteboard or paper  
     clients (mobile/web), APIs, web servers, data stores, cache, CDN, message queue, etc.  
   - Do back-of-the-envelope calculations  
3. Design deep dive: 10 - 25 minutes  
   增加low-level设计，添加细节  
4. Wrap up: 3 - 5 minutes  
   - 分析系统的瓶颈并给出优化方案  
   - 系统出错时的分析  
   - 运营问题  
   - 如何服务更多的用户  
   
## CHAPTER 4: DESIGN A RATE LIMITER
rate limiter is used to control the rate of traffic sent by a client or a service  
### 有什么用？  
- 防止DOS和DDOS攻击  
  DOS(Denial of Service)  单体对单体攻击  
  DDOS(Distributed Denial of Service) 多体对单体攻击  
- 降低成本
  减少服务器
  减少第三方付费api成本  
### Understand the problem and establish design scope
问题  
- 放在客户端还是服务器上？
- 根据什么来限流？
- 系统的量级，给start-up还是大公司用？
- 是否部署在分布式系统？
- 做成外部服务还是部署在内部代码上？

需求  
- 准确限制请求
- 低延迟
- 分布式
- 当被拦截时通知用户
- 高容错率，rate limiter出现问题也不会影响主服务  

### Propose high-level design and get buy-in
注意点  
- 添加服务时需要考虑项目现存技术栈  
- 自己做耗时耗力但是可以保证全权掌控，使用第三方省时省力  
- 如果本身已经有api gateway的话可以直接在上面搭建  

算法  
- Token bucket  
  ![image](https://github.com/KnnUUu/note/assets/44579350/e28a5e42-23e1-45f4-ac7a-fa4af3bf0ca6)  
  token按照一定时间增加直到到达token上限   
  每次处理请求消耗一个token，当剩下0的时候无视请求  

  加入有复数个服务需要限制，则需要复数个bucket  
  价格每个ip都需要限制，则每个ip都需要一个bucket  

  pro  
  - 容易部署  
  - 内存利用率高  
  - 允许短时间内流量快速增长  
  con  
  - 调整 bucket size 和 token refill rate可能比较困难  

- Leaking bucket  
- Fixed window counter   
- Sliding window log  
- Sliding window counter  

计数数据如果放在db就太慢了，这里使用了redis，因为快以及支持超时删除(EXPIRE)的功能    

### Design deep dive  
- 超过限制时候应该返回HTTP429错误并且附上以下信息  
  剩下次数、限制次数、多久后可以重新请求  
- 分布式可能出现的问题  
  - Race condition  
    接受第一个请求后在写入新的次数时再接收到第二个请求，会导致计算错误总共只增加了一次  
    可以考虑加锁，但是会导致变慢  
  - Synchronization issue  
    如果存在多个限制器，那么必须将不同限制器之间的数据同步，否则会出现在A限制器明明已经超过限制但却可以访问B限制器的状况  
    这里给出的解决方案是把不同限制器的数据放在同个redis里面   

## CHAPTER 5: DESIGN CONSISTENT HASHING
有多个cache服务器时，如果使用普通hash函数做load balancing，在某个服务器宕机的时候，为了修改分配算法几乎需要移动所有的数据  
一致性哈希就是为了解决此类问题使用的算法，也可以用于数据库分片sharding    

当哈希表调整大小时，k为键总数，n为slot数（这里则是服务器数量），一致性哈希只需要k/n次重新映射，而普通哈希则需要几乎重新映射所有的键值  

- 算法  
  1. 将服务器跟键都哈希后放在环里  
  2. 判断哪个键属于哪个服务器，从键的位置往后走，寻找第一个遇到的服务器  

- 问题    
  1. 因为会追加或者删除服务器，几乎不可能保证各个区间一样大  
  2. 键与服务器分布不均衡，导致负载不均衡  

- 解决方法 Virtual nodes   
  将一个真实的服务器复制多个虚拟节点，分布在环上，节点的增加可以降低标准差standard deviation，使得负载均衡  
  Automatic scaling：服务器数量可以根据负载自动调整  
  Heterogeneity：虚拟节点数量可以根据服务器性能调整，好的服务器可以设置更多节点  

## CHAPTER 6: DESIGN A KEY-VALUE STORE
NoSql的一种，键越短性能越好（快）  
常见数据库：Amazon dynamo, Memcached, Redis  

### 单个键值数据库  
部署简单，使用哈希表即可    
- 两个优化方法  
  1. 压缩数据  
  2. 内存只存放常用的数据，其他放硬盘  

### 分布式键值数据库  
也称作分布式哈希表  
- CAP (Consistency, Availability, Partition Tolerance) theorem  
  对于所有的分布式存储，不可能三角只能保证其中两个  
  - Consistency  
    同一时间点不管从哪个数据库取，取得的数据都是一样的  
  - Availability  
    任意时间点的请求都可以获得回复哪怕有一部分数据库宕机  
  - Partition Tolerance  
    不同服务器之间无法通信的时候，系统也可以继续运行  

  CA牺牲了P，但是由于网络故障不可避免，现实中不存在CA的设计,只能在CA种选一个  
  CP需要在数据写入到同步为止锁住，银行系统就是使用的CP
  AP则可以在任意时间点读，但代价是可能读到老旧数据

### 搭建键值数据库用到的技术以及组件
- Data partition(sharding)  
  大量的数据无法放在同一个数据库内，所以需要分割成小块放在不同数据库里面  
  - 问题  
    1. 如何均衡分割数据  
    2. 增加或减少节点的时候减少数据迁移的量  

- Data replication  
  防止singal point failure  
  方法依旧是consistent hashing，不同的是会指定一个数字N，在对键哈希后，往后寻找N个服务器复制过去  
  如果使用了Virtual nodes，为了防止最后存储在同一个服务器上，在寻找服务器时会跳过同一个服务器  

- Consistency  
  quorum  

## CHAPTER 15: DESIGN GOOGLE DRIVE
### Step 1 - Understand the problem and establish design scope
需求  
- 需要的功能：上传下载通知  
- 网络应用与app都要  
- 支持所有文件格式  
- 文件需要加密  
- 文件不能大于10GB  
- 10M DAU  
- 同步功能：在某个设备上追加的文件会被同步到其他设备上  
- 文件版本  
- 共享文件

隐形需求
- 可靠，不会出现文件丢失
- 快速同步
- 高带宽使用效率
- 可拓展性
- 高可用性 availability
  
API   
- upload  
  根据文件大小分为普通上传跟中断后可恢复上传(Resumable)  

  Example API:  
  - https://api.example.com/files/upload?uploadType=resumable  
  
  Params:  
  - uploadType=resumable  
  - data: Local file to be uploaded  
- download  
  Example API:  
  - https://api.example.com/files/download  
  
  Params:  
  - path: download file path  
- Get file revisions  
  Example API:  
  - https://api.example.com/files/list_revisions
  
  Params:  
  - path: The path to the file you want to get the revision history.  
  - limit: The maximum number of revisions to return.
    
---  

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

### Introduction to NoSQL databases
好处  
1. easily insertion and retrievals  
2. schema is easily change    
3. build for scale(horizonal sharding)  
4. build for aggregation   

坏处  
1. not build for update(no ACID)  
2. Read time are comparatively slower   
3. relation are not implicit  
4. join are hard

种类  
1. key-value  
   redis / DynanoDB  
2. Document  
   MongoDB(文件被以类似json的形式储存 bson - binary encoded Javascript Object Notation)  
3. Graph  
   Neo4j / Giraph  
4. Wild-Column     
   Cassandra / HBase  

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
