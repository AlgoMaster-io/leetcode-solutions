# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approaches:
- [Approach 1: DFS with Parent Pointers and BFS](#approach-1-dfs-with-parent-pointers-and-bfs)

---

## Approach 1: DFS with Parent Pointers and BFS

### Intuition
The problem requires us to find all nodes that are exactly K distance away from a given target node in a binary tree. The best way to achieve this is by traversing the tree in a way that allows us to find all possible paths leading to nodes at distance K. 

First, we'll annotate each node with a pointer to its parent using a DFS traversal. This allows us to traverse not only downwards but also upwards from any node. Once we have a pointer to each node's parent, we can perform a BFS starting from the target node to find all nodes that are K distance away, either by going up, left or right.

### Steps:
1. **Annotate Parent Pointers with DFS:**
   - Traverse the tree using DFS and store parent pointers for every node in a hashmap.
2. **BFS from Target Node:**
   - Use a breadth-first search starting from the target node while keeping track of visited nodes to avoid cycles.
   - Continue BFS until the target distance K is reached and collect all current nodes.

### Detailed Code

```javascript
// Definition for a binary tree node.
function TreeNode(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
}

const distanceK = function(root, target, K) {
    if (!root) return [];

    const parentMap = new Map();
    
    // Step 1: DFS to record all parent pointers
    const dfs = (node, parent = null) => {
        if (!node) return;
        parentMap.set(node, parent);
        dfs(node.left, node);
        dfs(node.right, node);
    };
    
    dfs(root);

    // Step 2: BFS to find all nodes that are distance K from target
    const result = [];
    const queue = [target];
    const visited = new Set();
    visited.add(target);
    
    let distance = 0;
    
    while (queue.length > 0) {
        const levelSize = queue.length;
        
        if (distance === K) {
            for (let i = 0; i < levelSize; i++) {
                result.push(queue[i].val);
            }
            return result;
        }
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();

            // Visit children nodes
            if (node.left && !visited.has(node.left)) {
                visited.add(node.left);
                queue.push(node.left);
            }
        
            if (node.right && !visited.has(node.right)) {
                visited.add(node.right);
                queue.push(node.right);
            }
        
            // Visit parent node
            const parent = parentMap.get(node);
            if (parent && !visited.has(parent)) {
                visited.add(parent);
                queue.push(parent);
            }
        }
        
        distance++;
    }
    
    return result;
};
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We are doing a DFS traversal to annotate parent pointers and a BFS traversal with potentially N nodes.
- **Space Complexity:** O(N), due to storage of the parentMap and the queue used in the BFS.

This solution effectively uses both DFS and BFS to address the problem efficiently, allowing bidirectional movement from the target using parent pointers, which is crucial for finding distance K in a binary tree.

