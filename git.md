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
