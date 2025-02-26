# Tree

# 2467. Most Profitable Path in a Tree

### Problem Statement
There is an undirected tree with `n` nodes labeled from `0` to `n - 1`, rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

At every node `i`, there is a gate. You are also given an array of even integers `amount`, where `amount[i]` represents:
* the price needed to open the gate at node `i`, if `amount[i]` is negative, or,
* the cash reward obtained on opening the gate at node `i`, otherwise.

The game goes on as follows:
* Initially, Alice is at node `0` and Bob is at node `bob`.
* At every second, Alice and Bob **each** move to an adjacent node. Alice moves towards some **leaf node**, while Bob moves towards node `0`.
* For **every** node along their path, Alice and Bob either spend money to open the gate at that node, or accept the reward. Note that:
   * If the gate is **already open**, no price will be required, nor will there be any cash reward.
   * If Alice and Bob reach the node **simultaneously**, they share the price/reward for opening the gate there. In other words, if the price to open the gate is `c`, then both Alice and Bob pay `c / 2` each. Similarly, if the reward at the gate is `c`, both of them receive `c / 2` each.
* If Alice reaches a leaf node, she stops moving. Similarly, if Bob reaches node `0`, he stops moving. Note that these events are **independent** of each other.

Return *the **maximum** net income Alice can have if she travels towards the optimal leaf node.*

### Examples

#### Example 1:
```
Input: edges = [[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6]
Output: 6
Explanation: 
The above diagram represents the given tree. The game goes as follows:
- Alice is initially on node 0, Bob on node 3. They open the gates of their respective nodes.
  Alice's net income is now -2.
- Both Alice and Bob move to node 1. 
  Since they reach here simultaneously, they open the gate together and share the reward.
  Alice's net income becomes -2 + (4 / 2) = 0.
- Alice moves on to node 3. Since Bob already opened its gate, Alice's income remains unchanged.
  Bob moves on to node 0, and stops moving.
- Alice moves on to node 4 and opens the gate there. Her net income becomes 0 + 6 = 6.
Now, neither Alice nor Bob can make any further moves, and the game ends.
It is not possible for Alice to get a higher net income.
```

#### Example 2:
```
Input: edges = [[0,1]], bob = 1, amount = [-7280,2350]
Output: -7280
Explanation: 
Alice follows the path 0->1 whereas Bob follows the path 1->0.
Thus, Alice opens the gate at node 0 only. Hence, her net income is -7280.
```

#### Constraints:
* `2 <= n <= 10^5`
* `edges.length == n - 1`
* `edges[i].length == 2`
* `0 <= ai, bi < n`
* `ai != bi`
* `edges` represents a valid tree.
* `1 <= bob < n`
* `amount.length == n`
* `amount[i]` is an **even** integer in the range `[-10^4, 10^4]`.

### 📌 Multiple Choice Questions: Tree Traversal and Path Finding  

#### 1️⃣ What is the main approach used to determine Bob's path in the solution?  
- [ ] A) Breadth-First Search (BFS) with backtracking  
- [ ] B) Depth-First Search (DFS) with path recording  
- [ ] C) Dijkstra's algorithm to find the shortest path  
- [ ] D) Dynamic programming to optimize the path selection  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Depth-First Search (DFS) with path recording**  
  
  The solution uses DFS to find Bob's path from his starting node to node 0, recording the steps taken at each node.
</details>  

---

#### 2️⃣ Why does the solution use `bob_travels[node] = float('inf')` initially for all nodes?  
- [ ] A) To mark all nodes as unvisited in Bob's path  
- [ ] B) To indicate that Bob hasn't reached these nodes yet  
- [ ] C) To represent that Bob will never visit these nodes  
- [ ] D) To initialize a comparison value that ensures any actual step count will be smaller  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) To initialize a comparison value that ensures any actual step count will be smaller**  
  
  Using `float('inf')` allows the algorithm to easily determine if Alice reaches a node before Bob by comparing their step counts.
</details>  

---

#### 3️⃣ How does the solution determine the optimal leaf node for Alice to target?  
- [ ] A) By calculating the maximum potential reward for each leaf node beforehand  
- [ ] B) By simulating Alice's path to every leaf node and choosing the most profitable one  
- [ ] C) By using dynamic programming to compute the optimal path  
- [ ] D) By exploring all possible paths for Alice using BFS and tracking the maximum income  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) By exploring all possible paths for Alice using BFS and tracking the maximum income**  
  
  The solution uses BFS to explore all possible paths Alice can take, and for each path that reaches a leaf node, it updates the maximum income if necessary.
</details>  

---

#### 4️⃣ What is the significance of the condition `bob_travels[node] > step` in the solution?  
- [ ] A) It checks if Alice reaches the node before Bob  
- [ ] B) It verifies if Bob has already visited the node  
- [ ] C) It determines if Alice and Bob meet at the node  
- [ ] D) It validates if the node is on Bob's path to node 0  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) It checks if Alice reaches the node before Bob**  
  
  This condition compares Alice's step count with Bob's step count to determine if Alice reaches the node before Bob, allowing her to collect the full amount at that node.
</details>  

---

#### 5️⃣ Why is the condition `len(adj[node]) == 1 and node != 0` used to identify leaf nodes?  
- [ ] A) Because leaf nodes have exactly one connection in an undirected tree  
- [ ] B) To exclude internal nodes that have multiple children  
- [ ] C) To ensure we only count nodes that Alice can actually stop at  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) All of the above**  
  
  In an undirected tree, a leaf node has exactly one edge connecting it to the rest of the tree. The condition checks if the node has only one neighbor (one edge) and is not the root node (0), which ensures it's a leaf node where Alice can stop.
</details>  

```python
class Solution:
    def mostProfitablePath(self, edges: List[List[int]], bob: int, amount: List[int]) -> int:
        '''
        '''

        def dfs(node: int, step: int) -> bool:
            prev = bob_travels[node]
            bob_travels[node] = step
            bob_visited.add(node)

            if node == 0:
                return True
            
            for neighbor in adj[node]: 
                if neighbor != node and neighbor not in bob_visited:
                    if dfs(neighbor, step + 1):
                        return True
            bob_travels[node] = prev
            return False

        # Build adj list
        adj = [[] for _ in range(len(edges) + 1)]
        
        for a, b in edges:
            adj[a].append(b)
            adj[b].append(a)
        
        # Track Bob travels:
        
        bob_travels = [float('inf')] * (len(edges) + 1)
        bob_visited = set()
        dfs(bob, 0)
        
        queue = collections.deque([(0, 0, 0)]) #(pos, step, income)
        
        # Determine if node is visited by bob check if step in bob_travels is < step

        visited = {0} # Alice visited

        max_income = -float('inf')

        while queue:
            
            node, step, income = queue.popleft()
            if bob_travels[node] > step:
                income += amount[node]
            
            elif bob_travels[node] == step:
                income += amount[node] // 2

            if len(adj[node]) == 1 and node != 0: # Ensure it is a leaf node
                max_income = max(max_income, income)

            for neighbor in adj[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append((neighbor, step + 1, income))
            
        return max_income
```
