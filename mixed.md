不知道怎么分类的内容  

---  
### line break types

```CR(CARRIAGE RETURN) \f``` 光标移动至行头，macOS用  
```LF(LINE FEED) \r``` 光标移动至行头下一行但不改变当前位置，Unix用  
```CRLF```windows用

---  
### 原子性 Atomicity 

无法再细分的操作，要么执行成功要么失败完全不执行

---
### 轮询 Polling

每隔一段时间连接并进行需要的处理
比如客户端每隔一段时间连接上服务器更新数据
