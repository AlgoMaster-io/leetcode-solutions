# [133. Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches
1. [Recursive Depth-First Search (DFS)](#recursive-dfs)
2. [Iterative Breadth-First Search (BFS)](#iterative-bfs)

### Recursive DFS

The DFS approach involves copying each node recursively while traversing the graph in a Depth-First Search manner. This solution uses a dictionary to keep track of nodes that have already been cloned to avoid duplicates and infinite loops in case of cycles.

**Intuition:**
- If a node has already been cloned, retrieve the clone from the dictionary and return it.
- Otherwise, create a new node, add it to the dictionary and clone its neighbors recursively.

```csharp
// Definition for a Node.
public class Node {
    public int val;
    public IList<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new List<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new List<Node>();
    }
    public Node(int _val, List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}

public class Solution {
    // Dictionary to keep original nodes as keys and clones as values
    private Dictionary<Node, Node> visited = new Dictionary<Node, Node>();
    
    public Node CloneGraph(Node node) {
        if (node == null) return null; // If input node is null, return null

        // If this node has been cloned before, return its clone
        if (visited.ContainsKey(node)) {
            return visited[node];
        }
        
        // Clone the node
        Node cloneNode = new Node(node.val);
        visited[node] = cloneNode; // Mark this node as visited

        // Clone all the neighbors recursively
        foreach (var neighbor in node.neighbors) {
            cloneNode.neighbors.Add(CloneGraph(neighbor));
        }

        return cloneNode; // Return the clone of the graph
    }
}
```

- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. Each node and edge is visited once.
- **Space Complexity:** O(V), the space usage for the recursion stack and the dictionary storing the cloned nodes.

### Iterative BFS

In the BFS approach, we use a queue to traverse the graph level-by-level. We also use a dictionary to map original nodes to their clones to avoid duplication and handle cycles.

**Intuition:**
- Start from the node, clone it and push it onto the queue.
- While the queue is not empty, pop a node, and for each of its neighbors, if not visited, clone it and enqueue it.

```csharp
public class Solution {
    public Node CloneGraph(Node node) {
        if (node == null) return null; // If input node is null, return null

        // Dictionary to keep original nodes as keys and clones as values
        Dictionary<Node, Node> visited = new Dictionary<Node, Node>();
        
        // Queue for BFS
        Queue<Node> queue = new Queue<Node>();
        queue.Enqueue(node); // Enqueue the starting node

        // Clone the starting node
        visited[node] = new Node(node.val);

        // Perform BFS
        while (queue.Count > 0) {
            Node n = queue.Dequeue();
            foreach (Node neighbor in n.neighbors) {
                if (!visited.ContainsKey(neighbor)) {
                    // Clone the neighbor and add it to the queue
                    visited[neighbor] = new Node(neighbor.val);
                    queue.Enqueue(neighbor);
                }
                // Add the cloned neighbor to the current node's neighbors
                visited[n].neighbors.Add(visited[neighbor]);
            }
        }

        return visited[node]; // Return the clone of the graph
    }
}
```

- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. Each node and edge is visited once.
- **Space Complexity:** O(V), for the queue and the dictionary storing the cloned nodes.

