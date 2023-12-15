## cheap sheet
没有`null`只有`None`   
`True` `False` 头字母大写  

## OOP
```python
class Dog:
    kind = 'canine'         # class variable shared by all instances
    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
```

## symtax  
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

## hash
### set
```python
s = set()
a.add(1)
if 1 in a: print("exist")
```

## 数组 array
### 2维数组
以下写法初始化数组会导致每一行的数组都是参照同一行的数据，也就是改了某一行的数据其他行的数据也会一起改变  
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
这种写法就没有问题  
```python
arr = [[0 for i in range(cols)] for j in range(rows)]
```
https://www.geeksforgeeks.org/python-using-2d-arrays-lists-the-right-way/  

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
```
https://docs.python.org/3/tutorial/introduction.html#strings  

### 排序
用sorted会变成list，所以要用join再连接起来
```python
>>> print(' and '.join(["apple","orange","banana"]))
apple and orange and banana

>>> print(sorted("apple"))
['a', 'e', 'l', 'p', 'p']

>>> print(''.join(sorted("apple")))
aelpp
```
