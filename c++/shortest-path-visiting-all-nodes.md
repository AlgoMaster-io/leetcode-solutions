## [Leetcode Problem 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

### Approaches
- [Breadth-First Search (BFS) Approach](#bfs-approach)

---

### BFS Approach

**Intuition:**
To find the shortest path that visits all nodes in an undirected graph, we can utilize a Breadth-First Search (BFS), as BFS is well-suited to find the shortest path in an unweighted graph. The challenge here is the requirement to visit all nodes at least once, a problem which naturally translates to finding the shortest Hamiltonian path.

We can use a state represented by both `node` and a `bitmask` indicating visited nodes:
- Each position in the bitmask is toggled when a node is visited.
- BFS will explore each possible path and stop once all nodes are visited (all bits set in the bitmask).

**Algorithm:**
1. Use a queue to keep track of nodes along with their visiting states encoded as a bitmask.
2. Start BFS from each node while treating each starting node as initial discovery of a new state.
3. Expand each node's neighbors while updating the visiting state.
4. If a state where all nodes are visited is found, return the number of steps taken.

**Implementation:**

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <tuple>
#include <unordered_set>

int shortestPathLength(std::vector<std::vector<int>>& graph) {
    int n = graph.size();
    // End state is when all nodes are visited.
    int endState = (1 << n) - 1;
    
    std::queue<std::tuple<int, int, int>> q; // (node, mask, dist)
    std::unordered_set<std::tuple<int, int>> visited; // (node, mask)
    
    // Initialize queue: start BFS from each node
    for (int i = 0; i < n; ++i) {
        int mask = (1 << i); // Initial bitmask for each starting node
        q.push({i, mask, 0});
        visited.insert({i, mask});
    }
    
    // Perform BFS
    while (!q.empty()) {
        auto [node, mask, dist] = q.front();
        q.pop();
        
        // If the mask indicates all nodes are visited, return the distance.
        if (mask == endState) {
            return dist;
        }
        
        // Visit neighbors
        for (int neighbor : graph[node]) {
            int nextMask = mask | (1 << neighbor);
            if (visited.find({neighbor, nextMask}) == visited.end()) {
                q.push({neighbor, nextMask, dist + 1});
                visited.insert({neighbor, nextMask});
            }
        }
    }
    
    return -1; // In case of unexpected input that prevents reaching the goal
}

// Time Complexity: O(n * 2^n), where n is the number of nodes in the graph.
// Space Complexity: O(n * 2^n), for the queue and the visited set.
```

**Explanation:**
- We start the BFS from each node because the path isn't predetermined to start from a specific node.
- We keep track of visited nodes using a bitmask which uniquely identifies each state by setting bits on visited nodes.
- `q` holds the current state signified by `(node, mask, dist)` which includes the current node, bitmask of nodes visited, and the current path length.
- The BFS proceeds to explore each graph node's neighbors, updating the bitmask each time.
- When a bitmask representing all nodes as visited is achieved, we return the step count, which is the shortest path to visit all nodes.

This BFS approach efficiently navigates and checks all potential paths due to its natural layer-by-layer exploration and state tracking, ensuring that the first completion state found is the shortest path.

