### cpp主要的区别
- 解释型语言无需编译
- 可以嵌入html使用
- dynamic typing
> dynamic typing means that a compiler or an interpreter assigns a type to all the variables at run-time
- 执行在服务器上，客户端只能看到执行结果
- 变量前带`$`，不需要宣言
- 字符串  
  字符串之间使用`.`连接  
  `""`括起来会识别逃离符，`''`则会直接输出不转换  
  字符串内植入变量时，需要以空格隔开or用`{}`括起来    
```php
//echo "variable$x";  // 错误
echo "variable $x ";  
echo "variable ${x}";  
echo "variable {$x}";  
```
### self vs this  
this是在实例化的时候来确定指向谁。 所以说，this就是指向当前对象实例的指针，不指向任何其他对象或类。  
self是指向类本身，也就是self是不指向任何已经实例化的对象，一般self使用来指向类中的静态变量。  
### 判定式
`==`与`!=`不考虑数据类型  
`===`与`!==`考虑数据类型    
```php
// 返回true
'0'   == 0
false == 0
NULL  == false

// 返回false
'0'   === 0
false === 0
NULL  === false
```
