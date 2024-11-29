# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result

function allPathsSourceTarget(graph: number[][]): number[][] {
    const result: number[][] = [];
    const path: number[] = [];
    path.push(0); // Start from the source node
    dfs(graph, 0, path, result);
    return result;
}

function dfs(graph: number[][], node: number, path: number[], result: number[][]): void {
    if (node === graph.length - 1) {
        // If we reach the target node, add the current path to the result
        result.push([...path]);
        return;
    }

    for (const neighbor of graph[node]) {
        path.push(neighbor); // Add the neighbor to the current path
        dfs(graph, neighbor, path, result); // Recur for the neighbor
        path.pop(); // Backtrack to explore other paths
    }
}
```


