### fetch
取得远程repository但不反映到本地repository  

### merge
将某个branch的修改反映到另外一个branch  

### pull
等同于fetch+merge，基本是更新的意思  

### stash
stash
将目前修改到一般的内容保存起来，可以后面随时恢复

stash push
将目前的修改丢堆起来回滚到HEAD

stash list
查看堆起来的修改

stash apply
恢复存储的修改，默认是最后一个，可以通过指定index修改
apply后stash不会被删除

stash drop
删除stash里的某一项修改

stash pop
apply+drop

### stage
将修改的文件加入下次commit的对象  

### 将修改merge到其他brunch
1. Switch/Checkout到需要被merge的branch
2. 打开log，找到想merge的log，Cherry Pick this comment
3. 目前所在brunch已经有了修改记录，push就可以
