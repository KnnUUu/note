### 正则表达
- `(...)`
  用于捕获分组，匹配后的结果会放在`groups()`里，`group(0)`是无分组，`group(x)`则是第x个`(...)`里的结果  
  ```python
  mail = "abc123@mail.com"
  pattern = "([\w]*)@([a-zA-Z0-9\.]*)"
  print(re.search(pattern, mail).group(0,1,2))

  ('abc123@mail.com','abc123','mail.com')
  ```
