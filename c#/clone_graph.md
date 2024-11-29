# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with Dictionary

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the Dictionary and recursion stack
using System.Collections.Generic;

class Node {
    public int val;
    public List<Node> neighbors;
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
    private Dictionary<Node, Node> map = new Dictionary<Node, Node>();

    public Node CloneGraph(Node node) {
        if (node == null) {
            return null;
        }

        // If the node is already cloned, return the cloned node
        if (map.ContainsKey(node)) {
            return map[node];
        }

        // Clone the current node
        Node clone = new Node(node.val);
        map[node] = clone;

        // Recursively clone all neighbors
        foreach (Node neighbor in node.neighbors) {
            clone.neighbors.Add(CloneGraph(neighbor));
        }

        return clone;
    }
}
```

