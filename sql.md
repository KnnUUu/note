用的是mysql但其他应该也差不了多少

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
