# [Leetcode Problem 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Priority Queue (Min Heap)](#approach-2-priority-queue-min-heap)
- [Approach 3: Binary Search with BFS](#approach-3-binary-search-with-bfs)

### Approach 1: Brute Force

**Intuition:**

Consider every possible time from 0 up to the maximum elevation in the grid. At each time, attempt to reach the bottom-right corner of the grid starting from the top-left corner using a simple depth-first search (DFS). The minimum time at which a path is found will be our answer.

**Time Complexity:** O(N^2 * MaxElevation)  
**Space Complexity:** O(N^2) for the stack during the DFS.

```go
package main

func swimInWater(grid [][]int) int {
    n := len(grid)
    
    // Check if it is possible to reach the bottom-right cell at a given time
    var canSwim func(int, int, int, [][]bool) bool
    canSwim = func(t, x, y int, visited [][]bool) bool {
        if x < 0 || y < 0 || x >= n || y >= n || visited[x][y] || grid[x][y] > t {
            return false
        }
        // If we reach the end, return true
        if x == n-1 && y == n-1 {
            return true
        }
        visited[x][y] = true
        // Try all 4 directions
        return canSwim(t, x+1, y, visited) || canSwim(t, x-1, y, visited) ||
               canSwim(t, x, y+1, visited) || canSwim(t, x, y-1, visited)
    }
    
    // Check every possible time
    for t := 0; t <= n*n-1; t++ {
        visited := make([][]bool, n)
        for i := range visited {
            visited[i] = make([]bool, n)
        }
        if canSwim(t, 0, 0, visited) {
            return t
        }
    }
    
    return -1
}
```
This approach is inefficient for larger inputs due to its high time complexity.

### Approach 2: Priority Queue (Min Heap)

**Intuition:**

We treat the grid as a graph where each cell is a node. Use a priority queue to always expand the node with the minimum elevation that can currently be entered (akin to Dijkstra's algorithm). Continue until we reach the target node (bottom-right corner of the grid).

**Time Complexity:** O(N^2 logN)  
**Space Complexity:** O(N^2)

```go
package main

import (
    "container/heap"
)

type Cell struct {
    height int
    x, y   int
}

type MinHeap []Cell

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].height < h[j].height }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(Cell))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func swimInWater(grid [][]int) int {
    n := len(grid)
    directions := []struct{ dx, dy int }{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    visited := make([][]bool, n)
    for i := range visited {
        visited[i] = make([]bool, n)
    }
    pq := &MinHeap{Cell{grid[0][0], 0, 0}}
    heap.Init(pq)
    visited[0][0] = true
    res := 0

    for pq.Len() > 0 {
        // Get the cell with the minimum elevation we can reach
        cell := heap.Pop(pq).(Cell)
        res = max(res, grid[cell.x][cell.y])
        // If we reach the end, return the max value seen so far
        if cell.x == n-1 && cell.y == n-1 {
            return res
        }

        // Explore neighbors
        for _, dir := range directions {
            newX, newY := cell.x+dir.dx, cell.y+dir.dy
            if newX >= 0 && newY >= 0 && newX < n && newY < n && !visited[newX][newY] {
                visited[newX][newY] = true
                heap.Push(pq, Cell{grid[newX][newY], newX, newY})
            }
        }
    }
    return res
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Approach 3: Binary Search with DFS/BFS

**Intuition:**

Perform a binary search on the time value. Use the binary search to find the minimum possible time where it's feasible to swim from the top-left to the bottom-right. For each mid-point in the binary search, use DFS or BFS to check feasibility.

**Time Complexity:** O(N^2 log MaxElevation)  
**Space Complexity:** O(N^2)

```go
package main

func swimInWater(grid [][]int) int {
    n := len(grid)
    left, right := grid[0][0], n*n-1

    var dfs func(int, int, int, [][]bool) bool
    dfs = func(t, x, y int, visited [][]bool) bool {
        if x < 0 || y < 0 || x >= n || y >= n || visited[x][y] || grid[x][y] > t {
            return false
        }
        if x == n-1 && y == n-1 {
            return true
        }
        visited[x][y] = true
        return dfs(t, x+1, y, visited) || dfs(t, x-1, y, visited) || 
               dfs(t, x, y+1, visited) || dfs(t, x, y-1, visited)
    }

    for left < right {
        mid := left + (right-left)/2
        visited := make([][]bool, n)
        for i := range visited {
            visited[i] = make([]bool, n)
        }
        if dfs(mid, 0, 0, visited) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    return left
}
```

This approach efficiently narrows down the minimum feasible time using binary search, with feasibility checked by searching the grid (DFS or BFS).

