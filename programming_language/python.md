## cheat sheet
- 没有`null`只有`None`
- 没有`this`只有`self`
- `True` `False` 头字母大写  

## 函数
### 值传递 or 引用传递
> arguments are passed by assignment in Python. Since assignment just creates references to objects, there’s no alias between an argument name in the caller and callee, and so no call-by-reference per Se

人话解释：如果函数收到的是一个可变对象的引用，就能修改对象的原始值——相当于传址。如果函数收到的是一个不可变对象的引用（其实也是对象地址！！！），就不能直接修改原始对象——相当于传值。  
- Immutable Objects  
  can’t be changed after it is created  
  `int`, `float`, `bool`, `string`, `Unicode`, `tuple`  
- Mutable Objects  
  Custom classes are generally mutable  
  `list`, `dict`, `set`  

# OOP
## class
```python
class Dog:
    kind = 'canine'         # class variable shared by all instances
    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
    def printKind(self):
        print(self.kind)    # DO NOT FORGET self. WHEN CALLING GLOBAL VARIBLE!!!
```
### property
使得函数可以像变量一样被调用  
```python
@property
def x(self):
    return self._x      # getter

@x.setter
def x(self, value):
    self._x = value     # setter

print(point.x）
point.x = 30
```
### 继承
```python
 class Student(Person):
    super().__init__(name) # super会返回一个父类的实体
```

## colon after variable 
叫做`type annotations`，用于标记变量数据型  
参考：https://docs.python.org/3/library/typing.html

## syntax  
`try`:测试一段代码是否出错    
`except`：出错时的处理  
`else`：没出错时的处理  
`finally`：不管有没有出错都会执行的处理  
`raise`：发出异常（Exception）  
```python
try: 
    #可能发生错误的处理
except error_type:
    #特定错误发生时候的处理
expect:
    #其他错误发生时候的处理
else:
    #没出错时的处理
finally:
    #不管有没有出错都会执行的处理 
```

## math
```python
# 排列 Permutation 从n里选k个
math.perm(n, k=None) # n! / (n - k)!
# 组合 Combination 从n里选k个
math.comb(n, k) # n! / (k! * (n - k)!)

math.ceil(x)
math.floor(x)
```

## 常数 constant
### 无限 infinity
```python
positive_infinity = float('inf')
negative_infinity = float('-inf')

import math
positive_infinity = math.inf
negative_infinity = -math.inf
```
### float
```python
>>> print('{:,}'.format(1000000)) #每3位插入,
1,000,000

>>> print('{:.4f}'.format(3.14)) #小数点以下4位
3.1400

>>> print('{:.2g}'.format(1234567)) #表示2位数字
1.2e+06
>>> print('{:.10g}'.format(0.1234567)) #超过本身长度的话则不会有变化
0.1234567
```

# data structure
## hash
### set
```python
a = set()
a.add(1)
print("exist") if 1 in a else  print("not exist")
a.remove(1)
print("exist") if 1 in a else  print("not exist")
```

## 数组 array
### 2维数组
❌以下写法初始化数组会导致每一行的数组都是参照同一行的数据，也就是改了某一行的数据其他行的数据也会一起改变  
```python
rows, cols = (5, 5)
arr = [[0]*cols]*rows
print(arr, "before")
 
arr[0][0] = 1 # update only one element
print(arr, "after")
```
```
([[0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]], 'before')
([[1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0]], 'after')
```
⭕这种写法就没有问题  
```python
arr = [[0 for i in range(cols)] for j in range(rows)]
```
https://www.geeksforgeeks.org/python-using-2d-arrays-lists-the-right-way/  
### flatten
```python
# numpy方法
import numpy as np
np.array([[1,2,3],[4,5,6],[7,8,9]]).flatten()  #=> array([1, 2, 3, 4, 5, 6, 7, 8, 9])

# sum
sum([[1,2,3],[4,5,6],[7,8,9]], [])  #=> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
## stack
可以用普通list跟append和pop实现   
```python
stack = []
stack.append(1) # [1]
stack.append(2) # [1, 2]
stack.append(3) # [1, 2, 3]

