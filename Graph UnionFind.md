# Leetcode Graph Union Find

```Python
class UnionFind():
  '''
  Path Compression
  Union by Rank
  '''
  def __init__(self, size: int):
      '''
      Time: O(n)
      '''
      self.root = [i for i in range(size)]
      self.rank = [0] * size

  def find(self, a: int):
      '''
      Time: O(alpha(n))
      '''
      if self.root[a] != a:
        self.root[a] = self.find(self.root[a])
  
      return self.root[a]

  def union(self, a: int, b: int):
      '''
      Time: O(alpha(n))
      '''
      root_a = find(a)
      root_b = find(b)
  
      if root_a != root_b:
        if self.rank[root_a] > self.rank[root_b]:
          self.root[root_b] = root_a
        elif self.rank[root_a] < self.rank[root_b]:
          self.root[root_a] = root_b
        else:
          self.root[root_b] = root_a
          self.rank[root_a] += 1

  def connected(self, a: int, b: int) -> bool:
      '''
      Time: O(alpha(n))
      '''
      return self.find(a) == self.find(b)
```

# 547. Number of Provinces

### Problem Statement
There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `i^th` city and the `j^th` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return *the total number of **provinces***.

### Examples

#### Example 1:
```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

#### Example 2:
```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```

#### Constraints:
* `1 <= n <= 200`
* `n == isConnected.length`
* `n == isConnected[i].length`
* `isConnected[i][j]` is `1` or `0`
* `isConnected[i][i] == 1`
* `isConnected[i][j] == isConnected[j][i]`

### 📌 Multiple Choice Questions: Union Find Approach  

#### 1️⃣ What data structure is used to implement the Union Find algorithm in this solution?  
- [ ] A) Queue
- [ ] B) Stack
- [ ] C) Binary Tree
- [ ] D) Disjoint Set (with arrays for root and rank)

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Disjoint Set (with arrays for root and rank)**  
  
</details>  

---

#### 2️⃣ What is the purpose of the "rank" array in the Union Find implementation?  
- [ ] A) To keep track of the number of elements in each set
- [ ] B) To optimize union operations by maintaining a balanced tree structure
- [ ] C) To record the distance between cities in a province
- [ ] D) To count the number of connections for each city

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) To optimize union operations by maintaining a balanced tree structure**  
</details>  

---

#### 3️⃣ What optimization technique is implemented in the `find` function?  
- [ ] A) Path compression
- [ ] B) Quick union
- [ ] C) Weighted quick find
- [ ] D) Breadth-first search

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) Path compression**  
</details>  

---

#### 4️⃣ How is the number of provinces calculated in this solution?  
- [ ] A) By counting the number of unique root nodes at the end
- [ ] B) By summing up all the values in the rank array
- [ ] C) By starting with n (total cities) and decrementing for each successful union operation
- [ ] D) By performing a depth-first search on the adjacency matrix

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) By starting with n (total cities) and decrementing for each successful union operation**  
</details>  

---

#### 5️⃣ What is the time complexity of this Union Find solution with path compression and union by rank?  
- [ ] A) O(n), where n is the number of cities
- [ ] B) O(n²)
- [ ] C) O(n² * α(n)), where α(n) is the inverse Ackermann function
- [ ] D) O(n³)

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(n² * α(n)), where α(n) is the inverse Ackermann function**  
</details>  

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        '''
        Approach: Union Find

        '''

        root = [i for i in range(len(isConnected))]
        rank = [1] * len(isConnected)

        def find(x):
            '''
            Path Compression
            '''
            if x == root[x]:
                return x
            
            root[x] = find(root[x])
            
            return root[x]
        
        def union(x, y):
            '''
            '''

            rootx = find(x)
            rooty = find(y)
            if rootx == rooty:
                return 0
            else:
                if rank[rootx] > rank[rooty]:
                    root[rooty] = rootx
                elif rank[rootx] < rank[rooty]:
                    root[rootx] = rooty
                else:
                    root[rootx] = rooty
                    rank[rooty] += 1
                
                return 1
        
        res = len(isConnected)

        for i in range(len(isConnected)):
            for j in range(len(isConnected[i])):
                if isConnected[i][j]:
                    res -= union(i, j)
        
        return res
```

