# dockerfile
用于生成image，docker会逐行执行命令

```dockerfile
# Comment
INSTRUCTION arguments
```
### FROM
抓取一个image并作为后续操作的基底
```dockerfile
FROM [--platform=<platform>] <image> [AS <name>]
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

### RUN
执行命令
```dockerfile
RUN <shell_command> [paramater_1] [paramater_2]
```

### COPY
从source复制文件到dest
```chown```改变复制到容器内文件的拥有者和属组
```chmod```改变复制到容器内文件的权限
```dockerfile
COPY [--chown=<user>:<group>] [--chmod=<perms>] <source_1> <source_2> ... <dest>
```


### ADD
类似**COPY**
```dockerfile
ADD [--chown=<user>:<group>] [--chmod=<perms>] [--checksum=<checksum>] <src>... <dest>
```
### CMD
目的是为**ENTRYPOINT**提供默认值或者container启动时执行命令，如果有多个**CMD**的话只有最后一个会被执行
```dockerfile
CMD ["param1","param2"] #as default parameters to ENTRYPOINT
CMD command param1 param2 
```
### ENTRYPOINT
类似于 CMD 指令，但其不会被 docker run 的命令行参数指定的指令所覆盖
```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2
```

### ENV
设置环境变量
```dockerfile
ENV <key> <value>
```

### ARG
设置变量，跟```ENV```区别在于```ARG```的变量作用域之限制在dockerfile里
```dockerfile
ARG <name>[=<default value>]
```

### VOLUME

背景知识：使用```docker run -v```可以创造一个宿主机（host）目录（称数据卷）和容器目录做映射
- 可以在容器之间共享和重用
- 对数据卷的修改会立马生效
- 对数据卷的更新，不会影响镜像
- 会一直存在，即使容器被删除

而```VOLUME```可以定义匿名数据卷。
使用户忘记指定```docker run -v```时，程序也可以跑得起来并且再次期间处理的数据不会随着容器删除丢失
```dockerfile
VOLUME <path>
```
参考：[一文说清楚Dockerfile 中VOLUME到底有什么用？](https://blog.csdn.net/qq32933432/article/details/120944205)

### EXPOSE
声明监听用port，默认TCP也可以指定UDP
```dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

### WORKDIR
指定一个工作路径，```RUN```、```CMD```、```ENTRYPOINT```、```COPY```、```ADD```都会在这个路径下执行  
没有指定的话默认是```/```
```dockerfile
WORKDIR /path/to/workdir
```
