# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approaches:
- [Basic Approach: Breadth-First Search (BFS)](#basic-approach-breadth-first-search-bfs)
- [Optimal Approach: BFS with State Compression and Bitmasking](#optimal-approach-bfs-with-state-compression-and-bitmasking)

### Basic Approach: Breadth-First Search (BFS)

#### Intuition:
The problem is about finding the shortest path that visits all nodes in an unweighted graph. A basic approach to handle this kind of problem is to use Breadth-First Search (BFS). We can perform a BFS for each node to explore all possible paths. However, this approach results in multiple explorations of the same paths and nodes.

The idea is to start BFS from each node treating it as the starting point, while maintaining a visited status for each node. The complexity arises from needing to continue the search until no further actions are possible within a reasonable constraint.

#### Approach:
- Perform BFS from every node in the graph.
- Use a queue to track the current node and the set of visited nodes.
- For each step, mark a node as visited and enqueue its neighbors until all nodes are visited.
- Recognize when all nodes have been visited as the condition for stopping the search.

#### Code:
```typescript
function shortestPathAllNodes(graph: number[][]): number {
    const n = graph.length;
    const queue: [number, Set<number>, number][] = [];

    // Initialize the queue with each node as the starting point
    for (let start = 0; start < n; start++) {
        queue.push([start, new Set([start]), 0]);
    }

    while (queue.length > 0) {
        const [node, visited, pathLength] = queue.shift()!;

        // If all nodes are visited, return the path length
        if (visited.size === n) {
            return pathLength;
        }

        // Explore neighbors
        for (const neighbor of graph[node]) {
            // If the neighbor has not been visited in this path, enqueue the new state
            if (!visited.has(neighbor)) {
                const newVisited = new Set(visited);
                newVisited.add(neighbor);
                queue.push([neighbor, newVisited, pathLength + 1]);
            }
        }
    }

    return 0;
}

// Time complexity: O(N!) where N is the number of nodes (exploring all possible paths)
// Space complexity: O(N * 2^N) for the queue and visited sets
```

### Optimal Approach: BFS with State Compression and Bitmasking

#### Intuition:
The basic BFS approach becomes inefficient due to revisits and redundant calculations. We need a mechanism to efficiently remember which nodes have been visited in every state. By using a bitmask to represent visited nodes, we compact this information into a single integer value, allowing us to reduce the complexity.

In this optimized approach, each state is represented by both the current node and a bitmask of visited nodes. Instead of exploring combinations of sets of visited nodes, the bitmask helps check visited nodes efficiently.

#### Approach:
- Perform BFS with each state represented by (current_node, visited_mask).
- Use a queue to manage states, where each element tracks the node, the bitmask, and the path length.
- Use a set to track states that have already been processed to avoid redundant checks.

#### Code:
```typescript
function shortestPathLength(graph: number[][]): number {
    const n = graph.length;
    const allVisitedMask = (1 << n) - 1; // When all nodes are visited
    const queue: [number, number, number][] = []; // [node, visited_mask, path_length]
    const visited = Array.from({length: n}, () => new Set<number>());

    // Initialize the queue with each node as the starting point
    for (let start = 0; start < n; start++) {
        queue.push([start, 1 << start, 0]);
        visited[start].add(1 << start);
    }

    while (queue.length > 0) {
        const [node, visitedMask, pathLength] = queue.shift()!;

        // If all nodes are visited, return the path length
        if (visitedMask === allVisitedMask) {
            return pathLength;
        }

        // Explore neighbors
        for (const neighbor of graph[node]) {
            const newVisitedMask = visitedMask | (1 << neighbor);

            // Enqueue the new state if it hasn't been processed yet
            if (!visited[neighbor].has(newVisitedMask)) {
                visited[neighbor].add(newVisitedMask);
                queue.push([neighbor, newVisitedMask, pathLength + 1]);
            }
        }
    }

    return 0;
}

// Time complexity: O(N * 2^N), N nodes and each state is defined by a node and a subset of those nodes
// Space complexity: O(N * 2^N), for storing states in the queue and visited sets
```

