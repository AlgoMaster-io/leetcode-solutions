[Leetcode Problem 417: Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

### Solutions:
- [Approach 1: Brute Force DFS](#approach-1-brute-force-dfs)
- [Approach 2: Bidirectional DFS](#approach-2-bidirectional-dfs)
- [Approach 3: Bidirectional BFS](#approach-3-bidirectional-bfs)

---

### Approach 1: Brute Force DFS

#### Intuition:
The problem involves checking for each cell if water can flow to both the Pacific and Atlantic oceans. A straightforward way to do this is to perform a DFS from each cell, checking whether it can reach each ocean. This will check all paths originating from each cell leading to the oceans.

#### Code:
```go
func pacificAtlantic(heights [][]int) [][]int {
    if len(heights) == 0 || len(heights[0]) == 0 {
        return [][]int{}
    }
    m, n := len(heights), len(heights[0])
    var result [][]int

    def canFlow(i, j int, oceanCanReach [][]bool, prevHeight int) bool {
        // Check boundary conditions and whether we've visited
        if i < 0 || j < 0 || i >= m || j >= n || 
           oceanCanReach[i][j] || heights[i][j] < prevHeight {
            return false
        }
        return true
    }
    
    pacificCanReach := make([][]bool, m)
    atlanticCanReach := make([][]bool, m)
    for i := range pacificCanReach {
        pacificCanReach[i] = make([]bool, n)
        atlanticCanReach[i] = make([]bool, n)
    }
    
    var dfs func(i, j int, oceanCanReach [][]bool)
    dfs = func(i, j int, oceanCanReach [][]bool) {
        // Mark the current cell as reachable by the ocean
        oceanCanReach[i][j] = true
        // Move in 4 possible directions
        directions := [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
        for _, dir := range directions {
            newX, newY := i+dir[0], j+dir[1]
            if canFlow(newX, newY, oceanCanReach, heights[i][j]) {
                dfs(newX, newY, oceanCanReach)
            }
        }
    }
    
    // Check from edges (can be optimized by excluding the runs from points already visited)
    for i := 0; i < m; i++ {
        dfs(i, 0, pacificCanReach)
        dfs(i, n-1, atlanticCanReach)
    }
    for j := 0; j < n; j++ {
        dfs(0, j, pacificCanReach)
        dfs(m-1, j, atlanticCanReach)
    }
    
    // Collect results where both oceans can be reached
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if pacificCanReach[i][j] && atlanticCanReach[i][j] {
                result = append(result, []int{i, j})
            }
        }
    }
    return result
}
```

#### Time Complexity:
- O((m*n)^2) 

#### Space Complexity:
- O(m*n)

---

### Approach 2: Bidirectional DFS

#### Intuition:
Instead of simulating water flow from each cell, we can simulate the opposite flow: from the oceans into the grid. We initiate DFS from the ocean boundaries, marking all cells that can reach the ocean from those boundaries.

#### Code:
```go
func pacificAtlantic(heights [][]int) [][]int {
    if len(heights) == 0 {
        return [][]int{}
    }
    m, n := len(heights), len(heights[0])
    var dfs func(int, int, [][]bool)
    dfs = func(x, y int, visited [][]bool) {
        directions := [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
        visited[x][y] = true
        for _, d := range directions {
            newX, newY := x + d[0], y + d[1]
            if newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY] && heights[newX][newY] >= heights[x][y] {
                dfs(newX, newY, visited)
            }
        }
    }

    pacificVisited := make([][]bool, m)
    atlanticVisited := make([][]bool, m)
    for i := range pacificVisited {
        pacificVisited[i] = make([]bool, n)
        atlanticVisited[i] = make([]bool, n)
    }
    
    for i := 0; i < m; i++ {
        dfs(i, 0, pacificVisited)
        dfs(i, n-1, atlanticVisited)
    }
    for j := 0; j < n; j++ {
        dfs(0, j, pacificVisited)
        dfs(m-1, j, atlanticVisited)
    }
    
    var result [][]int
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if pacificVisited[i][j] && atlanticVisited[i][j] {
                result = append(result, []int{i, j})
            }
        }
    }
    return result
}
```

#### Time Complexity:
- O(m*n)

#### Space Complexity:
- O(m*n)

---

### Approach 3: Bidirectional BFS

#### Intuition:
Similar to bidirectional DFS, but we utilize BFS to leverage its iterative nature which can be easier to handle than recursion for large inputs. This ensures all nodes at the same "shore distance" are processed together, lending itself well to the problem.

#### Code:
```go
func pacificAtlantic(heights [][]int) [][]int {
    if len(heights) == 0 || len(heights[0]) == 0 {
        return [][]int{}
    }
    m, n := len(heights), len(heights[0])
    
    directions := [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
    
    bfs := func(queue [][2]int) [][]bool {
        visited := make([][]bool, m)
        for i := range visited {
            visited[i] = make([]bool, n)
        }
        
        for len(queue) > 0 {
            x, y := queue[0][0], queue[0][1]
            queue = queue[1:]
            visited[x][y] = true

            for _, d := range directions {
                newX, newY := x+d[0], y+d[1]
                if newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY] && heights[newX][newY] >= heights[x][y] {
                    queue = append(queue, [2]int{newX, newY})
                    visited[newX][newY] = true
                }
            }
        }
        
        return visited
    }
    
    pacificQueue, atlanticQueue := make([][2]int, 0), make([][2]int, 0)
    for i := 0; i < m; i++ {
        pacificQueue = append(pacificQueue, [2]int{i, 0})
        atlanticQueue = append(atlanticQueue, [2]int{i, n-1})
    }
    for j := 0; j < n; j++ {
        pacificQueue = append(pacificQueue, [2]int{0, j})
        atlanticQueue = append(atlanticQueue, [2]int{m-1, j})
    }
    
    pacificVisited := bfs(pacificQueue)
    atlanticVisited := bfs(atlanticQueue)
    
    var result [][]int
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if pacificVisited[i][j] && atlanticVisited[i][j] {
                result = append(result, []int{i, j})
            }
        }
    }
    return result
}
```

#### Time Complexity:
- O(m*n)

#### Space Complexity:
- O(m*n)

This approach utilizes BFS which generally results in the same time complexity but can be more memory-efficient for certain recursive depth limits due to avoiding deep recursion stacks.

