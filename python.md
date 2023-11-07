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
set.add()

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
