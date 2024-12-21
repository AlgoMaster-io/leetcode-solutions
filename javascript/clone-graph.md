# [Leetcode 133: Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches

1. [DFS with HashMap](#dfs-with-hashmap)
2. [BFS with HashMap](#bfs-with-hashmap)

---

### Approach 1: DFS with HashMap

#### Intuition
To clone a graph, we need to ensure that each node is cloned exactly once and that the connections (edges) between nodes are duplicated correctly. Using a Depth-First Search (DFS) approach allows us to systematically explore each node and its neighbors, cloning them as we go. By leveraging a HashMap, we store each original node's clone, which helps in checking if a node is cloned, and provides quick access to the cloned node for connecting it with other nodes.

#### Steps
1. Define a recursive DFS function that takes a node and a HashMap.
2. If the node is null, return null to handle edge cases.
3. If the node is already cloned (exists in HashMap), return its clone.
4. Otherwise, create a clone of the node and store it in the HashMap.
5. Recursively clone each of the node's neighbors and add them to the clone's neighbor list.
6. Return the cloned node.

#### Time Complexity
- **O(N + M)**, where `N` is the number of nodes, and `M` is the number of edges. We visit each node once and iterate through all their neighbors.

#### Space Complexity
- **O(N)**, for the recursion stack and the space used by the HashMap.

```javascript
// Definition for a Node.
// function Node(val, neighbors) {
//     this.val = val === undefined ? 0 : val;
//     this.neighbors = neighbors === undefined ? [] : neighbors;
// }

function cloneGraph(node) {
    if (node === null) return null;

    const visited = new Map();

    function dfs(node) {
        // If already cloned, return the clone.
        if (visited.has(node)) {
            return visited.get(node);
        }

        // Clone the node.
        const clone = new Node(node.val);
        visited.set(node, clone);

        // Clone and connect all the neighbors.
        for (const neighbor of node.neighbors) {
            clone.neighbors.push(dfs(neighbor));
        }

        return clone;
    }

    return dfs(node);
}
```

---

### Approach 2: BFS with HashMap

#### Intuition
Using Breadth-First Search (BFS) offers an iterative way to traverse the graph level by level. This ensures we clone a node and then all its neighbors, without getting too deep into recursion which could be less efficient for very deep or large graphs. As we process each node, we keep track of the cloned nodes in a HashMap, similarly ensuring each node is cloned only once.

#### Steps
1. Handle the null node case immediately by returning null.
2. Initialize a queue with the starting node and a HashMap for cloned nodes.
3. Clone the starting node and add it to the HashMap.
4. While processing nodes from the queue:
   - Dequeue the front node.
   - For each unvisited neighbor, clone it, enqueue it, and connect it to the current node's clone.
5. Return the clone of the starting node.

#### Time Complexity
- **O(N + M)**, since we visit each node and edge as before.

#### Space Complexity
- **O(N)**, mainly due to the HashMap storage and queue size.

```javascript
function cloneGraph(node) {
    if (node === null) return null;

    const visited = new Map();
    const queue = [node];

    // Create a clone for the starting node.
    visited.set(node, new Node(node.val));

    while (queue.length > 0) {
        const n = queue.shift();

        // Process each neighbor.
        for (const neighbor of n.neighbors) {
            if (!visited.has(neighbor)) {
                // Clone the neighbor and add to the queue if not visited.
                visited.set(neighbor, new Node(neighbor.val));
                queue.push(neighbor);
            }

            // Connect the cloned node with the cloned neighbor.
            visited.get(n).neighbors.push(visited.get(neighbor));
        }
    }

    return visited.get(node);
}
```
With these two approaches, you can easily clone any given graph using either DFS or BFS strategy, taking appropriate care of the nodes and their interconnected nature through a HashMap.