stack.pop() # [1, 2]  
stack.pop() # [1]
stack.pop() # []
```
双向队列与pop()  
```python
from collections import deque

dq = deque()
dq.append(1) # [1]
dq.append(2) # [1, 2]
dq.append(3) # [1, 2, 3]

dq.pop() # [1, 2]  
dq.pop() # [1]
dq.pop() # []
```
## 队列 queue
跟stack类似，但是用pop(0)  
```python
queue = []
queue.append(1) # [1]
queue.append(2) # [1, 2]
queue.append(3) # [1, 2, 3]

queue.pop(0) # [2, 3]  
queue.pop(0) # [3]
queue.pop(0) # []
```
双向队列与popleft()  
```python
from collections import deque

dq = deque()
dq.append(1) # [1]
dq.append(2) # [1, 2]
dq.append(3) # [1, 2, 3]

dq.popleft() # [2, 3]  
dq.popleft() # [3]
dq.popleft() # []
```

## heap
```python
import heapq
h = [1,3,5,7,9,2,4,6,8,0]
heapq.heapify(h)
for i in range(len(h)):
    print(heapq.heappop(h)) # 1 2 3 4 5 6 7 8 9

x = []
for i in [1,3,5,7,9,2,4,6,8,0]:
    heapq.heappush(x,i)
while len(x) > 0:
    print(heapq.heappop(x)) # 1 2 3 4 5 6 7 8 9
```

## 字符串 string
### 正则表达
- `(...)`
  用于捕获分组，匹配后的结果会放在`groups()`里，`group(0)`是无分组，`group(x)`则是第x个`(...)`里的结果  
  ```python
  mail = "abc123@mail.com"
  pattern = "([\w]*)@([a-zA-Z0-9\.]*)"
  print(re.search(pattern, mail).group(0,1,2))

  ('abc123@mail.com','abc123','mail.com')
  ```
  
### 输出
If you don’t want characters prefaced by \ to be interpreted as special characters, you can use raw strings by adding an r before the first quote:  
```python
>>> print('C:\some\name')  # here \n means newline!
C:\some
ame

>>> print(r'C:\some\name')  # note the r before the quote
C:\some\name

>>> s = 'C:\some\name'
>>> print(repr(s)) # 转化变量写法
C:\some\name
```
https://docs.python.org/3/tutorial/introduction.html#strings  

### 按字母表顺序排序
用sorted会变成list，所以要用join再连接起来
```python
>>> print(' and '.join(["apple","orange","banana"]))
apple and orange and banana

>>> print(sorted("apple"))
['a', 'e', 'l', 'p', 'p']

>>> print(''.join(sorted("apple")))
aelpp
```

### 修改字符串
```python
>>> a = "Hello, World!"

>>> print(a.upper())
HELLO, WORLD!

>>> print(a.lower())
hello, world!

>>> b = " H e l l o , W o r l d !"
>>> print(b.strip()) # 去除头尾空格
H e l l o , W o r l d !

>>> print(a.replace("H", "J")) 
Jello, World!

>>> print(a.split(","))
[' Hello', ' World! ']

>>> print(a[:4]+"a"+a[5:]) # 修改一部分字符,直接给a[4]赋值会出错.
Helao, World!
```

### Unicode转换
```python
>>> ord("a")
97
>>> chr(97)
‘a’
```

# environment
## `__init__.py`
用于标记库，使得该路径的程序可以被其他程序调用  
一个文件夹根目录下存在 __init__.py 那就会认为该文件夹是Python包，否则那这个文件夹就是一个普通的文件夹  
参考：https://stackoverflow.com/questions/448271/what-is-init-py-for  
