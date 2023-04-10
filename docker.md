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
--- 
# docker-compose
当container数量变多时，一个个设置开启关闭会非常麻烦，这时可以使用```docker-compose```来定义与生成多个container
```yaml
version: "x.x"
services:
  container_1:
    build: path_to_docker_file
    ports:
      - "HOST_PORT:CONTAINER_PORT"
      - "CONTAINER_PORT"
    depends_on:
      container_2:
        condition: service_healthy
    ...
  container_2:
    image: image_name
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"] # 设置检测程序
      interval: 1m30s # 设置检测间隔
      timeout: 10s # 设置检测超时时间
      retries: 3 # 设置重试次数
      start_period: 40s # 启动后，多少秒开始启动检测程序
    networks:
      aliases:
        alias_name
    ...
  ...

volumes:
  ...
networks:
  ...
```
```version```：```docker-compose```的版本  
```services```：需要定义的containers  
- ```depends_on```：用于定于依赖关系  
  1. ```docker-compose up```会按照被依赖容器先依赖容器后的顺序启动
  2. ```docker-compose up SERVICE```时会先启动被依赖容器
  3. ```docker-compose stop```则会先关闭依赖容器后关闭被依赖容器  

  - ```condition```:判断条件  
    - ```service_started```：依赖的服务启动了便可以启动
    - ```service_healthy```：依赖的服务启动了并且通过了healthcheck便可以启动  
    - ```service_completed_successfully```：依赖的服务正常关闭  
   
- ```healthcheck```：用于检测 docker 服务是否健康运行

- ```restart```：设置重启条件  
  - no：默认选项，在任何情况下都不会重启
  - always：总是重启
  - on-failure：在容器非正常退出时（退出状态非0）重启
  - unless-stopped：总是重启除非被用户停止
 
```volumes```：各个container之间的共享文件夹。  
> While it is possible to declare volumes on the fly as part of the service declaration, this section allows you to create named volumes that can be reused across multiple services (without relying on volumes_from)

可以被放置在```services```块内或外，其区别在于：定义在```services```外（top-level）的数据卷是全局共享的，可以在多个服务之间共享使用。

```networks```：用于自定义网络。  
如果未自定义会自动生成一个叫```folder_default```的网络（```folder```为当前所在文件夹名）。各个container之间可以通过```://container_name:container_port```的形式访问。
```services```内用于定义该container可以访问的网络,在其内部可以用```aliases```定义该container在该网络里的别名

```services```外则是对于各个网络的具体设置
