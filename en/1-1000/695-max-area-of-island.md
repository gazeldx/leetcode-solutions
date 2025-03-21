# 695. Max Area of Island (Solution 1: 'Depth-First Search' by Recursion)
LeetCode link: [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

# LeetCode problem description
You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The `area` of an island is the number of cells with a value `1` in the island.

Return _the maximum area of an island_ in `grid`. If there is no island, return `0`.

## Example 1
![](../../images/examples/695_1.jpg)
```
Input: grid = [
    [0,0,1,0,0,0,0,1,0,0,0,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,1,1,0,1,0,0,0,0,0,0,0,0],
    [0,1,0,0,1,1,0,0,1,0,1,0,0],
    [0,1,0,0,1,1,0,0,1,1,1,0,0],
    [0,0,0,0,0,0,0,0,0,0,1,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

## Example 2
```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
```

## Constraints
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

## Intuition
The island problem can be abstracted into a **graph theory** problem. This is an **undirected graph**:

![](../../images/graph_undirected_1.svg)

And this graph may have multiple **connected components** (islands):

![](../../images/graph_undirected_2.png)

Return _the maximum area of an island_ is to return the vertex count of the largest **connected component**.

## Approach
1. Find the first land.
2. Starting at the first land, find all the lands of the island.
    * There are two major ways to explore a `connected component` (island): **Breadth-First Search** and **Depth-First Search**.
    * For **Depth-First Search**, there are two ways to make it: `Recursive` and `Iterative`. So I will provide 3 solutions in total.
    * When we traverse each `connected components` (island), we can:
        * Mark each found land as `8` which represents `visited` (or `included` in `area`). Visited lands don't need to be visited again.
        * **count** the lands of the island.
3. After all lands on an island have been visited, look for the next non-visited land.
4. Repeat the above two steps until all the lands have been `visited`.
5. At last, return the `max_area`.

## Solution 1: 'Depth-First Search' by Recursion
![](../../images/binary_tree_DFS_1.png)

From this sample code bellow, you can see that starting from a vertex, through recursive calls, it goes up until it can't go any further, turns right, and continues up. The priority order of directions is `up, right, down, left`.
```java
depth_first_search(i - 1, j); // up
depth_first_search(i, j + 1); // right
depth_first_search(i + 1, j); // down
depth_first_search(i, j - 1); // left
```

## Solution 2: 'Depth-First Search' by Iteration
Similar problem is `200. Number of Islands`, please click [Depth-First Search by Iteration Solution](200-number-of-islands-2.md) to view.

## Solution 3: Breadth-First Search
Similar problem is `200. Number of Islands`, please click [Breadth-First Search Solution](200-number-of-islands-3.md) to view.

## Complexity
* Time: `O(n * m)`.
* Space: `O(n * m)`.

# Python
```python
class Solution:
    def __init__(self):
        self.max_area = 0
        self.area = 0
        self.grid = None

    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        self.grid = grid

        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    self.area = 0
                    self.depth_first_search(i, j)
                    self.max_area = max(self.max_area, self.area)

        return self.max_area
    
    def depth_first_search(self, i, j):
        if i < 0 or j < 0 or i >= len(self.grid) or j >= len(self.grid[0]):
            return

        if self.grid[i][j] != 1:
            return

        self.grid[i][j] = 8
        self.area += 1

        for m, n in [
            (-1, 0),
            (0, -1), (0, 1),
            (1, 0),
        ]:
            self.depth_first_search(i + m, j + n)
```

# Java
```java
class Solution {
    int[][] grid;
    int maxArea = 0;
    int area = 0;

    public int maxAreaOfIsland(int[][] grid) {
        this.grid = grid;

        for (var i = 0; i < grid.length; i++) {
            for (var j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    area = 0;
                    depthFirstSearch(i, j);
                    maxArea = Math.max(maxArea, area);
                }
            }
        }

        return maxArea;
    }

    void depthFirstSearch(int i, int j) {
        if (i < 0 || i >= grid.length) { 
            return;
        }

        if (j < 0 || j >= grid[0].length) {
            return;
        }

        if (grid[i][j] != 1) {
            return;
        }

        grid[i][j] = 8;
        area++;

        depthFirstSearch(i - 1, j);
        depthFirstSearch(i, j + 1);
        depthFirstSearch(i + 1, j);
        depthFirstSearch(i, j - 1);
    }
}
```

# C++
```cpp
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        grid_ = grid;

        for (auto i = 0; i < grid_.size(); i++) {
            for (auto j = 0; j < grid_[0].size(); j++) {
                if (grid_[i][j] == 1) {
                    area_ = 0;
                    depth_first_search(i, j);
                    max_area_ = max(max_area_, area_);
                }
            }
        }

        return max_area_;
    }

