# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
javascript
```javascript
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result

var allPathsSourceTarget = function(graph) {
    let result = [];
    let path = [0]; // Start from the source node
    dfs(graph, 0, path, result);
    return result;
};

function dfs(graph, node, path, result) {
    if (node === graph.length - 1) {
        // If we reach the target node, add the current path to the result
        result.push([...path]);
        return;
    }

    for (let neighbor of graph[node]) {
        path.push(neighbor); // Add the neighbor to the current path
        dfs(graph, neighbor, path, result); // Recur for the neighbor
        path.pop(); // Backtrack to explore other paths
    }
}
```

