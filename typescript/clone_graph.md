# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the HashMap and recursion stack

class Node {
    val: number;
    neighbors: Node[];

    constructor(val?: number, neighbors?: Node[]) {
        this.val = val === undefined ? 0 : val;
        this.neighbors = neighbors === undefined ? [] : neighbors;
    }
}

class Solution {
    private map: Map<Node, Node> = new Map();

    public cloneGraph(node: Node | null): Node | null {
        if (node === null) {
            return null;
        }

        // If the node is already cloned, return the cloned node
        if (this.map.has(node)) {
            return this.map.get(node)!;
        }

        // Clone the current node
        const clone = new Node(node.val);
        this.map.set(node, clone);

        // Recursively clone all neighbors
        for (const neighbor of node.neighbors) {
            clone.neighbors.push(this.cloneGraph(neighbor)!);
        }

        return clone;
    }
}
```

