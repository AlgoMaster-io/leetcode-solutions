# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution
go
```go
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and distance matrix
package main

type Point struct {
    x int
    y int
}

func updateMatrix(mat [][]int) [][]int {
    rows := len(mat)
    cols := len(mat[0])
    distances := make([][]int, rows)
    visited := make([][]bool, rows)
    queue := []Point{}

    for i := 0; i < rows; i++ {
        distances[i] = make([]int, cols)
        visited[i] = make([]bool, cols)
    }

    // Step 1: Initialize the queue with all '0' cells and mark them as visited
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if mat[i][j] == 0 {
                queue = append(queue, Point{i, j})
                visited[i][j] = true
            }
        }
    }

    // Step 2: Perform BFS to calculate distances
    directions := []Point{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        for _, dir := range directions {
            newRow := current.x + dir.x
            newCol := current.y + dir.y
            
            // Check bounds and if the cell is not visited
            if newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && !visited[newRow][newCol] {
                distances[newRow][newCol] = distances[current.x][current.y] + 1
                queue = append(queue, Point{newRow, newCol})
                visited[newRow][newCol] = true
            }
        }
    }

    return distances
}
```

