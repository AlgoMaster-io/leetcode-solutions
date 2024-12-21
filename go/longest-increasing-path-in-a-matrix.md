# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approaches
- [DFS with Recursion](#dfs-with-recursion)
- [Memoized DFS](#memoized-dfs)
- [Topological Sort using Indegree](#topological-sort-using-indegree)

---

### DFS with Recursion

**Intuition:**

The problem is about finding the longest increasing path in a 2D matrix. We can apply depth-first search (DFS) traversal from each cell and explore in all four possible directions (up, down, left, right) to find increasing paths. For each DFS call, we explore all neighboring cells that have a value greater than the current cell's value.

**Approach:**

1. Iterate over every cell in the matrix.
2. For each cell, if it hasn't been visited, initiate a DFS to find the longest path starting from that cell.
3. The DFS will recursively call itself for each of the neighbors (if valid and increasing).
4. Keep track of the longest path found during the DFS from each cell.
5. Return the length of the longest path found.

**Time Complexity:** O((m * n) * 4^(m*n)) - The naive DFS approach explores all paths leading to exponential time in nature.

**Space Complexity:** O(m * n) - for recursion stack.

```go
func longestIncreasingPath(matrix [][]int) int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return 0
    }

    // Directions for left, right, up, down
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    var dfs func(int, int, int) int
    var max int

    // Define DFS function
    dfs = func(x int, y int, prev int) int {
        if x < 0 || x >= len(matrix) || y < 0 || y >= len(matrix[0]) || matrix[x][y] <= prev {
            return 0
        }
        currentVal := matrix[x][y]
        var maxPath = 0

        // Explore all 4 directions
        for _, dir := range directions {
            newX, newY := x+dir[0], y+dir[1]
            maxPath = maxInt(maxPath, dfs(newX, newY, currentVal))
        }
        return maxPath + 1
    }

    for i := 0; i < len(matrix); i++ {
        for j := 0; j < len(matrix[0]); j++ {
            max = maxInt(max, dfs(i, j, -1))
        }
    }

    return max
}

func maxInt(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

### Memoized DFS

**Intuition:**

The DFS approach involves recomputing the longest increasing path from a cell multiple times. We can improve the performance by using memoization to store the result of the longest path from each cell.

**Approach:**

1. Create a memoization table `memo` to store the results of DFS from each cell.
2. Implement DFS similar to the previous approach but store the results in the `memo` table.
3. Before exploring a cell for the longest path, check if its result is already computed in `memo`.

**Time Complexity:** O(m * n) - Each cell is visited and computed once.

**Space Complexity:** O(m * n) - for memoization and recursion stack.

```go
func longestIncreasingPathMemo(matrix [][]int) int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return 0
    }

    rows, cols := len(matrix), len(matrix[0])
    memo := make([][]int, rows)
    for i := range memo {
        memo[i] = make([]int, cols)
    }
    var dfs func(int, int, int) int
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    var longest int

    dfs = func(x, y, prevVal int) int {
        if x < 0 || x >= rows || y < 0 || y >= cols || matrix[x][y] <= prevVal {
            return 0
        }
        if memo[x][y] != 0 {
            return memo[x][y]
        }
        var maxPath = 0
        currentVal := matrix[x][y]
        for _, dir := range directions {
            newX, newY := x+dir[0], y+dir[1]
            maxPath = maxInt(maxPath, dfs(newX, newY, currentVal))
        }
        memo[x][y] = maxPath + 1
        return memo[x][y]
    }

    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            longest = maxInt(longest, dfs(i, j, -1))
        }
    }
    return longest
}
```

---

### Topological Sort using Indegree

**Intuition:**

Topological sort can be leveraged here by treating cells as nodes and edges representing increasing paths. We utilize a Kahn’s algorithm-like approach where we process all nodes with indegree zero.

**Approach:**

1. Calculate the indegree of each cell in the matrix, where indegree represents how many cells have a greater value.
2. Use a queue to perform Kahn’s algorithm. Initially, add all cells with an indegree of 0 to the queue.
3. Begin from the queue, process each cell by reducing the indegree of its neighbors. If a cell’s indegree becomes zero, add it to the queue.
4. Count the number of layers of processing to find the longest increasing path.

**Time Complexity:** O(m * n) - Each cell and its edges are considered once.

**Space Complexity:** O(m * n) - for indegree and queue structures.

```go
func longestIncreasingPathTopo(matrix [][]int) int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return 0
    }

    rows, cols := len(matrix), len(matrix[0])
    indegree := make([][]int, rows)
    for i := range indegree {
        indegree[i] = make([]int, cols)
    }

    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    // Step 1: Calculate indegree for each cell
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            for _, dir := range directions {
                ni, nj := i+dir[0], j+dir[1]
                if ni >= 0 && ni < rows && nj >= 0 && nj < cols && matrix[ni][nj] > matrix[i][j] {
                    indegree[ni][nj]++
                }
            }
        }
    }

    // Step 2: Use queue for Kahn's Algorithm
    queue := [][]int{}
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if indegree[i][j] == 0 {
                queue = append(queue, []int{i, j})
            }
        }
    }

    var longest int
    for len(queue) > 0 {
        longest++
        // length of already in queue
        size := len(queue)
        for i := 0; i < size; i++ {
            cell := queue[0]
            queue = queue[1:]
            x, y := cell[0], cell[1]
            for _, dir := range directions {
                ni, nj := x+dir[0], y+dir[1]
                if ni >= 0 && ni < rows && nj >= 0 && nj < cols && matrix[ni][nj] > matrix[x][y] {
                    indegree[ni][nj]--
                    if indegree[ni][nj] == 0 {
                        queue = append(queue, []int{ni, nj})
                    }
                }
            }
        }
    }
    return longest
}
```

These different approaches provide solutions ranging from simple but inefficient to more optimized algorithms for solving the problem of finding the longest increasing path in a matrix.

