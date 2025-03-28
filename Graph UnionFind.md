# Graph Union Find

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

## Questions
1. [547. Number of Provinces](#547-number-of-provinces)
2. [323. Number of Connected Components in an Undirected Graph](#323-number-of-connected-components-in-an-undirected-graph)
3. [1101. The Earliest Moment When Everyone Become Friends](#1101-the-earliest-moment-when-everyone-become-friends)
4. [1202. Smallest String With Swaps](#1202-smallest-string-with-swaps)


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
# 1202. Smallest String With Swaps

### Problem Statement
You are given a string `s` and an array of pairs `pairs` where `pairs[i] = [a, b]` indicates that you can swap the characters at indices `a` and `b` in the string.

You can perform multiple swaps on the string. The goal is to make the string as small as possible in lexicographical order using the given swap pairs.

Return the lexicographically smallest string that can be obtained by applying the given swaps.

### Examples

#### Example 1:
```
Input: s = "dcab", pairs = [[0,3],[1,2]]
Output: "bacd"
Explanation: 
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"
```

#### Example 2:
```
Input: s = "dcab", pairs = [[0,3],[1,2],[0,2]]
Output: "abcd"
Explanation: 
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"
Swap s[0] and s[2], s = "abcd"
```

#### Example 3:
```
Input: s = "cba", pairs = [[0,1],[1,2]]
Output: "abc"
Explanation: 
Swap s[0] and s[1], s = "bca"
Swap s[1] and s[2], s = "bac"
Swap s[0] and s[1], s = "abc"
```

#### Constraints:
* `1 <= s.length <= 10^5`
* `0 <= pairs.length <= 10^5`
* `0 <= pairs[i][0], pairs[i][1] < s.length`
* `s` only contains lowercase English letters.

### 📌 Multiple Choice Questions: Union Find Approach  

#### 1️⃣ What insight about the swap operations is crucial for solving this problem?  
- [ ] A) Swaps should be performed in a specific order
- [ ] B) Only adjacent characters can be effectively swapped
- [ ] C) Characters in the same connected component can be rearranged in any order
- [ ] D) Each character can only be swapped once

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Characters in the same connected component can be rearranged in any order**  
  
  If there is a path of swaps connecting indices i and j, then characters at those positions can be swapped through a series of operations. This means all characters in a connected component can be rearranged in any order.
</details>  

---

#### 2️⃣ How are the characters grouped after the Union-Find operation?  
- [ ] A) By their parent/root indices in the disjoint set
- [ ] B) By their original order in the string
- [ ] C) By their lexicographical order
- [ ] D) By the frequency of swap operations

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) By their parent/root indices in the disjoint set**  
  
  After Union-Find, characters are grouped by their parent (root) indices, which represent the connected components formed by the swap pairs.
</details>  

---

#### 3️⃣ What data structure is used to store the grouped characters and their indices?  
- [ ] A) Array of lists
- [ ] B) Set of tuples
- [ ] C) Dictionary with tuple values (lists of indices and characters)
- [ ] D) Priority queue

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Dictionary with tuple values (lists of indices and characters)**  
  
  The solution uses a defaultdict where keys are the root indices and values are tuples containing two lists: one for original indices and one for characters.
</details>  

---

#### 4️⃣ What is the purpose of sorting both indices and characters within each group?  
- [ ] A) To ensure the original character order is preserved
- [ ] B) To place the smallest characters at the smallest available indices
- [ ] C) To minimize the number of swaps needed
- [ ] D) To verify the correctness of the Union-Find algorithm

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) To place the smallest characters at the smallest available indices**  
  
  Sorting both indices and characters ensures that within each connected component, the smallest characters will be placed at the smallest indices, which helps achieve the lexicographically smallest string.
</details>  

---

#### 5️⃣ What is the time complexity of this solution?  
- [ ] A) O(n + m), where n is the string length and m is the number of pairs
- [ ] B) O(n log n)
- [ ] C) O(n + m + k log k), where k is the maximum size of a connected component
- [ ] D) O(n^2)

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(n + m + k log k), where k is the maximum size of a connected component**  
  
  The time complexity comes from: processing the string (O(n)), union-find operations (O(m α(n)) which is effectively O(m)), and sorting characters within each component (O(k log k) for each component, where k is the component size).
</details>  

```python
class Solution:
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        '''
        Approach: Union-Find with Component Sorting
        
        1. Use Union-Find to identify connected components
        2. Group characters by their connected components
        3. Sort characters within each component
        4. Reconstruct the string by placing sorted characters at their original positions
        '''
        def find(x):
            if x == root[x]:
                return x
            
            root[x] = find(root[x])
            
            return root[x]
        
        def union(x, y):
            rootx = find(x)
            rooty = find(y)

            if rootx != rooty:
                if rank[x] > rank[y]:
                    root[rooty] = rootx
                
                elif rank[x] < rank[y]:
                    root[rootx] = rooty
                
                else:
                    root[rooty] = rootx
                    rank[rootx] += 1
        
        root = [i for i in range(len(s))]
        rank = [1] * len(s)

        # 1. Union-Find to connect all indices that can be swapped
        for a, b in pairs:
            union(a, b)

        # 2. Grouping indices and characters by their connected components
        group = defaultdict(lambda: ([], []))

        for i, ch in enumerate(s):
            parent = find(i)
            group[parent][0].append(i)
            group[parent][1].append(ch)
        
        # 3. Sorting indices and characters within each component
        res = [''] * len(s)

        for idxs, chars in group.values():
            idxs.sort()
            chars.sort()

            # 4. Reconstruct the string by placing characters optimally
            for ch, i in zip(chars, idxs):
                res[i] = ch
        
        return ''.join(res)
```
