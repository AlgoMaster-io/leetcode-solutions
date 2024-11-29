# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
go
```go
// Import necessary libraries
import (
    "container/heap"
    "math"
)

// Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
// Space Complexity: O(m * n)

type Item struct {
    effort, x, y int
}

type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].effort < pq[j].effort
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
    item := x.(*Item)
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

func minimumEffortPath(heights [][]int) int {
    m, n := len(heights), len(heights[0])
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    efforts := make([][]int, m)

    for i := range efforts {
        efforts[i] = make([]int, n)
        for j := range efforts[i] {
            efforts[i][j] = math.MaxInt32
        }
    }
    efforts[0][0] = 0

    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, &Item{effort: 0, x: 0, y: 0})

    for pq.Len() > 0 {
        current := heap.Pop(pq).(*Item)
        currentEffort, x, y := current.effort, current.x, current.y

        if x == m-1 && y == n-1 {
            return currentEffort
        }

        for _, direction := range directions {
            newX, newY := x+direction[0], y+direction[1]
            if newX >= 0 && newX < m && newY >= 0 && newY < n {
                effort := max(currentEffort, abs(heights[newX][newY]-heights[x][y]))
                if effort < efforts[newX][newY] {
                    efforts[newX][newY] = effort
                    heap.Push(pq, &Item{effort: effort, x: newX, y: newY})
                }
            }
        }
    }

    return 0
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
go
```go
// Import necessary libraries
import "math"

// Time Complexity: O(m * n * log(maxDifference))
// Space Complexity: O(m * n)

func minimumEffortPath(heights [][]int) int {
    m, n := len(heights), len(heights[0])
    left, right, result := 0, 1_000_000, 1_000_000
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    for left <= right {
        mid := left + (right-left)/2
        if canReachEndWithEffort(heights, mid, m, n, directions) {
            result = mid
            right = mid - 1
        } else {
            left = mid + 1
        }
    }

    return result
}

func canReachEndWithEffort(heights [][]int, maxEffort, m, n int, directions [][]int) bool {
    visited := make([][]bool, m)
    for i := range visited {
        visited[i] = make([]bool, n)
    }
    queue := [][2]int{{0, 0}}
    visited[0][0] = true

    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        x, y := current[0], current[1]

        if x == m-1 && y == n-1 {
            return true
        }

        for _, direction := range directions {
            newX, newY := x+direction[0], y+direction[1]
            if newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY] {
                currentEffort := abs(heights[newX][newY] - heights[x][y])
                if currentEffort <= maxEffort {
                    visited[newX][newY] = true
                    queue = append(queue, [2]int{newX, newY})
                }
            }
        }
    }

    return false
}
```

