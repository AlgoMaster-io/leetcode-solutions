# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
go
```go
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)
import (
    "container/list"
)

func shortestPathLength(graph [][]int) int {
    n := len(graph)
    if n == 1 {
        return 0
    }

    // Queue for BFS, storing {node, visited nodes mask}
    queue := list.New()
    visited := make([][]bool, n)
    for i := range visited {
        visited[i] = make([]bool, 1<<n)
    }

    // Initialize queue and visited set with all starting nodes
    for i := 0; i < n; i++ {
        queue.PushBack([]int{i, 1 << i})
        visited[i][1<<i] = true
    }

    steps := 0
    for queue.Len() > 0 {
        size := queue.Len()
        for i := 0; i < size; i++ {
            current := queue.Remove(queue.Front()).([]int)
            node, mask := current[0], current[1]

            // If all nodes are visited
            if mask == (1<<n) - 1 {
                return steps
            }

            // Visit all the neighbors
            for _, nei := range graph[node] {
                nextMask := mask | (1 << nei)
                if !visited[nei][nextMask] {
                    queue.PushBack([]int{nei, nextMask})
                    visited[nei][nextMask] = true
                }
            }
        }
        steps++
    }
    return -1
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
go
```go
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)
import (
    "container/list"
    "math"
)

func shortestPathLength(graph [][]int) int {
    n := len(graph)
    dp := make([][]int, 1<<n)
    for i := range dp {
        dp[i] = make([]int, n)
        for j := range dp[i] {
            dp[i][j] = math.MaxInt32
        }
    }

    queue := list.New()
    for i := 0; i < n; i++ {
        queue.PushBack([]int{1 << i, i})
        dp[1<<i][i] = 0
    }

    for queue.Len() > 0 {
        current := queue.Remove(queue.Front()).([]int)
        mask, node := current[0], current[1]

        for _, nei := range graph[node] {
            nextMask := mask | (1 << nei)
            if dp[nextMask][nei] > dp[mask][node] + 1 {
                dp[nextMask][nei] = dp[mask][node] + 1
                queue.PushBack([]int{nextMask, nei})
            }
        }
    }

    result := math.MaxInt32
    for i := 0; i < n; i++ {
        if dp[(1<<n)-1][i] < result {
            result = dp[(1<<n)-1][i]
        }
    }
    return result
}
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

