# [Leetcode 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: DFS for Coloring and Union-Find for Optimization](#approach-2-dfs-for-coloring-and-union-find-for-optimization)

### Approach 1: Brute Force 

#### Intuition
The problem can initially be approached by iterating over each zero in the grid, flipping it to one, and then utilizing a depth-first search (DFS) or breadth-first search (BFS) to find the size of the island that forms. This brute-force method despite being straightforward, can be really inefficient due to the repetitive exploration of the islands. Nonetheless, it's a foundational approach for understanding the problem.

#### Time Complexity
- O(N^4), where N is the grid length
- For each cell, we perform a DFS/BFS which can visit all cells again in the worst case, and we do this for each cell.

#### Space Complexity
- O(N^2), for the recursion stack if all the land is one component.

```go
func largestIsland(grid [][]int) int {
    n := len(grid)
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    
    var dfs func(int, int, [][]bool) int
    dfs = func(x, y int, visited [][]bool) int {
        stack := [][]int{{x, y}}
        visited[x][y] = true
        size := 1

        for len(stack) > 0 {
            curr := stack[len(stack)-1]
            stack = stack[:len(stack)-1]

            for _, d := range directions {
                nx, ny := curr[0]+d[0], curr[1]+d[1]
                if nx >= 0 && ny >= 0 && nx < n && ny < n && grid[nx][ny] == 1 && !visited[nx][ny] {
                    stack = append(stack, []int{nx, ny})
                    visited[nx][ny] = true
                    size++
                }
            }
        }
        return size
    }
    
    maxIsland := 0
    foundZero := false

    for i := 0; i < n; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 0 {
                visited := make([][]bool, n)
                for k := 0; k < n; k++ {
                    visited[k] = make([]bool, n)
                }
                
                grid[i][j] = 1
                foundZero = true
                maxIsland = max(maxIsland, dfs(i, j, visited))
                grid[i][j] = 0
            }
        }
    }
    
    if !foundZero {
        return n * n
    }
    
    return maxIsland
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Approach 2: DFS for Coloring and Union-Find for Optimization

#### Intuition
To optimize, we can color different islands with unique identifiers using DFS and count their sizes during the process. Then, when flipping a zero to one, calculate possible island sizes by uniting surrounding islands. This allows us to use precomputed values and avoid recalculating island sizes multiple times.

1. Color each island with a unique id (starting from 2).
2. Store island sizes in a map or array.
3. For each water cell (0), calculate the potential "new" island size if we were to fill it with land, by considering neighboring islands.

#### Time Complexity
- O(N^2), where N is the grid length. Each cell is visited a constant number of times (once during coloring, once during evaluation).

#### Space Complexity
- O(N^2), space used by the `color` grid and island sizes map or array.

```go
func largestIsland(grid [][]int) int {
    n := len(grid)
    if n == 0 {
        return 0
    }

    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    islandID := 2
    islandSizes := map[int]int{}

    var dfs func(x, y, id int) int
    dfs = func(x, y, id int) int {
        stack := [][]int{{x, y}}
        grid[x][y] = id
        size := 1
        for len(stack) > 0 {
            curr := stack[len(stack)-1]
            stack = stack[:len(stack)-1]

            for _, d := range directions {
                nx, ny := curr[0]+d[0], curr[1]+d[1]
                if nx >= 0 && ny >= 0 && nx < n && ny < n && grid[nx][ny] == 1 {
                    grid[nx][ny] = id
                    stack = append(stack, []int{nx, ny})
                    size++
                }
            }
        }
        return size
    }

    // Color each component and calculate their size
    for i := range grid {
        for j := range grid[i] {
            if grid[i][j] == 1 {
                size := dfs(i, j, islandID)
                islandSizes[islandID] = size
                islandID++
            }
        }
    }

    maxIsland := 0
    // Calculate the maximum possible island size
    for i := range grid {
        for j := range grid[i] {
            if grid[i][j] == 0 {
                seen := make(map[int]bool)
                size := 1 // Initial size for the newly flipped 1
                for _, d := range directions {
                    ni, nj := i+d[0], j+d[1]
                    if ni >= 0 && nj >= 0 && ni < n && nj < n {
                        id := grid[ni][nj]
                        if _, found := seen[id]; id > 1 && !found {
                            size += islandSizes[id]
                            seen[id] = true
                        }
                    }
                }
                maxIsland = max(maxIsland, size)
            }
        }
    }
    
    // If no zero was flipped, the grid was fully filled with 1s
    if maxIsland == 0 {
        return n * n
    }
    return maxIsland
}
```

This significant efficiency gain from approach 1 helps to solve the problem in scenarios closer to the input boundaries allowed by the problem's constraints, making it more practical for competitive scenarios.

