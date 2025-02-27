# Leetcode Graph Union Find

```Python
class UnionFind():
  def __init__(self, size: int):
    self.root = [i for i in range(size)]
    self.rank = [0] * size

  def find(self, a: int):
    if self.root[a] != a:
      self.root[a] = self.find(self.root[a])

    return self.root[a]

  def union(self, a: int, b: int):
    root_a = find(a)
    root_b = find(b)

    if root_a != root_b:
      if self.rank[root_a] > self.rank[root_b]:
        self.root[root_b] = root_a
      elif self.rank[root_a]
```