private:
    vector<vector<int>> grid_;
    int max_area_ = 0;
    int area_ = 0;

    void depth_first_search(int i, int j) {
        if (
            i < 0 || i >= grid_.size() || 
            j < 0 || j >= grid_[0].size() || 
            grid_[i][j] != 1
        ) {
            return;
        }

        grid_[i][j] = 8;
        area_++;

        depth_first_search(i - 1, j);
        depth_first_search(i, j + 1);
        depth_first_search(i + 1, j);
        depth_first_search(i, j - 1);
    }
};
```

# JavaScript
```JavaScript
let grid
let maxArea = 0
let area

var maxAreaOfIsland = function (grid_) {
  grid = grid_
  maxArea = 0

  grid.forEach((row, i) => {
    row.forEach((value, j) => {
      if (value === 1) {
        area = 0
        depthFirstSearch(i, j)
        maxArea = Math.max(maxArea, area)
      }
    })
  })

  return maxArea
};


function depthFirstSearch(i, j) {
  if (i < 0 || i >= grid.length) {
    return
  }

  if (j < 0 || j >= grid[0].length) {
    return
  }

  if (grid[i][j] != 1) {
    return
  }

  grid[i][j] = 8
  area++

  depthFirstSearch(i - 1, j)
  depthFirstSearch(i, j + 1)
  depthFirstSearch(i + 1, j)
  depthFirstSearch(i, j - 1)
}
```

# C#
```c#
public class Solution
{
    int[][] grid;
    int maxArea = 0;
    int area = 0;

    public int MaxAreaOfIsland(int[][] grid)
    {
        this.grid = grid;

        for (var i = 0; i < grid.Length; i++)
        {
            for (var j = 0; j < grid[0].Length; j++)
            {
                if (grid[i][j] == 1)
                {
                    area = 0;
                    depthFirstSearch(i, j);
                    maxArea = Math.Max(maxArea, area);
                }
            }
        }

        return maxArea;
    }

    void depthFirstSearch(int i, int j)
    {
        if (i < 0 || i >= grid.Length)
            return;

        if (j < 0 || j >= grid[0].Length)
            return;

        if (grid[i][j] != 1)
            return;

        grid[i][j] = 8;
        area++;

        depthFirstSearch(i - 1, j);
        depthFirstSearch(i, j + 1);
        depthFirstSearch(i + 1, j);
        depthFirstSearch(i, j - 1);
    }
}
```

# Go
```go
var (
    grid [][]int
    maxArea int
    area int
)

func maxAreaOfIsland(grid_ [][]int) int {
    grid = grid_
    maxArea = 0

    for i, row := range grid {
        for j, value := range row {
            if value == 1 {
                area = 0
                depthFirstSearch(i, j)
                maxArea = max(maxArea, area)
            }
        }
    }

    return maxArea
}

func depthFirstSearch(i int, j int) {
    if i < 0 || i >= len(grid) {
        return
    }

    if j < 0 || j >= len(grid[0]) {
        return
    }

    if grid[i][j] != 1 {
        return
    }

    grid[i][j] = 8
    area++

    depthFirstSearch(i - 1, j)
    depthFirstSearch(i, j + 1)
    depthFirstSearch(i + 1, j)
    depthFirstSearch(i, j - 1)
}
```

# Ruby
```Ruby
def max_area_of_island(grid)
  @grid = grid
  @max_area = 0
  @area = 0

  @grid.each_with_index do |row, i|
    row.each_with_index do |value, j|
      if value == 1
        @area = 0
        depth_first_search(i, j)
        @max_area = [@max_area, @area].max
      end
    end
  end

  @max_area
end

def depth_first_search(i, j)
  return if i < 0 || i >= @grid.size

  return if j < 0 || j >= @grid[0].size

  return if @grid[i][j] != 1

  @grid[i][j] = 8
  @area += 1

  depth_first_search(i - 1, j)
  depth_first_search(i, j + 1)
  depth_first_search(i + 1, j)
  depth_first_search(i, j - 1)
end
```

# C
```c
// Welcome to create a PR to complete the code of this language, thanks!
```

# Kotlin
```kotlin
// Welcome to create a PR to complete the code of this language, thanks!
```

# Swift
```swift
// Welcome to create a PR to complete the code of this language, thanks!
```

# Rust
```rust
// Welcome to create a PR to complete the code of this language, thanks!
```

# Other languages
```
// Welcome to create a PR to complete the code of this language, thanks!
```
