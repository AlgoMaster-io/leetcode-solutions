# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approaches
- [Approach 1: Breadth-First Search (BFS) with Bitmask State Representation](#approach-1-bfs-with-bitmask-state-representation)

---

## Approach 1: Breadth-First Search (BFS) with Bitmask State Representation

### Intuition
The problem requires visiting all nodes in a graph, ideally in the shortest path possible. This problem can be visualized as a state-space search problem where:

- Each state consists of the current node and a bitmask representing all visited nodes.
- We are to visit all nodes, which is equivalent to setting all bits in the bitmask for nodes as visited.

Using Breadth-First Search is appropriate here because it explores neighbor nodes level by level, guaranteeing the shortest path assuming constant edge weights. The graph can also be perfectly represented in this manner since it is unweighted.

The major insight is to use a bitmask of length `n` (where `n` is the number of nodes) to represent visited nodes, which transforms our state space from potentially exponential space to manageable space through efficient bit manipulation.

### Key Steps
1. **Initialize BFS Queue**: Start BFS from each node since any node can hypothetically be the start of the path.
2. **Use Bitmasking**: Each state in the queue contains information of the current node and a bitmask of visited nodes.
3. **BFS Traversal**: For each node, explore all its neighbors and add those states that haven't been visited before in the current bitmask.
4. **End Condition**: If the bitmask indicates all nodes as visited, return the path length known.

### Code

```javascript
var shortestPathLength = function(graph) {
    const n = graph.length;
    
    // Queue for BFS: [currentNode, visitedMask, pathLength]
    let queue = [];
    let seen = new Array(n).fill(0).map(() => new Array(1 << n).fill(false));

    for(let i = 0; i < n; i++) {
        queue.push([i, 1 << i, 0]);  // starting at each node
        seen[i][1 << i] = true;      // mark the node and its bitmask as seen
    }
    
    const fullMask = (1 << n) - 1;  // This bitmask means all nodes visited
    
    while(queue.length > 0) {
        let [currentNode, visitedMask, pathLength] = queue.shift();

        // If all nodes are visited, return the current path length
        if(visitedMask === fullMask) {
            return pathLength;
        }

        // Explore neighbors
        for(let neighbor of graph[currentNode]) {
            let nextMask = visitedMask | (1 << neighbor);

            if(!seen[neighbor][nextMask]) {
                seen[neighbor][nextMask] = true;
                queue.push([neighbor, nextMask, pathLength + 1]);
            }
        }
    }
    
    return -1;  // No such path, should not hit this line
};
```

### Complexity Analysis
- **Time Complexity**: \( O(n \times 2^n) \), where \( n \) is the number of nodes. The BFS explores each state once. There are \( n \) nodes and \( 2^n \) possible visited states.
- **Space Complexity**: \( O(n \times 2^n) \) due to the queue and seen structure storing states for each node with each possible visited subset.

The BFS with bitmask approach effectively narrows down exploration to feasible states in polynomial time and space, making it an optimal solution given the problem constraints.

