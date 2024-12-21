# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## Approaches:
1. [Depth-First Search (DFS)](#DFS)
2. [Breadth-First Search (BFS)](#BFS)
3. [Union-Find](#Union-Find)

### Depth-First Search (DFS)

**Intuition:**

The problem requires converting all 'O's that are surrounded by 'X's to 'X', but 'O's on the boundary (and connected to any boundary 'O') must remain unchanged. We can tackle this problem by considering two steps: identifying boundary-connected 'O's which shouldn't be changed, and converting the rest.

DFS is a good fit here. We can start a DFS from every 'O' located on the boundary. All 'O's connected to these boundary 'O's will be marked as non-surrounded. After marking, traverse all cells to flip unmarked 'O's to 'X' and change marked cells back to 'O'.

```go
func solve(board [][]byte) {
	if len(board) == 0 || len(board[0]) == 0 {
		return
	}

	// Directions for moving up, down, left and right
	directions := [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
	rows, cols := len(board), len(board[0])

	// Helper function to perform DFS and mark 'O's that connect to boundary
	var dfs func(int, int)
	dfs = func(x, y int) {
		// Boundary check and check if it's 'O'
		if x < 0 || x >= rows || y < 0 || y >= cols || board[x][y] != 'O' {
			return
		}

		// Mark the cell to 'E' (an arbitrary symbol to mark it as non-flippable)
		board[x][y] = 'E'

		// Traverse adjacent cells
		for _, direction := range directions {
			dfs(x+direction[0], y+direction[1])
		}
	}

	// Start DFS on all boundary 'O's
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if i == 0 || i == rows-1 || j == 0 || j == cols-1 {
				if board[i][j] == 'O' {
					dfs(i, j)
				}
			}
		}
	}

	// Flip all 'O's to 'X', since they are surrounded
	// Flip 'E' back to 'O', since they are connected to boundaries
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if board[i][j] == 'O' {
				board[i][j] = 'X'
			} else if board[i][j] == 'E' {
				board[i][j] = 'O'
			}
		}
	}
}
```

**Time Complexity:** O(N * M) - We potentially visit every cell once.

**Space Complexity:** O(N * M) - In the worst case, recursive call stack size in DFS which can be as large as the grid.

### Breadth-First Search (BFS)

**Intuition:**

BFS can be interpreted similarly to DFS. By initiating BFS traversal from the boundary 'O's, we guarantee that we identify all 'O's connected to the boundary. We begin by putting each 'O' along the border into a queue and mark it. For each cell processed, we expand in all four cardinal directions.

```go
import "container/list"

func solve(board [][]byte) {
	if len(board) == 0 || len(board[0]) == 0 {
		return
	}

	rows, cols := len(board), len(board[0])
	queue := list.New()

	// Mark boundary cells which are 'O'
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if (i == 0 || i == rows-1 || j == 0 || j == cols-1) && board[i][j] == 'O' {
				queue.PushBack([]int{i, j})
			}
		}
	}

	// Directions for moving up, down, left and right
	directions := [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}

	// Process the queue for BFS
	for queue.Len() > 0 {
		cell := queue.Remove(queue.Front()).([]int)
		x, y := cell[0], cell[1]

		// If it's 'O', mark it and add its unmarked neighbors to the queue
		if board[x][y] == 'O' {
			board[x][y] = 'E'
			
			for _, direction := range directions {
				nx, ny := x+direction[0], y+direction[1]
				if nx >= 0 && nx < rows && ny >= 0 && ny < cols && board[nx][ny] == 'O' {
					queue.PushBack([]int{nx, ny})
				}
			}
		}
	}

	// Convert all remaining 'O's to 'X's and 'E's back to 'O's
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if board[i][j] == 'O' {
				board[i][j] = 'X'
			} else if board[i][j] == 'E' {
				board[i][j] = 'O'
			}
		}
	}
}
```

**Time Complexity:** O(N * M) - We potentially visit every cell once.

**Space Complexity:** O(N * M) - In the worst case, the BFS queue can grow as large as the grid itself.

### Union-Find

**Intuition:**

Union-Find can be used to solve this problem by treating each 'O' as a node in a graph. We connect node 'O' to the virtual node when it is on the boundary or connected to another boundary 'O'. The idea is to find connected components of 'O's and check if they are connected to the virtual node which represents the boundary.

```go
type UnionFind struct {
	parent []int
	rank   []int
}

func NewUnionFind(size int) *UnionFind {
	parent := make([]int, size)
	rank := make([]int, size)
	for i := range parent {
		parent[i] = i
	}
	return &UnionFind{parent, rank}
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
	}
}

func (uf *UnionFind) isConnected(p, q int) bool {
	return uf.find(p) == uf.find(q)
}

func solve(board [][]byte) {
	if len(board) == 0 || len(board[0]) == 0 {
		return
	}

	rows, cols := len(board), len(board[0])
	uf := NewUnionFind(rows*cols + 1)
	virtualNode := rows * cols // This node is connected to all boundary 'O's

	// Connect first & last rows and first & last columns 'O's to the virtual node
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if board[i][j] == 'O' {
				if i == 0 || i == rows-1 || j == 0 || j == cols-1 {
					uf.union(i*cols+j, virtualNode)
				} else { // union with adjacent 'O's
					for _, d := range [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}} {
						x, y := i+d[0], j+d[1]
						if x >= 0 && x < rows && y >= 0 && y < cols && board[x][y] == 'O' {
							uf.union(i*cols+j, x*cols+y)
						}
					}
				}
			}
		}
	}

	// Flip 'O's to 'X' if they are not connected to the virtual node
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if board[i][j] == 'O' && !uf.isConnected(i*cols+j, virtualNode) {
				board[i][j] = 'X'
			}
		}
	}
}
```

**Time Complexity:** O(N * M) - Union find with path compression operates in near constant time, essentially linear due to the nearly constant-time complexity of union and find with path compression and union by rank.

**Space Complexity:** O(N * M) - UnionFind data structure stores arrays of size N * M plus an additional node for the boundary.

