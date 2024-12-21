# [Leetcode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approaches
1. [Depth First Search (DFS) Approach](#depth-first-search-dfs-approach)
2. [Breadth First Search (BFS) Approach](#breadth-first-search-bfs-approach)
3. [Union-Find Approach](#union-find-approach)

### Depth First Search (DFS) Approach

The intuition behind using a DFS approach is that each student (or city) is a node, and the direct friend relationship as edges form a graph. We need to find the number of disconnected components in this graph, which corresponds to the number of provinces.

#### Steps
1. Create an array `visited` to track cities that have been visited.
2. Iterate over each city, using DFS to mark all connected cities as visited. Each DFS call from an unvisited city indicates a new province.
3. Increment the counter for each unvisited city (each new DFS call).

#### Code

```go
func findCircleNum(isConnected [][]int) int {
    n := len(isConnected)
    visited := make([]bool, n)
    provinces := 0

    var dfs func(int)
    dfs = func(city int) {
        for otherCity := 0; otherCity < n; otherCity++ {
            // If there is a direct connection and it's not visited
            if isConnected[city][otherCity] == 1 && !visited[otherCity] {
                visited[otherCity] = true
                dfs(otherCity) // dive deeper into this city
            }
        }
    }

    for i := 0; i < n; i++ {
        if !visited[i] {
            provinces++
            visited[i] = true
            dfs(i) // explore all cities in this province
        }
    }

    return provinces
}
```
#### Complexity
- **Time Complexity**: O(n^2), where n is the number of cities. We check each connection once.
- **Space Complexity**: O(n), for the `visited` array.

### Breadth First Search (BFS) Approach

Similar to DFS, using BFS to explore each connected component as a province.

#### Steps
1. Create an array `visited` to track cities that have been visited.
2. Use a queue to perform a BFS and mark all connected cities from an unvisited city.
3. Each BFS from an unvisited city indicates a new province.

#### Code

```go
func findCircleNumBFS(isConnected [][]int) int {
    n := len(isConnected)
    visited := make([]bool, n)
    provinces := 0

    for i := 0; i < n; i++ {
        if !visited[i] {
            provinces++
            queue := []int{i}
            visited[i] = true

            for len(queue) > 0 {
                city := queue[0]
                queue = queue[1:]
                for otherCity := 0; otherCity < n; otherCity++ {
                    // If there is a direct connection and it's not visited
                    if isConnected[city][otherCity] == 1 && !visited[otherCity] {
                        visited[otherCity] = true
                        queue = append(queue, otherCity) // explore this city
                    }
                }
            }
        }
    }

    return provinces
}
```
#### Complexity
- **Time Complexity**: O(n^2), same as DFS approach.
- **Space Complexity**: O(n), for the `visited` array and the BFS queue.

### Union-Find Approach

The Union-Find approach finds connected components using union operations. Each city starts as its own component, and union operations are used to merge cities connected by a direct edge.

#### Steps
1. Initialize each city as its own parent in a `parent` array.
2. Define find and union helper functions to manage component merging.
3. For each connection in the matrix, perform a union operation.
4. Count unique parents as the number of provinces.

#### Code

```go
func findCircleNumUnionFind(isConnected [][]int) int {
    n := len(isConnected)
    parent := make([]int, n)
    for i := 0; i < n; i++ {
        parent[i] = i
    }

    var find func(int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x]) // path compression
        }
        return parent[x]
    }

    var union func(int, int)
    union = func(x, y int) {
        rootX := find(x)
        rootY := find(y)
        if rootX != rootY {
            parent[rootX] = rootY // union the roots
        }
    }

    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            if isConnected[i][j] == 1 {
                union(i, j)
            }
        }
    }

    provinces := 0
    for i := 0; i < n; i++ {
        if parent[i] == i {
            provinces++
        }
    }

    return provinces
}
```
#### Complexity
- **Time Complexity**: O(n^2), mainly due to iterating through the matrix.
- **Space Complexity**: O(n), for the `parent` array, but efficient due to path compression.

