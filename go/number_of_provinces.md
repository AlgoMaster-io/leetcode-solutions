# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func findCircleNum(isConnected [][]int) int {
	n := len(isConnected)
	visited := make([]bool, n)
	provinces := 0

	for i := 0; i < n; i++ {
		if !visited[i] {
			dfs(isConnected, visited, i)
			provinces++ // Each DFS completes a province
		}
	}

	return provinces
}

func dfs(isConnected [][]int, visited []bool, i int) {
	visited[i] = true // Mark the city as visited
	for j := 0; j < len(isConnected); j++ {
		if isConnected[i][j] == 1 && !visited[j] {
			dfs(isConnected, visited, j) // Visit connected cities
		}
	}
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func findCircleNum(isConnected [][]int) int {
	n := len(isConnected)
	visited := make([]bool, n)
	provinces := 0

	for i := 0; i < n; i++ {
		if !visited[i] {
			bfs(isConnected, visited, i)
			provinces++
		}
	}

	return provinces
}

func bfs(isConnected [][]int, visited []bool, i int) {
	queue := []int{i}
	visited[i] = true // Mark as visited

	for len(queue) > 0 {
		city := queue[0]
		queue = queue[1:]
		for j := 0; j < len(isConnected); j++ {
			if isConnected[city][j] == 1 && !visited[j] {
				queue = append(queue, j)
				visited[j] = true
			}
		}
	}
}
```

## Approach 3: Union-Find

### Solution
```go
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
package main

type UnionFind struct {
	parent []int
	count  int
}

func newUnionFind(n int) *UnionFind {
	uf := &UnionFind{
		parent: make([]int, n),
		count:  n,
	}
	for i := range uf.parent {
		uf.parent[i] = i
	}
	return uf
}

func (uf *UnionFind) find(x int) int {
	if uf.parent[x] != x {
		uf.parent[x] = uf.find(uf.parent[x]) // Path compression
	}
	return uf.parent[x]
}

func (uf *UnionFind) union(x, y int) {
	rootX := uf.find(x)
	rootY := uf.find(y)
	if rootX != rootY {
		uf.parent[rootX] = rootY // Union by root
		uf.count--
	}
}

func (uf *UnionFind) getCount() int {
	return uf.count
}

func findCircleNum(isConnected [][]int) int {
	n := len(isConnected)
	uf := newUnionFind(n)

	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if isConnected[i][j] == 1 {
				uf.union(i, j) // Union the connected cities
			}
		}
	}

	return uf.getCount() // Return the number of provinces
}
```


