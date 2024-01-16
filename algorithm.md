# 复杂度
不同复杂度的增长速度  
![speed](https://github.com/KnnUUu/note/assets/44579350/e897070c-222c-4393-9e30-5543bfafb13b)

# 数据结构
## Tree
- 有序
- search，insert, erase O(Log n)   
- C++ std::map / ~Python OrderedDict()~ OrderedDict只是记录插入顺序而无法按照键或者值排序    
- 底层 red-black trees  
## Hash
- 无序  
- Search，insert，delete  
  O(1) -> Average  
  O(n) -> Worst Case  
- C++ std::unordered_map /std::unordered_set / Python dict() set()
- 因为自带的hash无法计算一些数据型（比如unordered_set<pair<int, int>>），所以当使用自定义类型时需要另外定义hash函数，面试竞赛时还是用vector吧
## Heap
- 父节点总是比子节点大（or小），所以根节点是最大（or最小）节点  
- get min or max O(1), insert worse O(logN) average O(1), heapify a list O(N)   
- C++ std::priority_queue / Python heappush(list, value) heappop(list, value) heapify(listy)   
- 为什么heapify时间复杂度O(N)  
  如果建立一个空二叉树然后依次插入的话因为维护每次需要O(logN)所以总共是O(NlogN)    
  如果是按照默认顺序生成二叉树，然后由底层向高层遍历维护heap的话就只需要O(N)  
## List
- Random access, append O(1), insert O(N)  
- C++ std::vector/ Python list  

# 算法
## 递归 Recursion  

## 动态规划 Dynamic Programming  
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

## 二分查找  
### 二分查找边界条件  
https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-de-xun-huan-bu-bian-liang-zhi-yao-/
```python
# 查找左侧边界
def leftBound(nums, target):
    l = 0, r = len(nums)-1
    while l<=r:
        mid = l+(r-l)//2 # 如果两个int都很大相加则可能溢出所以使用这种写法
        if nums[mid] >= target:
            r = mid - 1
        else: #nums[mid] < target
            l = mid + 1
    
    if l<0 or l>=len(nums) or nums[l]!=target:
        # target not found
        return -1
    else:
        # 循环中止条件是r+1==l，此时l指向左边边界
        return nums[l]
```

## BFS&DFS  
### BFS vs DFS  
BFS可以用于寻找最短路径，但是空间复杂度比DFS高  
二叉树的情况下，DFS最坏是遍历到最深一层也就是O(logN)，但BDS遍历到最深一层是O(N/2)==O(N)  
### 寻找最短路径 模板
```python
# 计算从起点 start 到终点 target 的最近距离
def BFS(Node start, Node target):
    if root == None:
        # edge case
         
    nodeQueue = []
    visited = set()
    visited.add(root)
    depth = 1

    while len(nodeQueue)>0:
        queueLen = len(nodeQueue)
        for i in range(queueLen):            
            tempNode = nodeQueue.pop(0)
            if tempNode is target:
                return depth
            for adjNode in tempNode.adj():
                if adjNode not in visited:
                    nodeQueue.append(adjNode)
                    visited.add(adjNode)
        depth+=1
    
    # target not found
    error()
```
参考：https://github.com/labuladong/fucking-algorithm/blob/master/%E7%AE%97%E6%B3%95%E6%80%9D%E7%BB%B4%E7%B3%BB%E5%88%97/BFS%E6%A1%86%E6%9E%B6.md
### 为什么BFS&DFS时间复杂度是O(E+V)？
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
参考：https://stackoverflow.com/questions/26549140/breadth-first-search-time-complexity-analysis / https://stackoverflow.com/questions/11468621/why-is-the-time-complexity-of-both-dfs-and-bfs-o-v-e   

### 例子
- 滑动拼图  
  https://leetcode.com/problems/sliding-puzzle/description/  
  将二位数组扁平化，作为字符串保存  
  上下左右四个cell便是邻接点  

## 花式遍历
发散思维，尝试翻转、镜像等获得思路  

### 例子
- 原地反转所有单词的顺序    
  输入：`s = "hello world labuladong"`  
  输出：`s = "labuladong world hello"`  
  解法：先反转整个字符串，再按照各个单词反转回来  

- 旋转矩阵  
  输入  
  ```
  [[1,2,3],
  [4,5,6],
  [7,8,9]]
  ```
  输出  
  ```
  [[7,4,1],
  [8,5,2],
  [9,6,3]]
  ```
  思路：旋转二维矩阵的难点在于将「行」变成「列」，将「列」变成「行」，而只有按照对角线的对称操作是可以轻松完成这一点  
  解法：先按照对角线对称一下（也就是xy互换），然后在中间对称一下

- 矩阵的螺旋遍历  
  输入  
  ```
  [[1,2,3],
  [4,5,6],
  [7,8,9]]
  ```
  输出：`[1,2,3,4,5,6,7,8,9]`  
  解法：有上下左右四个边界，每次遍历完一行或者一列将对应的边界缩小一层，沿着边界遍历即可  
  
## 回溯（huísù）法 BackTracking  
本质是是遍历一棵决策树，每个叶子节点存放着一个合法答案，遍历所有叶子节点就能收集所有的合法答案  
```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```
### 例子
```java
class Solution {
    List<List<Integer>> res = new LinkedList<>();

    /* 主函数，输入一组不重复的数字，返回它们的全排列 */
    List<List<Integer>> permute(int[] nums) {
        // 记录「路径」
        LinkedList<Integer> track = new LinkedList<>();
        // 「路径」中的元素会被标记为 true，避免重复使用
        boolean[] used = new boolean[nums.length];
        
        backtrack(nums, track, used);
        return res;
    }

    // 路径：记录在 track 中
    // 选择列表：nums 中不存在于 track 的那些元素（used[i] 为 false）
    // 结束条件：nums 中的元素全都在 track 中出现
    void backtrack(int[] nums, LinkedList<Integer> track, boolean[] used) {
        // 触发结束条件
        if (track.size() == nums.length) {
            res.add(new LinkedList(track));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            // 排除不合法的选择
            if (used[i]) {
                // nums[i] 已经在 track 中，跳过
                continue;
            }
            // 做选择
            track.add(nums[i]);
            used[i] = true;
            // 进入下一层决策树
            backtrack(nums, track, used);
            // 取消选择
            track.removeLast();
            used[i] = false;
        }
    }
}
```
## KMP算法  
用于字符串匹配：字符串 P 是否为字符串 S 的子串？如果是，它出现在 S 的哪些位置？  

### LCS (longest common string)  
File comparison.  
The Unix program "diff" is used to compare two different versions of the same file, to determine what changes have been made to the file.  
It works by finding a longest common subsequence of the lines of the two files;  
any line in the subsequence has not been changed, so what it displays is the remaining set of lines that have changed.  
In this instance of the problem we should think of each line of a file as being a single complicated character in a string.    
