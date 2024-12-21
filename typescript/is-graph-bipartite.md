# [Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Table of Contents:
1. [Basic Approach: DFS Coloring](#basic-approach-dfs-coloring)
2. [Optimized Approach: BFS Coloring](#optimized-approach-bfs-coloring)

## Basic Approach: DFS Coloring

**Intuition:**

A graph is bipartite if it can be colored using two colors such that no two adjacent nodes share the same color. In this approach, we will use Depth First Search (DFS) to attempt to color the graph with two colors. We will keep track of the color of each node using an array and attempt to color the graph while ensuring that no two adjacent nodes have the same color. If we find any conflict, the graph is not bipartite.

**Algorithm:**
1. Initialize a colors array of the same length as the graph with 0, indicating that no node has been colored.
2. Iterate through each node. If the node is not colored, initiate DFS to color it with color 1.
3. In DFS function, color the current node and then propagate to its adjacent nodes with the opposite color.
4. If an adjacent node is already colored with the same color, return false indicating it's not bipartite.
5. If all nodes are successfully colored, return true.

```typescript
function isBipartite(graph: number[][]): boolean {
    // Initialize a color array where 0 means no color, 1 and -1 are two different colors.
    const colors = new Array(graph.length).fill(0);

    // Helper function to perform DFS
    const dfs = (node: number, color: number): boolean => {
        // Color the node with the current color
        colors[node] = color;
        
        // Visit all adjacent nodes
        for (let neighbor of graph[node]) {
            // If the neighbor has the same color, it's not bipartite
            if (colors[neighbor] === color) return false;
            // If it's not colored, recursively color it with the opposite color
            if (colors[neighbor] === 0 && !dfs(neighbor, -color)) return false;
        }
        
        // All neighbors can be colored successfully
        return true;
    };

    // Check each node if it is not colored
    for (let i = 0; i < graph.length; i++) {
        if (colors[i] === 0 && !dfs(i, 1)) {
            return false;
        }
    }
    
    return true;
}
```

**Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges, as each node and edge is visited once.

**Space Complexity:** O(V), for the colors array and recursive call stack.

## Optimized Approach: BFS Coloring

**Intuition:**

In this approach, we use Breadth First Search (BFS) to color the graph. This method can also ensure that no two adjacent nodes are of the same color by examining each node's neighbors iteratively. BFS can be more suitable for problems involving shortest paths or level order traversal, as it explores neighbors level by level.

**Algorithm:**
1. Initialize a colors array of the same length as the graph with 0.
2. Use a queue to manage nodes to visit.
3. Iterate through each node. If the node is not colored, start BFS and color it with 1.
4. Process nodes from the queue, coloring the uncolored neighbors with the opposite color.
5. If any neighbor is already colored with the same color during processing, return false.
6. If all nodes are successfully colored without conflict, return true.

```typescript
function isBipartiteBFS(graph: number[][]): boolean {
    const colors = new Array(graph.length).fill(0);

    // Helper function to perform BFS
    const bfs = (start: number): boolean => {
        const queue: number[] = [];
        queue.push(start);
        colors[start] = 1;

        // Process nodes in the queue
        while (queue.length > 0) {
            const node = queue.shift();

            for (let neighbor of graph[node!]) {
                // If the neighbor has the same color, it's not bipartite
                if (colors[neighbor] === colors[node!]) return false;
                // If it's not colored, color it with the opposite color and add to queue
                if (colors[neighbor] === 0) {
                    colors[neighbor] = -colors[node!];
                    queue.push(neighbor);
                }
            }
        }

        return true;
    };

    for (let i = 0; i < graph.length; i++) {
        if (colors[i] === 0 && !bfs(i)) {
            return false;
        }
    }

    return true;
}
```

**Time Complexity:** O(V + E), similar to DFS, as each node and edge is processed once.

**Space Complexity:** O(V), for the colors array and the queue used in BFS.

