# [LeetCode 1293: Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)
- [Approach 2: A* Search (Heuristic BFS)](#approach-2-a-search-heuristic-bfs)

### Approach 1: Breadth-First Search (BFS)

#### Intuition
The problem can be approached using BFS since we seek the shortest path in an unweighted grid. Here, "unweighted" means that each move has the same cost. We will enhance the BFS by storing an additional dimension in our state: the number of obstacles we can still eliminate while reaching a particular cell.

#### Algorithm
1. Use a queue to perform BFS, starting from the top-left corner of the grid.
2. Each state in our queue will include the current cell position `(i, j)`, the number of steps taken to reach this cell, and the number of obstacles that can still be eliminated.
3. A visited map is used to track the least number of eliminations used to reach `(i, j)` with a certain number of remaining eliminations to avoid cycles.
4. Explore all possible directions (up, down, left, right) from the current cell.
5. Check if the destination `(m-1, n-1)` is reached, and return the number of steps taken if so.
6. Repeat until the queue is empty or the destination is reached.

#### Time Complexity
- **O(m * n * k)**, where `m` is the number of rows, `n` is the number of columns, and `k` is the number of eliminations allowed, as each cell can be visited with different values of remaining eliminations.

#### Space Complexity
- **O(m * n * k)** to store the BFS queue and visited states.

```go
func shortestPath(grid [][]int, k int) int {
    m, n := len(grid), len(grid[0])
    if m == 1 && n == 1 {
        return 0
    }
    
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    
    // BFS queue of (x, y, steps, remaining eliminations)
    queue := [][]int{{0, 0, 0, k}}
    visited := make([][][]bool, m)
    for i := range visited {
        visited[i] = make([][]bool, n)
        for j := range visited[i] {
            visited[i][j] = make([]bool, k+1)
        }
    }
    
    // Mark the start position as visited
    visited[0][0][k] = true
    
    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        
        x, y, steps, remainingEliminations := current[0], current[1], current[2], current[3]
        
        // Traverse all possible directions
        for _, direction := range directions {
            newX, newY := x + direction[0], y + direction[1]
            
            // Check boundaries
            if newX >= 0 && newX < m && newY >= 0 && newY < n {
                newEliminations := remainingEliminations
                
                if grid[newX][newY] == 1 {
                    // Need to eliminate an obstacle
                    newEliminations--
                }
                
                if newEliminations >= 0 && !visited[newX][newY][newEliminations] {
                    // When we reach the destination
                    if newX == m-1 && newY == n-1 {
                        return steps + 1
                    }
                    visited[newX][newY][newEliminations] = true
                    queue = append(queue, []int{newX, newY, steps + 1, newEliminations})
                }
            }
        }
    }
    
    return -1
}
```

### Approach 2: A* Search (Heuristic BFS)

#### Intuition
A* search is a more optimized BFS-based method for pathfinding problems, especially when you know the goal node. In A*, we use heuristics to prioritize paths that are likely to lead to the goal faster. The heuristic used is the Manhattan distance from a cell to the destination. 

#### Algorithm
1. Similar to BFS, but use a priority queue instead, which sorts nodes by the sum of steps taken and heuristic.
2. The heuristic is calculated as `|m-1 - x| + |n-1 - y|`, representing the Manhattan distance from the current cell `(x, y)` to the target cell `(m-1, n-1)`.
3. Rest of the steps remain the same as in BFS, but we pop states from the priority queue.

#### Time Complexity
- Potentially better in realistic scenarios compared to BFS but in the worst case, **O(m * n * k)**.

#### Space Complexity
- **O(m * n * k)** similar to BFS.

```go
import (
    "container/heap"
)

type State struct {
    x, y, steps, remainingEliminations int
}

// Computing Manhattan distance heuristic
func heuristic(x, y, m, n int) int {
    return m - 1 - x + n - 1 - y
}

type PriorityQueue []State

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    // Lesser total cost => higher priority
    xi, yi := pq[i].x, pq[i].y
    xj, yj := pq[j].x, pq[j].y
    return pq[i].steps + heuristic(xi, yi, len(grid), len(grid[0])) <
        pq[j].steps + heuristic(xj, yj, len(grid), len(grid[0]))
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
    *pq = append(*pq, x.(State))
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    x := old[n-1]
    *pq = old[0 : n-1]
    return x
}

func shortestPath(grid [][]int, k int) int {
    m, n := len(grid), len(grid[0])
    if m == 1 && n == 1 {
        return 0
    }
    
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    
    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, State{0, 0, 0, k})
    
    visited := make([][][]int, m)
    for i := range visited {
        visited[i] = make([][]int, n)
        for j := range visited[i] {
            visited[i][j] = make([]int, k+1)
            for l := range visited[i][j] {
                visited[i][j][l] = -1
            }
        }
    }
    
    visited[0][0][k] = 0
    
    for pq.Len() > 0 {
        current := heap.Pop(pq).(State)
        
        if current.x == m-1 && current.y == n-1 {
            return current.steps
        }
        
        for _, direction := range directions {
            newX, newY := current.x+direction[0], current.y+direction[1]
            
            if newX >= 0 && newX < m && newY >= 0 && newY < n {
                newEliminations := current.remainingEliminations
                if grid[newX][newY] == 1 {
                    newEliminations--
                }
                
                if newEliminations >= 0 && (visited[newX][newY][newEliminations] == -1 || visited[newX][newY][newEliminations] > current.steps+1) {
                    visited[newX][newY][newEliminations] = current.steps + 1
                    heap.Push(pq, State{newX, newY, current.steps + 1, newEliminations})
                }
            }
        }
    }
    
    return -1
}
```

Whether using pure BFS or the optimized A* strategy, picking the right tool depends on the typical input size and distribution in competitive programming.

