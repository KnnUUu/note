---  
### daemon process  
  
Linux上与用户操作无关，系统启动后就会一起启动的后台程序。基本等同windows的service

---  

### crontab 
用来定期执行程序的命令。


### Unix Timestamp   
从1970年1月1日开始经过时间的秒数，可以转换成常用时间格式  
例：1697022446 → Wed Oct 11 2023 20:07:26 GMT+0900 (GMT+09:00)  

### /etc/environment
记载了所有程序使用的环境变量  

### ~/.bashrc
bash启动时读取的设定文件  

### ~/.profile
login时读取的的设定文件  

### nano
vim操作太反人类了，nano比较好用点  
`$nano filename`  

# command
### I/O
`<`: 从其他文件读取数据  
`>`: 将输出写入到其他文件里  
`>>`: 同上，但是不覆盖只是追加到最后  

### make
> make utility is to determine automatically which pieces of a large program need to be recompiled, and issue the commands to recompile them.

make是为了文件修改时方便再次编译与生成可执行文件，但也可以用在其他地方以便执行一系列命令。  
```make
#Makefile
target … : prerequisites …
        recipe
        …
        …
target … : prerequisites …
        recipe
        …
        …
```
使用需要在同文件夹内添加```Makefile```（如有需要也可以指定path）后，通过```make target prerequisite_1 prerequisite_2 ...```的形式来调用，target用来识别哪一系列任务会被执行，如果没有前置条件（类似函数形参）可以省略。  
如果没有指定```target```则默认第一个目标会被执行，可以通过在文件开头添加```.DEFAULT_GOAL := default_target```的形式指定默认目标。  
