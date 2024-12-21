# [Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Navigation
- [Approach 1: Depth First Search (DFS) with Two Colors](#approach-1-depth-first-search-dfs-with-two-colors)
- [Approach 2: Breadth First Search (BFS) with Two Colors](#approach-2-breadth-first-search-bfs-with-two-colors)

## Approach 1: Depth First Search (DFS) with Two Colors

### Intuition
The problem of determining if a graph is bipartite can be thought of in terms of coloring the graph with two colors such that no two adjacent vertices share the same color. We can use a DFS technique to dive into nodes and try to color them alternately. If we encounter a node that cannot be colored according to the two-color rule, the graph is not bipartite.

### Code
```javascript
function isBipartite(graph) {
    // Create a color array to store color assignments to nodes, -1 indicating uncolored
    const color = Array(graph.length).fill(-1);

    // Helper function for DFS
    function dfs(node, c) {
        // Assign the color c to the current node
        color[node] = c;

        // Explore all the neighbors
        for (let neighbor of graph[node]) {
            // If the neighbor is uncolored, color it with the opposite color, apply dfs
            if (color[neighbor] === -1) {
                if (!dfs(neighbor, 1 - c)) {
                    return false;
                }
            } else if (color[neighbor] === color[node]) {
                // If the neighbor is colored and has the same color as the current node, not bipartite
                return false;
            }
        }
        return true;
    }

    // Try to color each component of the graph
    for (let i = 0; i < graph.length; i++) {
        if (color[i] === -1) {
            if (!dfs(i, 0)) {
                return false;
            }
        }
    }

    return true;
}
```

### Complexity Analysis
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. This is because we traverse each edge and vertex once.
- **Space Complexity:** O(V), for storing the color array and recursion stack during DFS.

---

## Approach 2: Breadth First Search (BFS) with Two Colors

### Intuition
Similarly to the DFS approach, we can use BFS to color the graph. The main idea remains the same: use an iterative approach where we color the node and then process all its neighbors by adding them to a queue. If a contradiction is found during this process, the graph isn't bipartite.

### Code
```javascript
function isBipartite(graph) {
    const color = Array(graph.length).fill(-1);

    // Traverse each component
    for (let start = 0; start < graph.length; start++) {
        if (color[start] !== -1) continue;

        // Start BFS from each uncolored node
        let queue = [start];
        color[start] = 0; // Start coloring with color 0

        while (queue.length > 0) {
            let node = queue.shift();

            for (let neighbor of graph[node]) {
                if (color[neighbor] === -1) {
                    // If uncolored, assign the opposite color
                    color[neighbor] = 1 - color[node];
                    queue.push(neighbor);
                } else if (color[neighbor] === color[node]) {
                    // If colored with the same color as current node
                    return false;
                }
            }
        }
    }

    return true;
}
```

### Complexity Analysis
- **Time Complexity:** O(V + E), similar to DFS, since each vertex and edge is processed only once.
- **Space Complexity:** O(V), for the color array and the queue used for BFS.

Each approach provides a versatile toolset to tackle the problem of graph bipartiteness, offering both depth-first and breadth-first strategies. Choose based on comfort or need within specific constraints of your environment.

