# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approaches
1. [Breadth-First Search (BFS) with State Compression](#approach-1-breadth-first-search-bfs-with-state-compression)

---

## Approach 1: Breadth-First Search (BFS) with State Compression

### Intuition
The problem is about finding the shortest path that visits all nodes in an undirected graph. This can naturally be modeled as a shortest path problem where each state in our BFS will not only track what node we are on but also what set of nodes we have visited.

To efficiently keep track of which nodes have been visited, we can use bitmasking. Represent visited nodes as a bitmask where the `i-th` bit is `1` if the node `i` is visited and `0` otherwise. The goal is to reach a state where all bits are `1`.

Our BFS queue will consist of `(current_node, visited_bitmask)`, and we will explore all possible paths while marking them as visited. This way, we avoid unnecessary revisiting of states that we've seen before with fewer steps.

### Algorithm
1. Initialize a queue with all nodes in the graph as starting points.
2. For each node, push the node and its corresponding bitmask visit representation into the queue.
3. Use a set to keep track of visited `(node, bitmask)` combinations to avoid revisits.
4. While the queue is not empty:
   - Dequeue a `(node, bitmask)`.
   - If the bitmask equals `(1 << n) - 1` (i.e., all nodes have been visited), return the number of steps taken.
   - Iterate over all neighbors of the current node, determine the new visited bitmask, and enqueue `(neighbor, new_bitmask)` if this state hasn't been visited.
5. Return an appropriate value if no solution is found (though per problem constraints, a solution always exists).

### Code

```go
func shortestPathLength(graph [][]int) int {
    // Number of nodes in the graph
    n := len(graph)
    // Goal is a bitmask where all nodes are visited
    finalState := (1 << n) - 1
    
    // Queue for BFS, containing tuples of the form (current_node, bitmask)
    type state struct {
        node, mask, dist int
    }
    queue := []state{}
    
    // Visited set to store (node, bitmask)
    visited := make(map[state]bool)
    
    // Initialize the queue with each node as a starting point
    for i := 0; i < n; i++ {
        initialState := state{i, (1 << i), 0}
        queue = append(queue, initialState)
        visited[initialState] = true
    }
    
    // BFS algorithm
    for len(queue) > 0 {
        currentState := queue[0]
        queue = queue[1:]
        currentNode, currentMask, currentDist := currentState.node, currentState.mask, currentState.dist
        
        // If we have visited all nodes, return the distance
        if currentMask == finalState {
            return currentDist
        }
        
        // Traverse all adjacent nodes
        for _, neighbor := range graph[currentNode] {
            // New mask with the neighbor node visited
            nextMask := currentMask | (1 << neighbor)
            newState := state{neighbor, nextMask, currentDist + 1}
            
            // If the new state has not been visited, enqueue it
            if !visited[newState] {
                queue = append(queue, newState)
                visited[newState] = true
            }
        }
    }
    
    // Should never be hit because there is always a path
    return -1
}
```

### Complexity Analysis

- **Time Complexity**: \(O(N \times 2^N)\), where \(N\) is the number of nodes. We have a bitmask for each node which can take \(2^N\) values, and we may need to explore each node for each possible combination of visited nodes.
- **Space Complexity**: \(O(N \times 2^N)\) for storing the visited states in the queue and set.

The BFS with state compression efficiently finds the shortest path visiting all nodes and captures the essence of the transition between different states with bitmasking agilely managing the state space.

