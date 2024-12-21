# [Leetcode 417: Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

## Approaches
- [Approach 1: DFS with Recursion](#approach-1-dfs-with-recursion)
- [Approach 2: BFS with Queue](#approach-2-bfs-with-queue)
- [Approach 3: Optimized Graph Traversal](#approach-3-optimized-graph-traversal)

### Approach 1: DFS with Recursion

#### Intuition
The problem can be visualized as finding cells that can flow water to both the Pacific and Atlantic oceans. By considering this as a reverse problem, you can attempt to perform a Depth First Search (DFS) starting from both oceans and mark reachable cells.

- Start DFS from all border cells that can flow into the Pacific, marking them as reachable.
- Likewise, perform DFS from all border cells that can flow into the Atlantic.
- The intersection of these two sets of reachable cells is the solution.

#### Code

```typescript
function pacificAtlantic(heights: number[][]): number[][] {
    const rows = heights.length;
    const cols = heights[0].length;
    const pacific: boolean[][] = Array.from({ length: rows }, () => Array(cols).fill(false));
    const atlantic: boolean[][] = Array.from({ length: rows }, () => Array(cols).fill(false));
    
    function dfs(row: number, col: number, visited: boolean[][], ocean: string): void {
        if (visited[row][col]) return;
        
        // Mark cell as visited for the respective ocean
        visited[row][col] = true;

        // Iterate over four possible directions
        const directions: [number, number][] = [[0, 1], [1, 0], [0, -1], [-1, 0]];
        for (let [dr, dc] of directions) {
            const newRow = row + dr;
            const newCol = col + dc;
            // Boundary and height conditions for DFS
            if (
                newRow >= 0 && newRow < rows &&
                newCol >= 0 && newCol < cols &&
                heights[newRow][newCol] >= heights[row][col]
            ) {
                dfs(newRow, newCol, visited, ocean);
            }
        }
    }
    
    // Initiate DFS from Pacific border
    for (let col = 0; col < cols; col++) {
        dfs(0, col, pacific, 'Pacific');
        dfs(rows - 1, col, atlantic, 'Atlantic');
    }
    for (let row = 0; row < rows; row++) {
        dfs(row, 0, pacific, 'Pacific');
        dfs(row, cols - 1, atlantic, 'Atlantic');
    }
    
    const result: number[][] = [];
    // Collect cells reachable by both oceans
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            if (pacific[row][col] && atlantic[row][col]) {
                result.push([row, col]);
            }
        }
    }
    
    return result;
}
```

#### Time and Space Complexity
- **Time Complexity**: O(M * N), where M and N are the dimensions of the grid. Each cell might be visited once per ocean.
- **Space Complexity**: O(M * N) for the visited arrays which keep track of reachability for each ocean.

### Approach 2: BFS with Queue

#### Intuition
Similar to DFS, but using BFS can be more intuitive in certain cases because it processes level by level. Each cell at the edge is enqueued, and you propagate the reachability condition with a queue.

#### Code

```typescript
function pacificAtlantic(heights: number[][]): number[][] {
    const rows = heights.length;
    const cols = heights[0].length;
    const pacificQueue: [number, number][] = [];
    const atlanticQueue: [number, number][] = [];
    const pacificReachable = new Set<string>();
    const atlanticReachable = new Set<string>();

    function bfs(queue: [number, number][], reachableSet: Set<string>): void {
        const directions: [number, number][] = [[0, 1], [1, 0], [0, -1], [-1, 0]];
        while (queue.length > 0) {
            const [row, col] = queue.shift()!;
            if (reachableSet.has(`${row},${col}`)) {
                continue;
            }
            reachableSet.add(`${row},${col}`);
            
            for (let [dr, dc] of directions) {
                const newRow = row + dr;
                const newCol = col + dc;
                if (
                    newRow >= 0 && newRow < rows &&
                    newCol >= 0 && newCol < cols &&
                    heights[newRow][newCol] >= heights[row][col]
                ) {
                    queue.push([newRow, newCol]);
                }
            }
        }
    }

    for (let col = 0; col < cols; col++) {
        pacificQueue.push([0, col]);
        atlanticQueue.push([rows - 1, col]);
    }
    for (let row = 0; row < rows; row++) {
        pacificQueue.push([row, 0]);
        atlanticQueue.push([row, cols - 1]);
    }

    bfs(pacificQueue, pacificReachable);
    bfs(atlanticQueue, atlanticReachable);

    const result: number[][] = [];
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            if (pacificReachable.has(`${row},${col}`) && atlanticReachable.has(`${row},${col}`)) {
                result.push([row, col]);
            }
        }
    }
    
    return result;
}
```

#### Time and Space Complexity
- **Time Complexity**: O(M * N), similar to DFS each cell is processed once per ocean.
- **Space Complexity**: O(M * N) for the reachable sets.

### Approach 3: Optimized Graph Traversal

For an optimized approach, typically enhancing DFS with memoization or using advanced data structures could help. However, for this problem, the DFS/BFS approaches already provide significant exploration efficiency. This solution naturally balances between flexibility and performance in the current state constraints.

For further optimization, consider reducing unnecessary state storage or using bidirectional search principles, but those are more complex and may not be needed given the problem constraints and input sizes provided by Leetcode. The BFS/DFS solutions are optimal in terms of practical execution for this problem scope.

Since this problem involves fairly standard grid traversal, the BFS/DFS method fits naturally with the imposed problem constraints and typical input size—balancing complexity and readability admirably.

