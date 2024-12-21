# [LeetCode 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approaches
1. [Level Order Traversal (Breadth-First Search)](#approach-1-level-order-traversal)
2. [Depth-First Search with Depth Tracking](#approach-2-dfs-with-depth-tracking)
3. [Optimized BFS](#approach-3-optimized-bfs)

### Approach 1: Level Order Traversal

**Intuition**: The idea is to use the level order traversal technique (Breadth-First Search). For each level, we keep track of the last node encountered at that level because that's the node that would be visible from the right side.

- Perform level order traversal using a queue.
- At each level, add only the last node to the result as it represents the rightmost view of that level.

**Time Complexity**: O(N) where N is the number of nodes in the binary tree.
**Space Complexity**: O(W) where W is the maximum width of the binary tree, due to the queue.

```javascript
var rightSideView = function(root) {
    if (!root) return [];
    
    const result = [];
    const queue = [root];
    
    while (queue.length > 0) {
        let levelSize = queue.length;
        let lastNode = null;
        
        for (let i = 0; i < levelSize; i++) {
            // Extract the front node from the queue
            const current = queue.shift();
            lastNode = current; // Always update lastNode to the current node
            
            // Add their children to the queue for the next level
            if (current.left) queue.push(current.left);
            if (current.right) queue.push(current.right);
        }
        
        // Add the last node of the current level to the result
        result.push(lastNode.val);
    }
    
    return result;
};
```

---

### Approach 2: Depth-First Search with Depth Tracking

**Intuition**: Use a DFS approach to explore each node. As we recurse, we keep track of the current depth and update an array that stores the first node encountered at each depth level from the right side.

- Traverse the tree using DFS, prioritizing the right subtree first.
- Keep a record of each depth level's first seen node which would be the rightmost at that depth.

**Time Complexity**: O(N) where N is the number of nodes.
**Space Complexity**: O(H) where H is the height of the tree due to the call stack.

```javascript
var rightSideView = function(root) {
    const result = [];
    
    const dfs = (node, depth) => {
        if (!node) return;
        
        // If this is the first time we reached this depth, record it
        if (result.length === depth) {
            result.push(node.val);
        }
        
        // Prioritize the right side first
        dfs(node.right, depth + 1);
        dfs(node.left, depth + 1);
    };
    
    dfs(root, 0);
    return result;
};
```

---

### Approach 3: Optimized BFS

**Intuition**: This approach is a slight modification of the BFS approach. Instead of using an external queue management system, we directly modify the existing queue, making it more memory efficient with a focus on tracking just the nodes necessary for the right side view.

- Same BFS as the first approach but leverages array slice to manage the queue efficiently.

**Time Complexity**: O(N) where N is the number of nodes.
**Space Complexity**: O(W) where W is the maximum width of the tree.

```javascript
var rightSideView = function(root) {
    if (!root) return [];
    
    const result = [];
    let queue = [root];
    
    while (queue.length > 0) {
        const level = [];
        let levelSize = queue.length;

        while (levelSize > 0) {
            const node = queue.shift();
            level.push(node.val);
            
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
            
            levelSize--;
        }
        
        // Add the last element from the current level
        result.push(level[level.length - 1]);
    }
    
    return result;
};
```

Each approach provides a different perspective on achieving the task, with BFS methods using level-aware operations and DFS taking advantage of recursive depth tracking.

