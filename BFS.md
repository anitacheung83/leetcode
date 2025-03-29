# Breath First Search

## Problems
1. [2503. Maximum Number of Points From Grid Queries](#2503-maximum-number-of-points-from-grid-queries)

# 2503. Maximum Number of Points From Grid Queries

### Problem Statement
You are given an `m x n` integer matrix `grid` and an array of integer `queries` of size `q`.

For each query `queries[i]`, you need to find the maximum number of points you can collect by starting at cell `(0, 0)` and only moving to cells with values strictly less than `queries[i]`.

You can move up, down, left, or right from a cell to an adjacent cell in the grid. Return an array `answer` where `answer[i]` is the answer to the `i`th query.

### Examples

#### Example 1:
```
Input: grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
Output: [5,8,1]
```

#### Example 2:
```
Input: grid = [[5,2,1],[1,1,2]], queries = [3]
Output: [0]
```

#### Constraints:
* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 1000`
* `1 <= grid[i][j] <= 10^6`
* `1 <= queries.length <= 10^4`
* `1 <= queries[i] <= 10^6`

### 📌 Multiple Choice Questions: Grid BFS with Priority Queue Approach  

#### 1️⃣ What is the main data structure used to solve this problem efficiently?  
- [ ] A) Regular queue for BFS  
- [ ] B) Min-heap (priority queue)  
- [ ] C) Union-find (disjoint set)  
- [ ] D) Segment tree  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Min-heap (priority queue)**  
  
</details>  

---

#### 2️⃣ Why do we sort the queries before processing them?  
- [ ] A) To optimize the time complexity to O(q log q)  
- [ ] B) To avoid recalculating the BFS for each query  
- [ ] C) To process queries in increasing order, allowing us to reuse previous results  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) All of the above**  
</details>  

---

#### 3️⃣ What does the `count` variable represent in the solution?  
- [ ] A) The number of queries processed  
- [ ] B) The number of cells visited with values less than the current query  
- [ ] C) The size of the min-heap  
- [ ] D) The maximum value in the grid  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) The number of cells visited with values less than the current query**  
</details>  

---

#### 4️⃣ Why do we use a min-heap instead of a regular queue for the BFS?  
- [ ] A) To visit cells in order of increasing values  
- [ ] B) To optimize the space complexity  
- [ ] C) To handle the queries more efficiently  
- [ ] D) Because this problem requires Dijkstra's algorithm  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) To visit cells in order of increasing values**  
</details>  

---

#### 5️⃣ What is the time complexity of this solution?  
- [ ] A) O(q log q + m*n log(m*n)), where q is the number of queries and m*n is the grid size  
- [ ] B) O(q * m*n), where q is the number of queries and m*n is the grid size  
- [ ] C) O(m*n + q log q), where q is the number of queries and m*n is the grid size  
- [ ] D) O(q log q + m*n), where q is the number of queries and m*n is the grid size  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) O(q log q + m*n log(m*n)), where q is the number of queries and m*n is the grid size**  
</details>  

```python
class Solution:
    def maxPoints(self, grid: List[List[int]], queries: List[int]) -> List[int]:
        
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        min_heap = [(grid[0][0], 0, 0)]
        visited = {(0, 0)}
        sorted_queries = sorted([(query, idx) for idx, query in enumerate(queries)])
        res = [0] * len(queries)
        count = 0

        for query, idx in sorted_queries:
            
            while min_heap and min_heap[0][0] < query:
                value, i, j = heapq.heappop(min_heap)
                count += 1

                for r, c in directions:
                    dr, dc = r + i, c + j

                    if 0 <= dr < len(grid) and 0 <= dc < len(grid[0]) and (dr, dc) not in visited:
                        heapq.heappush(min_heap, (grid[dr][dc], dr, dc))
                        visited.add((dr, dc))
            
            res[idx] = count
        
        return res
```