# 323. Number of Connected Components in an Undirected Graph

### Problem Statement
You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.

Return *the number of connected components in the graph*.

### Examples

#### Example 1:
```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

#### Example 2:
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

#### Constraints:
* `1 <= n <= 2000`
* `1 <= edges.length <= 5000`
* `edges[i].length == 2`
* `0 <= ai <= bi < n`
* All the given edges are **unique**.

### 📌 Multiple Choice Questions: Union Find Approach  

#### 1️⃣ What is the main difference between this problem and the "Number of Provinces" problem?  
- [ ] A) This problem uses a different algorithm
- [ ] B) This problem represents the graph with an edge list instead of an adjacency matrix
- [ ] C) This problem requires finding connected components while Provinces finds clusters
- [ ] D) There is no fundamental difference in the problems

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) This problem represents the graph with an edge list instead of an adjacency matrix**  
  
</details>  

---

#### 2️⃣ What is the initialization of the Union Find data structure in this problem?  
- [ ] A) Each node starts as its own root and has a rank of 1
- [ ] B) All nodes start with the same root
- [ ] C) Only nodes with edges are initialized
- [ ] D) Initialization depends on the input graph structure

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) Each node starts as its own root and has a rank of 1**  
</details>  

---

#### 3️⃣ How does the code determine the final number of connected components?  
- [ ] A) By counting the number of nodes with no edges
- [ ] B) By performing depth-first search on each node
- [ ] C) By starting with n components and decrementing for each successful union
- [ ] D) By counting the number of unique roots at the end

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) By starting with n components and decrementing for each successful union**  
</details>  

---

#### 4️⃣ What optimization techniques are used in the Union Find implementation?  
- [ ] A) Path compression only
- [ ] B) Union by rank only
- [ ] C) Both path compression and union by rank
- [ ] D) Neither path compression nor union by rank

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Both path compression and union by rank**  
</details>  

---

#### 5️⃣ What is the time complexity of the given solution?  
- [ ] A) O(n), where n is the number of nodes
- [ ] B) O(e), where e is the number of edges
- [ ] C) O(e * α(n)), where e is the number of edges and α(n) is the inverse Ackermann function
- [ ] D) O(n * e), where n is the number of nodes and e is the number of edges

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(e * α(n)), where e is the number of edges and α(n) is the inverse Ackermann function**  
</details>  

```python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        def find(x):
            if root[x] == x:
                return x
            
            #Path Compression
           
            root[x] = find(root[x])
            
            return root[x]
        
        def union(x, y):
            rootx = find(x)
            rooty = find(y)

            if rootx == rooty:
                return 0
            
            if rank[rootx] > rank[rooty]:
                root[rooty] = rootx
            elif rank[rootx] < rank[rooty]:
                root[rootx] = rooty
            else:
                root[rooty] = rootx
                rank[rootx] += 1
            
            return 1
        
        
        root = [i for i in range(n)]
        rank = [1] * n

        res = n
        for a, b in edges:
            res -= union(a, b)
        
        return res
