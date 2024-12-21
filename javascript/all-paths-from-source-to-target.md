# [Leetcode Problem 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches
1. [Depth-First Search (DFS) with Backtracking](#depth-first-search-dfs-with-backtracking)
2. [Breadth-First Search (BFS)](#breadth-first-search-bfs)

### Depth-First Search (DFS) with Backtracking

**Intuition**:  
The problem can be visualized as finding all paths in a directed acyclic graph (DAG) from node 0 to the last node `n-1`. The depth-first search approach is a natural fit for exploring paths in a graph as it allows us to explore each path completely until a dead-end or the target node is reached. By combining DFS with backtracking, we can explore all potential paths by dynamically exploring each option and then backtracking to explore other possibilities.

1. Use DFS to explore each node starting from node 0.
2. Keep track of the current path being explored.
3. If the target node `n-1` is reached, add a copy of the current path to the results.
4. Backtrack by removing the current node from the path before returning to explore other paths.
5. Continue this process until all paths from 0 to `n-1` have been explored.

```javascript
function allPathsSourceTarget(graph) {
    const result = [];
    const target = graph.length - 1;
    
    function dfs(node, path) {
        path.push(node);
      
        // If we reach the target node, copy the path to result
        if (node === target) {
            result.push([...path]);
        } else {
            // Continue exploring the neighbors of the current node
            for (const neighbor of graph[node]) {
                dfs(neighbor, path);
            }
        }
        
        // Backtrack by removing the current node from the path
        path.pop();
    }
    
    // Start DFS from the source node (0)
    dfs(0, []);
    return result;
}
```

**Time Complexity**: `O(2^n * n)` - In the worst-case scenario, you explore all paths in the DAG having `2^n` paths with `n` nodes. Therefore, for each path, it can take `O(n)` time to construct the path in the result.  
**Space Complexity**: `O(n)` - The recursion stack can go as deep as the number of nodes, plus the path storage in each recursion level.

### Breadth-First Search (BFS)

**Intuition**:  
While DFS is intuitive for path-finding problems, BFS can also be used when you want to explore the shortest paths or all possible paths from the source to the target level by level. BFS works well here as it explores paths layer by layer, which ensures that paths are explored in the order they are encountered.

1. Use a queue to explore paths starting from the source node 0.
2. For each path explored, enqueue new paths by extending the current path with the neighboring nodes.
3. If the target node `n-1` is reached, add the path to the results.
4. Continue this process until all paths from 0 to `n-1` have been explored.

```javascript
function allPathsSourceTargetBFS(graph) {
    const result = [];
    const target = graph.length - 1;
    let queue = [[0]];  // Start BFS with path containing only the source node
    
    while (queue.length > 0) {
        const path = queue.shift(); // Dequeue a path
        const lastNode = path[path.length - 1];
      
        // If the target node is reached, store the path in results
        if (lastNode === target) {
            result.push(path);
        } else {
            // Enqueue paths by extending the current path with each neighbor
            for (const neighbor of graph[lastNode]) {
                queue.push([...path, neighbor]);
            }
        }
    }
    
    return result;
}
```

**Time Complexity**: `O(2^n * n)` - Similar to DFS, the worst-case scenario involves exploring `2^n` paths with each having up to `n` nodes.  
**Space Complexity**: `O(2^n * n)` - Storage of all intermediate paths within the queue can effectively be up to `2^n` paths.

