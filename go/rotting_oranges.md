# 994. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Solution
go
```go
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and visited cells
package main

type Position struct {
    row int
    col int
}

func orangesRotting(grid [][]int) int {
    rows := len(grid)
    cols := len(grid[0])
    queue := []Position{}
    freshCount := 0

    // Step 1: Initialize the queue with all rotten oranges and count fresh oranges
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if grid[i][j] == 2 {
                queue = append(queue, Position{row: i, col: j})
            } else if grid[i][j] == 1 {
                freshCount++
            }
        }
    }

    // Step 2: Perform BFS to rot adjacent fresh oranges
    minutes := 0
    directions := []Position{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    for len(queue) > 0 && freshCount > 0 {
        size := len(queue)
        for i := 0; i < size; i++ {
            current := queue[0]
            queue = queue[1:]
            for _, dir := range directions {
                newRow := current.row + dir.row
                newCol := current.col + dir.col

                // Check bounds and if the orange is fresh
                if newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1 {
                    grid[newRow][newCol] = 2 // Rot the orange
                    queue = append(queue, Position{row: newRow, col: newCol})
                    freshCount--
                }
            }
        }
        minutes++
    }

    // If there are still fresh oranges left, return -1
    if freshCount == 0 {
        return minutes
    }
    return -1
}
```

