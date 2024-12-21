# [Leetcode 133: Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches
- [Approach 1: Depth-First Search (DFS) with Recursion](#approach-1-depth-first-search-dfs-with-recursion)
- [Approach 2: Breadth-First Search (BFS) with Iteration](#approach-2-breadth-first-search-bfs-with-iteration)

## Approach 1: Depth-First Search (DFS) with Recursion

### Intuition
The DFS approach involves recursively creating clone nodes and copying the connections (neighbors) in a depth-first manner. We use a hashmap to store the node's original as a key and its clone as a value to avoid creating multiple copies of the same node.

### Steps
1. A base case checks if the input node is `null`, returning `null` if true.
2. If the node has already been cloned (exists in the hashmap), return the cloned node.
3. Initiate a clone node and store it in the hashmap.
4. Recursively visit each neighbor of the node and add the cloned neighbors to the clone node's neighbors list.
5. Return the cloned node.

### Time and Space Complexity
- **Time Complexity**: O(N), where N is the number of nodes. Each node and edge is traversed once.
- **Space Complexity**: O(N) due to the recursion stack and hashmap storage of the nodes.

### Code
```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}

public class CloneGraph {
    private Map<Node, Node> visited = new HashMap<>();
    
    public Node cloneGraph(Node node) {
        // Base case
        if (node == null) {
            return null;
        }

        // If the node was already visited, return the clone from the hashmap
        if (visited.containsKey(node)) {
            return visited.get(node);
        }

        // Create a clone for the node
        Node cloneNode = new Node(node.val);
        visited.put(node, cloneNode);

        // Iterate over the neighbors to populate the cloned graph
        for (Node neighbor : node.neighbors) {
            cloneNode.neighbors.add(cloneGraph(neighbor));
        }
        return cloneNode;
    }
}
```

## Approach 2: Breadth-First Search (BFS) with Iteration

### Intuition
This approach consists of a breadth-first traversal using a queue. It uses a hashmap to store cloned nodes and iteratively connects neighbors by dequeuing nodes and exploring their neighbors.

### Steps
1. Handle the base case where input node is `null` by returning `null`.
2. Use a hashmap to store visited and cloned nodes.
3. Initialize a queue with the original node and clone the root node.
4. While the queue is not empty, process the node:
   - For each neighbor, if not cloned, clone and enqueue it.
   - Update the current node's clone to include the clone of its neighbors.
5. Once traversal ends, return the clone of the original node.

### Time and Space Complexity
- **Time Complexity**: O(N), where N is the number of nodes. Each node and edge is traversed once.
- **Space Complexity**: O(N) for the hashmap and queue.

### Code
```java
import java.util.*;

// Definition for a Node remains the same as above.
public class CloneGraph {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }

        // Map to store the visited node and its respective clone
        Map<Node, Node> visited = new HashMap<>();

        // Put the first node in the queue
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);

        // Clone the node and put it in the visited map
        visited.put(node, new Node(node.val));

        // Start BFS traversal
        while (!queue.isEmpty()) {
            // Extract a node from the front of the queue
            Node n = queue.poll();

            // Iterate through its neighbors
            for (Node neighbor : n.neighbors) {
                if (!visited.containsKey(neighbor)) {
                    // Clone the neighbor and add it to the visited map
                    visited.put(neighbor, new Node(neighbor.val));
                    // Add the newly encountered node to the queue
                    queue.add(neighbor);
                }
                // Add the cloned neighbor to the current node's neighbor list
                visited.get(n).neighbors.add(visited.get(neighbor));
            }
        }
        // Return the clone of the starting node
        return visited.get(node);
    }
}
```

These approaches effectively demonstrate different traversal techniques using DFS and BFS in graph cloning, ensuring each node and its connections are accurately replicated.

