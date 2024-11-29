# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the Map and recursion stack

class Node {
    constructor(val, neighbors = []) {
        this.val = val;
        this.neighbors = neighbors;
    }
}

class Solution {
    constructor() {
        this.map = new Map();
    }

    cloneGraph(node) {
        if (node === null) {
            return null;
        }

        // If the node is already cloned, return the cloned node
        if (this.map.has(node)) {
            return this.map.get(node);
        }

        // Clone the current node
        const clone = new Node(node.val);
        this.map.set(node, clone);

        // Recursively clone all neighbors
        for (let neighbor of node.neighbors) {
            clone.neighbors.push(this.cloneGraph(neighbor));
        }

        return clone;
    }
}
```

