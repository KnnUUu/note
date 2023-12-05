### What is LOAD BALANCING?
当需要处理大量请求时，一台服务器不足以应对，所以需要设置多台服务器。LOAD BALANCING用于解决客户端应该向哪台服务器发送请求的问题，使得各台服务器的负载均衡  
当请求增加时，需要增加服务器来应对。移动服务器数据时，应该尽可能保持原有状态，使得原有的缓存可以继续使用，所以需要使用constant hashing  
假设本来有4台服务器，需要增设到5台    
⭕：S1 S2 S3 S4各自减少5%，合并起来给新服务器S5（5+5+5+5+5）  
❌：S1减少5%合并到S2，S2减少10%合并到S3，S3减少15%合并到S4，S4减少20%构成S5（5+5+10+10+15+15+20+20）  

---

### Horizontal vs. Vertical Scaling
应对更多请求的方法，也就是scalability（可扩展性）  
1. 买台更大的电脑（Vertical Scaling）   
2. 买更多电脑（Horizontal Scaling）

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

### session 会话
指在等于或多余2台的设备间进行的双向的、有时间限制的。通常是有状态的、多余1条信息的  
由于http协议本身没有状态，所以必须使用额外的存储  
- 存储在浏览器  
  也就是常说的cookie  
- 存储在服务器  
  存放在数据库里  

### 常见API架构  
![famous_api_architecture](https://github.com/KnnUUu/note/assets/44579350/492002d2-6e3b-4466-9b48-12483a0ca50d)

## 例子
