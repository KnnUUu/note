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
| |Vertical|Horizontal|
|:-------------:|:-------------:|:-------------:|
|Load Balancing|need|not need|
|lots of machine|resilient⭐|single point of failure|
|IO cost|network calls|inter process communication⭐|
|data consistency|inconsist|consist⭐| 
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
  
## 例子
