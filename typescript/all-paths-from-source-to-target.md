# [Leetcode 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches
- [Approach 1: Depth First Search (DFS) with Backtracking](#approach-1-depth-first-search-dfs-with-backtracking)
- [Approach 2: Breadth First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Recursive DFS with Path Caching](#approach-3-recursive-dfs-with-path-caching)

---

## Approach 1: Depth First Search (DFS) with Backtracking

### Intuition
Start the search from the source node (node 0) and explore all possible paths using DFS. For each path, we backtrack by removing the last node and then exploring other possibilities. This approach allows us to effectively generate all possible paths from source to target.

### Steps
1. Initialize an empty list `results` to store all the paths from source to target.
2. Use a helper function `dfs` to perform the depth-first search:
   - If the current node is the target node, add the current path to `results`.
   - If not, for each neighbor of the current node, add the neighbor to the path, recursively call `dfs`, and then backtrack.
3. Start the DFS from node 0 with an initial path containing only the source.

### Code
```typescript
function allPathsSourceTarget(graph: number[][]): number[][] {
    const results: number[][] = [];
    
    function dfs(currentNode: number, path: number[]) {
        // If target node is reached, add path to results
        if (currentNode === graph.length - 1) {
            results.push([...path]);
            return;
        }

        // Explore each neighbor
        for (const neighbor of graph[currentNode]) {
            path.push(neighbor);  // Add the neighbor to path
            dfs(neighbor, path);  // Recurse
            path.pop();           // Backtrack
        }
    }

    dfs(0, [0]);  // Start DFS with source node

    return results;
}
```

### Complexity
- **Time Complexity**: \(O(2^N \cdot N)\) where \(N\) is the number of nodes. In the worst case, each node may have paths like a binary tree, leading to exponential paths.
- **Space Complexity**: \(O(N)\) for the recursion call stack and additional space for storing paths.

---

## Approach 2: Breadth First Search (BFS)

### Intuition
Use a queue to explore paths level by level from the source node. For every path ending at a node, extend the path by appending each of its neighbors and enqueue the new paths for further exploration. BFS ensures the earliest discovery of shortest paths in unweighted graphs.

### Steps
1. Initialize a queue with a path containing only the source.
2. While the queue is not empty:
   - Dequeue the first path.
   - For each neighbor of the last node in the path:
     - If the neighbor is the target, add the new path to results.
     - Else, enqueue the new path for further exploration.
   
### Code
```typescript
function allPathsSourceTargetBFS(graph: number[][]): number[][] {
    const results: number[][] = [];
    const queue: number[][] = [[0]];  // Initialize queue with path starting from source

    while (queue.length > 0) {
        const path = queue.shift()!;
        const currentNode = path[path.length - 1];

        for (const neighbor of graph[currentNode]) {
            const newPath = [...path, neighbor];  // Extend path to the neighbor
            if (neighbor === graph.length - 1) {
                results.push(newPath);  // If target reached, add to results
            } else {
                queue.push(newPath);  // Else, enqueue for exploration
            }
        }
    }

    return results;
}
```

### Complexity
- **Time Complexity**: \(O(2^N \cdot N)\) as we potentially explore a large number of possible paths.
- **Space Complexity**: \(O(2^N \cdot N)\) due to storage of paths in the queue.

---

## Approach 3: Recursive DFS with Path Caching

### Intuition
Utilize a recursive DFS but with a twist—cache the already computed paths from any node to the target. This reduces redundant computations and optimizes the overall solution similar to dynamic programming.

### Steps
1. Create an array `cache`, where each entry is a memoized result of paths from that node to the target.
2. Use a recursive function to explore paths from the current node:
   - If a cached result exists, return it directly.
   - Otherwise, if the current node is the target, return the path.
   - Else, explore each neighbor, combine the results, and cache them.
3. Call the recursive function starting from node 0.

### Code
```typescript
function allPathsSourceTargetDFSCache(graph: number[][]): number[][] {
    const cache: Map<number, number[][]> = new Map();

    function dfs(node: number): number[][] {
        if (cache.has(node)) {
            return cache.get(node)!;  // Return cached paths
        }
        
        if (node === graph.length - 1) {
            return [[node]];  // Return path ending with target
        }

        const result = [];
        for (const neighbor of graph[node]) {
            const pathsFromNeighbor = dfs(neighbor);  // Recursive dfs call
            for (const path of pathsFromNeighbor) {
                result.push([node, ...path]);  // Add current node to each path
            }
        }

        cache.set(node, result);  // Cache the computed paths from this node
        return result;
    }

    return dfs(0);
}
```

### Complexity
- **Time Complexity**: \(O(2^N \cdot N)\) similar to the other approaches due to the structure of the graph.
- **Space Complexity**: \(O(N^2)\) for caching paths, assuming the worst case of storing up to \(N\) paths of length \(N\).

---

