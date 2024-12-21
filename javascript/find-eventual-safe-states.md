# [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approaches
1. [Depth-First Search (DFS) with Recursion](#approach-1-depth-first-search-dfs-with-recursion)
2. [Kahn's Algorithm for Topological Sorting (BFS)](#approach-2-kahns-algorithm-for-topological-sorting-bfs)

### Approach 1: Depth-First Search (DFS) with Recursion

**Intuition:**
- The problem can be understood as finding nodes in a Directed Graph that do not have cycles leading out from them.
- A node is considered safe if all paths starting from that node lead to terminal nodes (nodes with no outgoing paths).
- Use DFS to detect cycles; nodes that are not part of any cycle are safe.
- Track the status of nodes during the traversal using a `state` array to mark them as `visited`, `visiting`, or `safe`.

**Approach:**
- Initialize a `state` array to keep track of three states: `-1` (unvisited), `0` (visiting), `1` (safe).
- Perform DFS starting from each node:
  - If we encounter a node marked as `safe`, we return true immediately.
  - If we encounter a node marked as `visiting`, it indicates a cycle.
  - Mark the current node as `visiting` and explore its neighbors.
  - If any neighbor leads to a cycle, mark the node as unsafe.
  - After exploring all neighbors, mark the node as `safe`.
- Collect all nodes marked as safe at the end of the traversal.

```javascript
function eventualSafeNodes(graph) {
    const n = graph.length;
    const state = new Array(n).fill(-1); // states: -1=unvisited, 0=visiting, 1=safe

    const isSafe = (node) => {
        if (state[node] !== -1) {
            return state[node] === 1; // Return true if node is marked safe
        }

        state[node] = 0; // Mark node as visiting

        for (const neighbor of graph[node]) {
            if (!isSafe(neighbor)) {
                return false; // If any neighbor forms a cycle, return false
            }
        }

        state[node] = 1; // All paths from this node are safe
        return true;
    };

    const result = [];
    for (let i = 0; i < n; i++) {
        if (isSafe(i)) {
            result.push(i);
        }
    }
    return result;
}
```
**Time Complexity:**
- O(V + E), where V is the number of nodes and E is the number of edges.
- Each node and edge is visited once.

**Space Complexity:**
- O(V), for the recursion stack and the `state` array.

### Approach 2: Kahn's Algorithm for Topological Sorting (BFS)

**Intuition:**
- Reverse the graph, making edges point from end nodes to start nodes.
- Nodes with zero indegree in the reversed graph are terminal nodes in the original graph.
- Perform a topological sort using BFS, iteratively removing nodes with zero indegree and marking them as safe.

**Approach:**
- Reverse the edges of the graph to track incoming nodes.
- Use an indegree array to track the number of incoming edges for each node.
- Use a queue to process all nodes with zero indegree.
- For each node processed, decrement the indegree of its neighbors and add to the queue if they reach zero indegree.
- Collect nodes processed in the topological order, which are safe states.

```javascript
function eventualSafeNodesKahn(graph) {
    const n = graph.length;
    const reverseGraph = Array.from({ length: n }, () => []);
    const indegree = new Array(n).fill(0);

    // Building the reverse graph
    for (let node = 0; node < n; node++) {
        for (const neighbor of graph[node]) {
            reverseGraph[neighbor].push(node); // Reversing the edge direction
            indegree[node]++;
        }
    }

    const queue = [];
    const safeNodes = [];

    // Nodes with zero indegree initially
    for (let i = 0; i < n; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }

    while (queue.length > 0) {
        const node = queue.shift(); // Process node
        safeNodes.push(node);

        for (const prevNode of reverseGraph[node]) {
            indegree[prevNode]--;
            if (indegree[prevNode] === 0) {
                queue.push(prevNode);
            }
        }
    }

    return safeNodes.sort((a, b) => a - b); // Return sorted safe nodes
}
```
**Time Complexity:**
- O(V + E), where V is the number of nodes and E is the number of edges.
- Each node and edge is processed once.

**Space Complexity:**
- O(V + E), to store the reversed graph and maintain the queue.

