## 复杂度
不同复杂度的增长速度  
![speed](https://github.com/KnnUUu/note/assets/44579350/e897070c-222c-4393-9e30-5543bfafb13b)

## 数据结构
### Tree
- 有序
- search，insert, erase O(Log n)   
- C++ std::map / Python OrderedDict()  
- 底层 red-black trees  
### Hash
- 无序  
- Search，insert，delete  O(1) -> Average  O(n) -> Worst Case
- C++ std::unordered_map /std::unordered_set / Python dict() set()
- 因为自带的hash无法计算一些数据型（比如unordered_set<pair<int, int>>），所以当使用自定义类型时需要另外定义hash函数，面试竞赛时还是用vector吧
### Heap
- Pop(to get min or max) O(1), insert worse O(logN) average O(1), heapify a list O(N) 
- C++ std::priority_queue / Python heappush(list, value) heappop(list, value) heapify(listy) 
- 为什么heapify时间复杂度O(N)  
  如果建立一个空二叉树然后依次插入的话因为维护每次需要O(logN)所以总共是O(NlogN)  
  但如果是从后最后一个父节点到根，每次只维护父节点跟左右两个子节点的话，就可以用O(N)完成  
### List
- Random access, append O(1), insert O(N)  
- C++ std::vector/ Python list  

## 算法
- 递归 Recursion  

- 动态规划 Dynamic Programming  
  Dynamic programming amounts to breaking down an optimization problem into simpler sub-problems, and storing the solution to each sub-problem so that each sub-problem is only solved once.  
  动态规划与递归的区别:都是将原问题拆成多个子问题然后求解，但动态规划保存了子问题的解，避免重复计算

- Greedy VS dynamic programming VS divide and conquer  
  贪心算法中，是以自顶向下的方式使用最优子结构的  
  贪心算法会先做选择，在当时看起来是最优的选择，然后再求解一个结果子问题，而不是先寻找子问题的最优解，然后再做选择  
  
  分治法(Divide-and-Conquer) : 将原问题划分成n个规模较小而结构与原问题相似的子问题  
  递归地解决这些子问题，然后再合并其结果，就得到原问题的解  
  分治模式在每一层递归上都有三个步骤：  
  - 分解(Divide)：将原问题分解成一系列子问题  
  - 解决(Conquer)：递归地解决各个子问题。若子问题足够小，则直接求解  
  - 合并(Combine)：将子问题的结果合并成原问题的解  
  例子：merge sort  

- 二分查找  
  二分查找边界条件  
  https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-de-xun-huan-bu-bian-liang-zhi-yao-/  

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

- 回溯（huísù）法 BackTracking  
  Backtracking is an improvement to the brute force approach.  
  removing those solutions that fail to satisfy the constraints of the problem at any point in time  
  Backtracking can be defined as a general algorithmic technique that considers searching every possible combination in order to solve a computational problem.  

- KMP算法  
  用于字符串匹配：字符串 P 是否为字符串 S 的子串？如果是，它出现在 S 的哪些位置？  

- LCS (longest common string)  
  File comparison.  
  The Unix program "diff" is used to compare two different versions of the same file, to determine what changes have been made to the file.  
  It works by finding a longest common subsequence of the lines of the two files;  
  any line in the subsequence has not been changed, so what it displays is the remaining set of lines that have changed.  
  In this instance of the problem we should think of each line of a file as being a single complicated character in a string.    