```

# 1101. The Earliest Moment When Everyone Become Friends

### Problem Statement
There are n people in a social group labeled from 0 to n-1. You are given a logs array where each `logs[i] = [timestamp_i, x_i, y_i]` indicates that x_i and y_i became friends at timestamp_i.

Friendship is symmetric. That means if x is friends with y, then y is friends with x. Also, a person is acquainted with oneself.

Return the earliest time when everyone in the group becomes friends with each other. If it's impossible for everyone to become friends, return -1.

### Examples

#### Example 1:
```
Input: logs = [[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[20190322,4,5]], n = 6
Output: 20190301
Explanation: 
The first event occurs at timestamp = 20190101, and after this, 0 and 1 are friends.
The second event occurs at timestamp = 20190104, and after this, 3 and 4 are friends.
The third event occurs at timestamp = 20190107, and after this, 2 and 3 are friends.
The fourth event occurs at timestamp = 20190211, and after this, 1 and 5 are friends.
The fifth event occurs at timestamp = 20190224, and after this, 2 and 4 are friends.
The sixth event occurs at timestamp = 20190301, and after this, 0 and 3 are friends.
The last friend which gets connected to all others is 0, and this happens at timestamp = 20190301.
```

#### Example 2:
```
Input: logs = [[0,2,0],[1,0,1],[3,0,3],[4,1,2],[7,3,1]], n = 4
Output: 3
```

#### Constraints:
* `2 <= n <= 100`
* `1 <= logs.length <= 10^4`
* `logs[i].length == 3`
* `0 <= timestamp_i <= 10^9`
* `0 <= x_i, y_i < n`
* `x_i != y_i`
* All the timestamps are distinct.
* All the pairs (x_i, y_i) are distinct.

### 📌 Multiple Choice Questions: Union Find Approach  

#### 1️⃣ What is the first step of the solution before applying the Union Find algorithm?  
- [ ] A) Initialize the root and rank arrays
- [ ] B) Find all connected components
- [ ] C) Check if n equals the number of logs
- [ ] D) Sort the logs array by timestamp

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Sort the logs array by timestamp**  
  Since we need to process the friendship connections in chronological order, we must first sort the logs by timestamp.
</details>  

---

#### 2️⃣ What is the termination condition for the algorithm to return a timestamp?  
- [ ] A) When all entries in the logs array are processed
- [ ] B) When at least half of the people are connected
- [ ] C) When there is exactly one connected component (res == 1)
- [ ] D) When the root array contains only one unique value

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) When there is exactly one connected component (res == 1)**  
  The algorithm returns the timestamp when everyone becomes friends, which happens when all people are in a single connected component (res == 1).
</details>  

---

#### 3️⃣ What does the solution return if it's impossible for everyone to become friends?  
- [ ] A) -1
- [ ] B) 0
- [ ] C) The maximum integer value
- [ ] D) The last timestamp in the logs array

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) -1**  
  If after processing all logs, there are still multiple connected components (res > 1), then it's impossible for everyone to become friends and the solution returns -1.
</details>  

---

#### 4️⃣ How does the algorithm track the number of connected components?  
- [ ] A) By counting unique roots in the root array
- [ ] B) By using a separate counter for each union operation
- [ ] C) By starting with n components and decrementing for each successful union
- [ ] D) By using the rank array to count components

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) By starting with n components and decrementing for each successful union**  
  The variable 'res' is initialized to n (where each person is their own component) and is decremented each time a successful union operation connects two previously separate components.
</details>  

---

#### 5️⃣ What is the overall time complexity of this solution?  
- [ ] A) O(n + m), where m is the number of logs
- [ ] B) O(m)
- [ ] C) O(m log m + m α(n)), where α(n) is the inverse Ackermann function
- [ ] D) O(n²)

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(m log m + m α(n)), where α(n) is the inverse Ackermann function**  
  The time complexity comes from sorting the logs array O(m log m) and then performing m union-find operations each with α(n) amortized time complexity due to path compression and union by rank.
</details>  

```python
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        '''
        Approach: Union Find
        
        1. Sort logs by timestamp
        2. Use Union Find to track when all people become connected
        3. Return the timestamp when everyone is in one connected component
        '''
        def find(x):
            if x == root[x]:
                return x
            
            root[x] = find(root[x])
            return root[x]
        
        def union(x, y):
            rootx = find(x)
            rooty = find(y)
            if rootx == rooty:
                return 0
            if rank[rootx] > rank[rooty]:
                root[rooty] = rootx
            elif rank[rootx] < rank[rooty]:
                root[rootx] = rooty
            else:
                root[rooty] = rootx
                rank[rootx] += 1
            
            return 1
        
        root = [i for i in range(n)]
        rank = [1] * n
        res = n
        logs.sort()
        for timestamp, a, b in logs:
            res -= union(a, b)
            if res == 1:
                return timestamp
        
        return -1
```
