# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
package main

import "math"

var directions = [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

func longestIncreasingPath(matrix [][]int) int {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return 0
	}
	
	m, n := len(matrix), len(matrix[0])
	memo := make([][]int, m)
	for i := range memo {
		memo[i] = make([]int, n)
	}

	longestPath := 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			longestPath = max(longestPath, dfs(matrix, i, j, memo))
		}
	}
	return longestPath
}

func dfs(matrix [][]int, i, j int, memo [][]int) int {
	if memo[i][j] != 0 {
		return memo[i][j]
	}

	maxPath := 1
	for _, dir := range directions {
		x, y := i+dir[0], j+dir[1]
		if x >= 0 && x < len(matrix) && y >= 0 && y < len(matrix[0]) && matrix[x][y] > matrix[i][j] {
			length := 1 + dfs(matrix, x, y, memo)
			maxPath = max(maxPath, length)
		}
	}

	memo[i][j] = maxPath
	return maxPath
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## Approach 2: Topological Sort with Indegree

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
package main

import "container/list"

var directionsTopo = [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

func longestIncreasingPathTopo(matrix [][]int) int {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return 0
	}

	m, n := len(matrix), len(matrix[0])
	indegree := make([][]int, m)
	for i := range indegree {
		indegree[i] = make([]int, n)
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for _, dir := range directionsTopo {
				x, y := i+dir[0], j+dir[1]
				if x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j] {
					indegree[x][y]++
				}
			}
		}
	}

	queue := list.New()
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if indegree[i][j] == 0 {
				queue.PushBack([]int{i, j})
			}
		}
	}

	pathLength := 0
	for queue.Len() > 0 {
		size := queue.Len()
		pathLength++
		for k := 0; k < size; k++ {
			element := queue.Front()
			cell := element.Value.([]int)
			queue.Remove(element)

			for _, dir := range directionsTopo {
				x, y := cell[0]+dir[0], cell[1]+dir[1]
				if x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[cell[0]][cell[1]] {
					indegree[x][y]--
					if indegree[x][y] == 0 {
						queue.PushBack([]int{x, y})
					}
				}
			}
		}
	}

	return pathLength
}
```

