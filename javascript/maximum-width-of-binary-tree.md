# [Maximum Width of Binary Tree - LeetCode Problem](https://leetcode.com/problems/maximum-width-of-binary-tree/)

## Approaches
1. [Level-order Traversal with Index](#level-order-traversal-with-index)
2. [Depth First Search with Position Tracking](#depth-first-search-with-position-tracking)

---

## 1. Level-order Traversal with Index

### Intuition:
The idea is to perform a level-order traversal (BFS) while keeping track of indices assigned to each node as if the tree were a "complete binary tree". The width of the tree at each level is determined by the difference between the largest and smallest indices at that level plus one.

### Approach:
- Use a queue to hold pairs of nodes and their respective indices.
- Traverse each level, maintaining minimum and maximum indices.
- Update the maximum width observed during the traversal.

### Code:
```javascript
function widthOfBinaryTree(root) {
    if (!root) return 0;

    let maxWidth = 0;
    const queue = [[root, 0]]; // Initializing queue with root node and its index

    while (queue.length > 0) {
        const levelLength = queue.length;
        let minIndex = queue[0][1]; // The first node's index in the current level
        let first = 0, last = 0; // To track the first and last indices at this level
        
        for (let i = 0; i < levelLength; ++i) {
            const [node, index] = queue.shift();
            const normalizedIndex = index - minIndex; // Normalize to avoid large numbers
            
            // Update first and last index at the level
            if (i === 0) first = normalizedIndex;
            last = normalizedIndex;
            
            // Add children to queue with calculated indices
            if (node.left) queue.push([node.left, 2 * normalizedIndex + 1]);
            if (node.right) queue.push([node.right, 2 * normalizedIndex + 2]);
        }
        maxWidth = Math.max(maxWidth, last - first + 1); // Update the maximum width
    }
    
    return maxWidth;
}
```

### Complexity:
- **Time Complexity**: \(O(n)\) where \(n\) is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: \(O(n)\) for the queue which in the worst case holds \(n\) elements (all nodes of a level).

---

## 2. Depth First Search with Position Tracking

### Intuition:
This approach employs DFS while using a map to track the first node index on each level. The width of the tree at each level is calculated using the position of the first node and the current node's position.

### Approach:
- Use a recursive DFS function with parameters for the node, its level, and its position.
- Maintain a map to note the first position for each level.
- Calculate width by comparing current node’s position with the first position stored in the map for that level.

### Code:
```javascript
function widthOfBinaryTree(root) {
    if (!root) return 0;

    let maxWidth = 0;
    const firstPositionMap = new Map(); // Map to track the first position of each level

    function dfs(node, level, position) {
        if (!node) return;

        // Record the first position at this depth if it does not exist
        if (!firstPositionMap.has(level)) {
            firstPositionMap.set(level, position);
        }

        // Calculate the width for the current level
        const firstPosition = firstPositionMap.get(level);
        maxWidth = Math.max(maxWidth, position - firstPosition + 1);

        // Recursive DFS on children with updated "position"
        dfs(node.left, level + 1, position * 2 + 1);
        dfs(node.right, level + 1, position * 2 + 2);
    }

    dfs(root, 0, 0); // Start DFS from the root at level 0 and position 0
    return maxWidth;
}
```

### Complexity:
- **Time Complexity**: \(O(n)\) because each node is processed once.
- **Space Complexity**: \(O(n)\) due to the recursion stack and map storage.

---

