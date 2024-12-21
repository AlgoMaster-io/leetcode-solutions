# [Leetcode 542: 01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming](#dynamic-programming-approach)
3. [Breadth-First Search (Optimal)](#breadth-first-search-approach)

---

## Brute Force Approach

### Intuition
The simplest way to solve this problem is to check each zero in the matrix for every one and keep track of the minimum distance found. We'll iterate through each cell in the matrix, and for each cell containing a one, we will perform a BFS (Breadth-First Search) to find the nearest zero.

This approach, although correct, is inefficient for larger matrices because it computes the distance from ones to zeros in an exhaustive manner.

### Approach Implementation

```go
func updateMatrix(mat [][]int) [][]int {
    rows, cols := len(mat), len(mat[0])
    // Initialize the result matrix with max values
    dist := make([][]int, rows)
    for i := range dist {
        dist[i] = make([]int, cols)
        for j := range dist[i] {
            dist[i][j] = rows + cols // Maximum possible distance
        }
    }

    // Helper function to calculate absolute value
    abs := func(x int) int {
        if x < 0 {
            return -x
        }
        return x
    }

    // Brute force search for each cell
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if mat[i][j] == 0 {
                dist[i][j] = 0
            } else {
                // Calculate distance to every other cell with 0
                for k := 0; k < rows; k++ {
                    for l := 0; l < cols; l++ {
                        if mat[k][l] == 0 {
                            dist[i][j] = min(dist[i][j], abs(i-k)+abs(j-l))
                        }
                    }
                }
            }
        }
    }
    return dist
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time Complexity**: O((m * n)²), where `m` and `n` are the number of rows and columns. For each cell, we potentially scan all other cells.
- **Space Complexity**: O(m * n) for the distance matrix.

---

## Dynamic Programming Approach

### Intuition
We can improve on the brute-force approach using dynamic programming. The idea is to first process the matrix from top-left to bottom-right and then again from bottom-right to top-left. In each pass, we compute the minimum distance using previously computed results.

### Approach Implementation

```go
func updateMatrix(mat [][]int) [][]int {
    rows, cols := len(mat), len(mat[0])
    maxDistance := rows + cols
    dist := make([][]int, rows)
    
    for i := range dist {
        dist[i] = make([]int, cols)
        for j := range dist[i] {
            dist[i][j] = maxDistance // Initialize with max possible value
        }
    }

    // Process from top-left to bottom-right
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if mat[i][j] == 0 {
                dist[i][j] = 0
            } else {
                if i > 0 {
                    dist[i][j] = min(dist[i][j], dist[i-1][j]+1)
                }
                if j > 0 {
                    dist[i][j] = min(dist[i][j], dist[i][j-1]+1)
                }
            }
        }
    }

    // Process from bottom-right to top-left
    for i := rows - 1; i >= 0; i-- {
        for j := cols - 1; j >= 0; j-- {
            if i < rows - 1 {
                dist[i][j] = min(dist[i][j], dist[i+1][j]+1)
            }
            if j < cols - 1 {
                dist[i][j] = min(dist[i][j], dist[i][j+1]+1)
            }
        }
    }

    return dist
}
```

### Complexity Analysis
- **Time Complexity**: O(m * n), where `m` and `n` are the dimensions of the matrix.
- **Space Complexity**: O(m * n) for the distance matrix.

---

## Breadth-First Search Approach

### Intuition
In this approach, we leverage BFS for its ability to explore nodes level by level. We'll use a queue to start from all zeros and propagate the distances to ones.

### Approach Implementation

```go
func updateMatrix(mat [][]int) [][]int {
    rows, cols := len(mat), len(mat[0])
    queue := make([][2]int, 0)
    directions := [4][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

    // Initialize a result matrix with high values and enqueue all zeros
    dist := make([][]int, rows)
    for i := range dist {
        dist[i] = make([]int, cols)
        for j := range dist[i] {
            if mat[i][j] == 0 {
                dist[i][j] = 0
                queue = append(queue, [2]int{i, j})
            } else {
                dist[i][j] = rows + cols // Maximum possible distance
            }
        }
    }

    // BFS from all zeros simultaneously
    for len(queue) > 0 {
        point := queue[0]
        queue = queue[1:]
        for _, dir := range directions {
            r, c := point[0]+dir[0], point[1]+dir[1]
            if r >= 0 && c >= 0 && r < rows && c < cols {
                if dist[r][c] > dist[point[0]][point[1]]+1 {
                    dist[r][c] = dist[point[0]][point[1]] + 1
                    queue = append(queue, [2]int{r, c})
                }
            }
        }
    }
    return dist
}
```

### Complexity Analysis
- **Time Complexity**: O(m * n), as each cell is processed once.
- **Space Complexity**: O(m * n), for the distance matrix. The queue's size can range from O(m * n) in worst case.

This BFS approach is preferred for its elegance and efficiency in dealing with the shortest-path problems of this nature in the matrix.

