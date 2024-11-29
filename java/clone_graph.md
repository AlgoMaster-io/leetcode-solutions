# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the HashMap and recursion stack
import java.util.HashMap;

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

public class Solution {
    private HashMap<Node, Node> map = new HashMap<>();

    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }

        // If the node is already cloned, return the cloned node
        if (map.containsKey(node)) {
            return map.get(node);
        }

        // Clone the current node
        Node clone = new Node(node.val);
        map.put(node, clone);

        // Recursively clone all neighbors
        for (Node neighbor : node.neighbors) {
            clone.neighbors.add(cloneGraph(neighbor));
        }

        return clone;
    }
}
```