# LeetCode 685. Redundant Connection II's Solution
LeetCode link: [685. Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii)

## LeetCode problem description
In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[ui, vi]` that represents a directed edge connecting nodes `ui` and `vi`, where `ui` is a parent of child `vi`.

Return _an edge that can be removed so that the resulting graph is a rooted tree of `n` nodes_. If there are multiple answers, return the answer that occurs last in the given 2D-array.

### Example 1
![](../../images/examples/685_1.jpg)
```java
Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
```

### Example 2
![](../../images/examples/685_2.jpg)
```java
Input: edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
Output: [4,1]
```

### Constraints
- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`

## Intuition
- Because a cycle is formed, the directed tree is no longer a directed tree. There are two cases to consider:
    1. If there is a vertex with in-degree 2, it will form a cycle. So one of the edges needs to be removed (returned).
    2. If there is no vertex with in-degree 2, once a cycle is formed, return the edge that causes the cycle.

- We are given `edges` data and need to divide them into multiple groups, each group can be abstracted into a **tree**.
- Finally, those trees can be merged into one tree if the redundant edge is removed.
- `UnionFind` algorithm is designed for grouping and searching data.

### 'UnionFind' algorithm
- `UnionFind` algorithm typically has three methods:
    - The `unite(node1, node2)` operation is used to merge two trees.
    - The `find_root(node)` method is used to return the root of a node.
    - The `is_same_root(node1, node2)` method is used to determine whether two nodes are in the same tree.

## Approach
1. Iterate `edges` data to look for the `two_conflict_edges` (the two edges caused a vertex with in-degree 2).
1. Initially, each node is in its own group.
1. Iterate `edges` data and `unite(node1, node2)`.
1. If there is no vertex with in-degree 2, as soon as `is_same_root(node1, node2) == true` (a cycle will be formed), return `[node1, node2]`.
1. If there is a vertex with in-degree 2, we need to determine which edge in `two_conflict_edges` should be returned.
See if the graph can form a cycle by not adding the second edge to the graph. If so, return the first edge. Otherwise, return the second edge.

## Complexity
* Time: `O(n)`.
* Space: `O(n)`.

## Python
```python
class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        self.parents = list(range(len(edges) + 1))

        two_conflict_edges_ = self.two_conflict_edges(edges)

        if not two_conflict_edges_:
            for x, y in edges:
                if self.is_same_root(x, y):
                    return [x, y]

                self.unite(x, y)

            raise Exception('No suitable edge was returned!')

        for x, y in edges:
            if [x, y] == two_conflict_edges_[1]:
                continue

            if self.is_same_root(x, y):
                return two_conflict_edges_[0]

            self.unite(x, y)

        return two_conflict_edges_[1]
    
    def two_conflict_edges(self, edges):
        pointed_node_to_source_node = {}

        for source_node, pointed_node in edges:
            if pointed_node in pointed_node_to_source_node:
                return [
                    [pointed_node_to_source_node[pointed_node], pointed_node],
                    [source_node, pointed_node],
                ]

            pointed_node_to_source_node[pointed_node] = source_node

        return []

    def unite(self, x, y):
        root_x = self.find_root(x)
        root_y = self.find_root(y)

        self.parents[root_y] = root_x
    
    def find_root(self, x):
        parent = self.parents[x]

        if x == parent:
            return x
        
        root = self.find_root(parent)

        self.parents[x] = root

        return root
    
    def is_same_root(self, x, y):
        return self.find_root(x) == self.find_root(y)
```

## Java
```java
// Welcome to create a PR to complete the code of this language, thanks!
```

## Python
```python
// Welcome to create a PR to complete the code of this language, thanks!
```

## C++
```cpp
// Welcome to create a PR to complete the code of this language, thanks!
```

## JavaScript
```javascript
// Welcome to create a PR to complete the code of this language, thanks!
```

## C#
```c#
// Welcome to create a PR to complete the code of this language, thanks!
```

## Go
```go
// Welcome to create a PR to complete the code of this language, thanks!
```

## Ruby
```ruby
# Welcome to create a PR to complete the code of this language, thanks!
```

## C
```c
// Welcome to create a PR to complete the code of this language, thanks!
```

## Kotlin
```kotlin
// Welcome to create a PR to complete the code of this language, thanks!
```

## Swift
```swift
// Welcome to create a PR to complete the code of this language, thanks!
```

## Rust
```rust
// Welcome to create a PR to complete the code of this language, thanks!
```

## Other languages
```
// Welcome to create a PR to complete the code of this language, thanks!
```
