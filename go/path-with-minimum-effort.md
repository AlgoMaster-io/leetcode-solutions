# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approaches
- [Approach 1: Brute Force - Modified DFS/BFS](#approach-1-brute-force-modified-dfsbfs)
- [Approach 2: Binary Search + BFS/Union-Find](#approach-2-binary-search-bfsunion-find)
- [Approach 3: Dijkstra's Algorithm](#approach-3-dijkstras-algorithm)

## Approach 1: Brute Force - Modified DFS/BFS

### Intuition:
In this brute force approach, imagine exploring each possible path from the top-left corner to the bottom-right corner. For each path, calculate the maximum effort required, which is the largest difference in height between two consecutively connected cells on a given path. Try all possible paths and find the minimum value of the maximum efforts.

This approach, while comprehensive, is not efficient due to its exponential time complexity. 

### Algorithm:
1. Use Depth First Search (DFS) to explore from the top-left to the bottom-right corner.
2. Track the path's maximum effort as the maximum height difference encountered.
3. After exploring all possibilities, return the lowest of these maximum efforts.

### Time Complexity:
- Time: O((m*n)!), where m and n are the dimensions of the grid. It attempts all possible paths.
- Space: O(m*n) for recursive DFS stack.

### Go Code:
```go
// This approach is a conceptual demonstration and is not implemented due to inefficiency in practice.
```

## Approach 2: Binary Search + BFS/Union-Find

### Intuition:
This approach leverages binary search on the solution space of possible maximum differences, coupled with a BFS or Union-Find to test feasibility of a path for each candidate difference.

### Algorithm:
1. Binary search across possible maximum efforts from 0 to the maximum height difference in the grid.
2. For a middle value in the binary search, use BFS or Union-Find to check if there is a valid path where all edge differences are less than or equal to this value.
3. Adjust binary search range based on whether a path is feasible for that middle value.

### Time Complexity:
- Time: O((m*n) log(max height difference)), as BFS is O(m*n) and binary search is O(log(max difference)).
- Space: O(m*n) for BFS queue/Union-Find data structure.

### Go Code:
```go
package main

import (
	"container/list"
	"fmt"
	"math"
)

func minimumEffortPath(heights [][]int) int {
	// Directions array for 4 possible movements: right, down, left, up.
	directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
	rows, cols := len(heights), len(heights[0])

	// Helper function for BFS using a threshold effort.
	canReachEnd := func(threshold int) bool {
		visited := make([][]bool, rows)
		for i := range visited {
			visited[i] = make([]bool, cols)
		}
		queue := list.New()
		queue.PushBack([2]int{0, 0})
		for queue.Len() > 0 {
			curr := queue.Remove(queue.Front()).([2]int)
			if curr[0] == rows-1 && curr[1] == cols-1 {
				return true
			}
			for _, dir := range directions {
				r, c := curr[0]+dir[0], curr[1]+dir[1]
				if r >= 0 && r < rows && c >= 0 && c < cols && !visited[r][c] {
					if abs(heights[r][c]-heights[curr[0]][curr[1]]) <= threshold {
						visited[r][c] = true
						queue.PushBack([2]int{r, c})
					}
				}
			}
		}
		return false
	}

	// Binary search for minimum effort.
	left, right := 0, int(1e6)
	for left < right {
		mid := left + (right-left)/2
		if canReachEnd(mid) {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func main() {
	heights := [][]int{{1, 2, 2}, {3, 8, 2}, {5, 3, 5}}
	fmt.Println(minimumEffortPath(heights)) // Output: 2
}
```

## Approach 3: Dijkstra's Algorithm

### Intuition:
Treat the grid as a graph where cells are nodes connected by edges weighted with the difference in heights. Use Dijkstra's algorithm to find the path from the top-left to the bottom-right with the minimum maximum edge weight.

### Algorithm:
1. Initialize a priority queue, starting with the top-left corner and effort 0.
2. Use a min-heap (priority queue) to always expand the least effort path first.
3. For each cell, update adjacent cells' effort and add them to the queue if the new calculated effort is smaller.
4. If reaching the bottom-right cell, the current path is the minimum effort path.

### Time Complexity:
- Time: O(m*n log(m*n)) due to priority queue operations.
- Space: O(m*n) for storing efforts and priority queue.

### Go Code:
```go
package main

import (
	"container/heap"
	"fmt"
	"math"
)

type Node struct {
	r, c, effort int
}

type MinHeap []Node

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i].effort < h[j].effort }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(Node)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func minimumEffortPathDijkstra(heights [][]int) int {
	directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
	rows, cols := len(heights), len(heights[0])

	efforts := make([][]int, rows)
	for i := range efforts {
		efforts[i] = make([]int, cols)
		for j := 0; j < cols; j++ {
			efforts[i][j] = math.MaxInt32
		}
	}
	efforts[0][0] = 0

	h := &MinHeap{}
	heap.Init(h)
	heap.Push(h, Node{0, 0, 0})

	for h.Len() > 0 {
		curr := heap.Pop(h).(Node)

		if curr.r == rows-1 && curr.c == cols-1 {
			return curr.effort
		}

		for _, dir := range directions {
			r, c := curr.r+dir[0], curr.c+dir[1]
			if r >= 0 && r < rows && c >= 0 && c < cols {
				newEffort := max(curr.effort, abs(heights[r][c]-heights[curr.r][curr.c]))
				if newEffort < efforts[r][c] {
					efforts[r][c] = newEffort
					heap.Push(h, Node{r, c, newEffort})
				}
			}
		}
	}
	return 0
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

func main() {
	heights := [][]int{{1, 2, 2}, {3, 8, 2}, {5, 3, 5}}
	fmt.Println(minimumEffortPathDijkstra(heights)) // Output: 2
}
```

Each approach improves upon the last in terms of efficiency. Approach 1 provides a thorough exploration but is impractical; Approach 2 balances exploration with efficient search; and Approach 3 optimizes for both time and space, making it the best solution for this problem.

