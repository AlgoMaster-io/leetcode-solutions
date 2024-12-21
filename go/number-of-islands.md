# [Leetcode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach

- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find (Disjoint Set Union)](#approach-3-union-find-disjoint-set-union)

### Approach 1: Depth-First Search (DFS)

**Intuition**:  
We can traverse each cell of the grid. When we find an unvisited land cell (`'1'`), we start a DFS from that cell, marking all connected land cells as visited. The entire connected component of cells forms one island, and we increase our island count.

**Go Code**:
```go
func numIslands(grid [][]byte) int {
    if len(grid) == 0 {
        return 0
    }

    var rows = len(grid)
    var cols = len(grid[0])
    var count = 0
    
    // DFS function to mark visited cells
    var dfs func(int, int)
    dfs = func(r int, c int) {
        if r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] == '0' {
            return
        }
        // Mark the current cell as visited
        grid[r][c] = '0'
        // Visit all 4 neighbors
        dfs(r-1, c)
        dfs(r+1, c)
        dfs(r, c-1)
        dfs(r, c+1)
    }
    
    // Traverse through each cell in the grid
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == '1' {
                count++
                dfs(r, c) // Start DFS and mark all connected lands
            }
        }
    }
    return count
}
```

**Time Complexity**: O(M * N), where M is the number of rows and N is the number of columns. We visit each cell once.  
**Space Complexity**: O(M * N) in worst case for the recursion stack if the grid is all land.

### Approach 2: Breadth-First Search (BFS)

**Intuition**:  
Similar to DFS, we can also use BFS to explore all connected land cells starting from each unvisited land cell (`'1'`). We use a queue to help explore the island layer by layer.

**Go Code**:
```go
import "container/list"

func numIslands(grid [][]byte) int {
    if len(grid) == 0 {
        return 0
    }

    var rows = len(grid)
    var cols = len(grid[0])
    var count = 0

    // A function to perform BFS traversal
    var bfs func(int, int)
    bfs = func(r int, c int) {
        queue := list.New()
        queue.PushBack([2]int{r, c})
        grid[r][c] = '0' // Mark as visited

        directions := [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

        for queue.Len() > 0 {
            cell := queue.Remove(queue.Front()).([2]int)
            
            for _, dir := range directions {
                nr, nc := cell[0]+dir[0], cell[1]+dir[1]

                if nr >= 0 && nc >= 0 && nr < rows && nc < cols && grid[nr][nc] == '1' {
                    queue.PushBack([2]int{nr, nc})
                    grid[nr][nc] = '0' // Mark as visited
                }
            }
        }
    }

    // Traverse through each cell in the grid
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == '1' {
                count++
                bfs(r, c) // Start BFS
            }
        }
    }
    return count
}
```

**Time Complexity**: O(M * N), where M is the number of rows and N is the number of columns.  
**Space Complexity**: O(min(M, N)) for the BFS queue in the worst case where the width or height of the grid is entirely land.

### Approach 3: Union-Find (Disjoint Set Union)

**Intuition**:  
Using a union-find (disjoint-set) structure, we can treat each land cell as a node in a graph. We perform union operations between adjacent land cells, and eventually, the number of distinct sets will be equal to the number of islands.

**Go Code**:
```go
type UnionFind struct {
    parent, rank []int
    count        int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := 0; i < n; i++ {
        parent[i] = i
    }
    return &UnionFind{parent, rank, 0}
}

func (uf *UnionFind) find(p int) int {
    if uf.parent[p] != p {
        uf.parent[p] = uf.find(uf.parent[p])
    }
    return uf.parent[p]
}

func (uf *UnionFind) union(p, q int) {
    rootP := uf.find(p)
    rootQ := uf.find(q)
    if rootP != rootQ {
        if uf.rank[rootP] > uf.rank[rootQ] {
            uf.parent[rootQ] = rootP
        } else if uf.rank[rootP] < uf.rank[rootQ] {
            uf.parent[rootP] = rootQ
        } else {
            uf.parent[rootQ] = rootP
            uf.rank[rootP]++
        }
        uf.count--
    }
}

func numIslands(grid [][]byte) int {
    if len(grid) == 0 {
        return 0
    }

    var rows = len(grid)
    var cols = len(grid[0])

    uf := NewUnionFind(rows * cols)
    count := 0

    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == '1' {
                index := r*cols + c
                uf.parent[index] = index // Initialize the node
                uf.count++
                
                // Check right and down for union
                if c+1 < cols && grid[r][c+1] == '1' {
                    uf.union(index, index+1)
                }
                if r+1 < rows && grid[r+1][c] == '1' {
                    uf.union(index, index+cols)
                }
            }
        }
    }
    return uf.count
}
```

**Time Complexity**: O(M * N * α(M*N)), where α is the Inverse Ackermann function, which grows very slowly.  
**Space Complexity**: O(M * N) for the union-find data structure.

