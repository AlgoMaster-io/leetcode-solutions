# [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)

---

## Approach 1: Breadth-First Search (BFS)

**Intuition**:  
We can visualize the grid as a graph where each cell has edges to its four adjacent cells. The process of an orange rotting is similar to a multi-source shortest path traversal in an unweighted graph. We need to propagate the rotting effect from initially rotten oranges to the fresh ones in a layer-by-layer fashion. BFS suits perfectly for this scenario where we need to determine the minimum time required for all fresh oranges to become rotten.

### Detailed Solution Steps:
1. Initialize a queue to store the position of all initially rotten oranges. We can treat these as our starting points for BFS.
2. Count the total number of fresh oranges, as we need to track when they all have become rotten.
3. Track the number of minutes passed. We will increment this after each level/layer of BFS completes.
4. Perform the BFS:
   - For each orange at the front of the queue, attempt to rot its adjacent oranges (up, down, left, right).
   - If a fresh orange turns rotten, decrease the fresh orange count, and add this new rotten orange position to the queue.
   - Increment the time after processing all the oranges in the current level of the queue.
5. The process continues until the queue is empty.
6. If at the end there are still fresh oranges remaining, return -1 because not all oranges can rot. Otherwise, return the time elapsed.

### Go Code with Comments:

```go
func orangesRotting(grid [][]int) int {
    if len(grid) == 0 {
        return -1
    }

    // Directions array to traverse in four possible directions: right, down, left, up
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    rows := len(grid)
    columns := len(grid[0])
    
    // Queue for BFS
    queue := make([][2]int, 0)
    // Fresh orange count
    freshCount := 0

    // Initialize the queue with all initially rotten oranges
    // Count the fresh oranges
    for r := 0; r < rows; r++ {
        for c := 0; c < columns; c++ {
            if grid[r][c] == 2 {
                queue = append(queue, [2]int{r, c})
            } else if grid[r][c] == 1 {
                freshCount++
            }
        }
    }

    // No fresh oranges to begin with, all are already rotten
    if freshCount == 0 {
        return 0
    }

    // Time to rot all oranges
    timeElapsed := 0

    // Perform BFS
    for len(queue) > 0 {
        newQueue := make([][2]int, 0)
        // Process all nodes (oranges) in the current level
        for len(queue) > 0 {
            orangePos := queue[0]
            queue = queue[1:]
            r, c := orangePos[0], orangePos[1]
            
            // Traverse in 4 directions
            for _, dir := range directions {
                nr, nc := r+dir[0], c+dir[1]
                // If the new position is within bounds and is a fresh orange
                if nr >= 0 && nr < rows && nc >= 0 && nc < columns && grid[nr][nc] == 1 {
                    // Make it rotten
                    grid[nr][nc] = 2
                    // Decrease fresh orange count
                    freshCount--
                    // Add the new rotten orange to the queue
                    newQueue = append(newQueue, [2]int{nr, nc})
                }
            }
        }
        // Move to the next minute if there were any fresh oranges rotten in this cycle
        if len(newQueue) > 0 {
            timeElapsed++
        }
        queue = newQueue
    }

    // If there's any fresh orange left, return -1, otherwise return timeElapsed
    if freshCount > 0 {
        return -1
    }
    return timeElapsed
}
```

### Time and Space Complexity:
- **Time Complexity**: O(N), where N is the number of cells in the grid. In the worst case, we need to visit all cells.
- **Space Complexity**: O(N) for the queue storage.

This approach efficiently uses a BFS strategy to simulate the rotting process and determine the minimum time required to rot all oranges. If any fresh oranges remain unrot in the end, it implies some fresh oranges are isolated and can't be rotted, thus returning -1. Otherwise, it returns the computed time `timeElapsed`.

