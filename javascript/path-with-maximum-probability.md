# [1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approaches

1. [Basic Approach: DFS with Backtracking](#approach-1-basic-approach-dfs-with-backtracking)
2. [Improved Approach: Dijkstra's Algorithm with Priority Queue](#approach-2-improved-approach-dijkstras-algorithm-with-priority-queue)

### Approach 1: Basic Approach: DFS with Backtracking

In this approach, we use Depth-First Search with backtracking to explore all possible paths from the `start` to the `end` node. At each step, we track the cumulative probability of reaching the current node. The maximum probability of all paths leading to the `end` node is stored and returned. This approach is straightforward but not efficient, particularly for large graphs, as it explores every possible path.

#### Intuition:

- Use a DFS approach to explore all paths.
- Track cumulative probabilities along each path.
- Backtrack when no more outgoing edges exist, or when reaching the end node.

```javascript
function maxProbability(n, edges, succProb, start, end) {
    const graph = new Map();
    
    // Create graph
    for (let i = 0; i < edges.length; i++) {
        const [u, v] = edges[i];
        if (!graph.has(u)) graph.set(u, []);
        if (!graph.has(v)) graph.set(v, []);
        graph.get(u).push([v, succProb[i]]);
        graph.get(v).push([u, succProb[i]]);
    }

    let maxProb = 0;

    function dfs(node, prob, visited) {
        if (node === end) {
            maxProb = Math.max(maxProb, prob);
            return;
        }
        visited[node] = true;

        for (const [neighbor, edgeProb] of (graph.get(node) || [])) {
            if (!visited[neighbor]) {
                dfs(neighbor, prob * edgeProb, visited);
            }
        }

        visited[node] = false;
    }

    dfs(start, 1, new Array(n).fill(false));
    return maxProb;
}
```

**Time Complexity:** O(V + E) per DFS call, where V is the number of vertices and E is the number of edges.

**Space Complexity:** O(V) for storing the visited array and recursion stack.

### Approach 2: Improved Approach: Dijkstra's Algorithm with Priority Queue

This approach leverages Dijkstra's algorithm to find the path with the maximum probability from `start` to `end`. Instead of using addition in classical Dijkstra, here we use multiplication due to the nature of probabilities, and we maximize rather than minimize.

#### Intuition:

- Use a priority queue (or a max-heap) to always expand the most promising node with the highest probability.
- Similar to BFS, but nodes are expanded based on their current best known probability.
- Upon reaching the end node, return its probability.

```javascript
function maxProbability(n, edges, succProb, start, end) {
    const graph = new Map();
    
    // Create the graph
    for (let i = 0; i < edges.length; i++) {
        const [u, v] = edges[i];
        if (!graph.has(u)) graph.set(u, []);
        if (!graph.has(v)) graph.set(v, []);
        graph.get(u).push([v, succProb[i]]);
        graph.get(v).push([u, succProb[i]]);
    }

    const prob = new Array(n).fill(0);
    prob[start] = 1;

    const pq = new MaxPriorityQueue({ priority: (x) => x[1] });
    pq.enqueue([start, 1]);

    while (!pq.isEmpty()) {
        const [node, nodeProb] = pq.dequeue().element;

        if (node === end) {
            return nodeProb;
        }

        for (const [neighbor, edgeProb] of (graph.get(node) || [])) {
            const newProb = nodeProb * edgeProb;
            if (newProb > prob[neighbor]) {
                prob[neighbor] = newProb;
                pq.enqueue([neighbor, newProb]);
            }
        }
    }

    return 0;
}
```

**Time Complexity:** O(E log V) due to priority queue operations, where E is the number of edges and V is the number of nodes.

**Space Complexity:** O(V + E) for storing graph and additional arrays for probabilities and priority queue.

