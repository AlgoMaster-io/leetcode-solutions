# [802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approaches
1. [DFS with Marking Unsafe Cycles](#dfs-with-marking-unsafe-cycles)
2. [Topological Sort (Reversed Graph)](#topological-sort-reversed-graph)

---

### 1. DFS with Marking Unsafe Cycles

#### Intuition
The problem is about identifying eventual safe states in a directed graph. A state is considered safe if it doesn't lead into a cycle. We can utilize a depth-first search (DFS) to explore the graph and mark nodes that are part of cycles, then identify the nodes which are eventually safe.

Key insights:
- Use a color array to track the state of each node:
  - `0`: Node has not been visited.
  - `1`: Node is currently being visited (part of current path).
  - `2`: Node is confirmed to be safe.
- A node is part of a cycle if during DFS traversal, we revisit a node that’s currently in the path (`color[node] == 1`).

#### Code
```typescript
function eventualSafeNodes(graph: number[][]): number[] {
    const n = graph.length;
    const color = new Array(n).fill(0);
    
    function dfs(node: number): boolean {
        // If this node is currently being visited, it's part of a cycle
        if (color[node] === 1) return false;
        
        // If it's already marked safe, return true
        if (color[node] === 2) return true;
        
        // Mark node as being visited
        color[node] = 1;
        
        for (const neighbor of graph[node]) {
            // If neighbor isn't safe, this node isn't safe
            if (!dfs(neighbor)) return false;
        }
        
        // Mark node as safe
        color[node] = 2;
        return true;
    }
    
    const result: number[] = [];
    for (let i = 0; i < n; i++) {
        if (dfs(i)) {
            result.push(i);
        }
    }
    
    return result;
}
```

#### Time and Space Complexity
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. Each node and edge is visited once.
- **Space Complexity:** O(V), due to the recursive stack and the coloring array.

---

### 2. Topological Sort (Reversed Graph)

#### Intuition
Instead of directly looking at the graph, we can reverse it and then consider reaching nodes with no further connections. This approach leverages concepts similar to topological sorting by using indegrees.

Key insights:
- Reverse the graph.
- Nodes with 0 out-degree in the reversed graph are safe since they can't start a cycle.
- Traverse starting from nodes with 0 in-degree and reduce the in-degrees of their predecessors in the reversed graph.

#### Code
```typescript
function eventualSafeNodes(graph: number[][]): number[] {
    const n = graph.length;
    const reversedGraph: number[][] = Array.from({ length: n }, () => []);
    const outdegree = new Array(n).fill(0);
    const queue: number[] = [];
    const result: number[] = [];

    // Reverse graph and calculate out-degrees
    for (let start = 0; start < n; start++) {
        for (const end of graph[start]) {
            reversedGraph[end].push(start);
        }
        outdegree[start] = graph[start].length;
    }

    // Queue initiation for nodes with zero out-degree
    for (let i = 0; i < n; i++) {
        if (outdegree[i] === 0) {
            queue.push(i);
        }
    }

    while (queue.length > 0) {
        const node = queue.shift()!;
        result.push(node);

        for (const prevNode of reversedGraph[node]) {
            outdegree[prevNode]--;
            if (outdegree[prevNode] === 0) {
                queue.push(prevNode);
            }
        }
    }

    return result.sort((a, b) => a - b); // Sort results as required by the problem statement.
}
```

#### Time and Space Complexity
- **Time Complexity:** O(V + E), similar reasoning: each node and edge is processed once.
- **Space Complexity:** O(V + E), for storing the reversed graph and outdegrees.

Both approaches efficiently find eventual safe states, each with a unique method of traversal and marking nodes, leveraging DFS and topological-like traversal respectively.

