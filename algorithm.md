- 递归 Recursion  

- 动态规划 Dynamic Programming  
  动态规划与递归的区别:都是将原问题拆成多个子问题然后求解，但动态规划保存了子问题的解，避免重复计算

- BFS  
  为什么BFS时间复杂度是O(E+V)？
  ```python
  def bfs(visited, graph, node): #function for BFS
    visited.append(node)
    queue.append(node)
  
    while queue:          # queue最坏会放进所有顶点所以以下的操作都是执行V次
      m = queue.pop(0) #V
      for neighbour in graph[m]:
        if neighbour not in visited: # 以下处理会执行各个顶点的临边次，这里标记为e(v)
          visited.append(neighbour)
          queue.append(neighbour)
  ```
  总时间复杂度  
  = O(V+V*e(v))  
  = O(V+E)
  参考：https://stackoverflow.com/questions/26549140/breadth-first-search-time-complexity-analysis  
