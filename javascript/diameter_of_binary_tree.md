# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
    constructor() {
        this.diameter = 0;
    }

    diameterOfBinaryTree(root) {
        this.depth(root);
        return this.diameter;
    }

    depth(node) {
        if (node === null) {
            return 0; // Base case: null node has depth 0
        }

        const leftDepth = this.depth(node.left);  // Calculate depth of left subtree
        const rightDepth = this.depth(node.right); // Calculate depth of right subtree

        // Update the diameter (maximum path length between two nodes)
        this.diameter = Math.max(this.diameter, leftDepth + rightDepth);

        // Return the depth of the current subtree
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
    constructor() {
        this.maxDiameter = 0;
    }

    diameterOfBinaryTree(root) {
        this.calculateDepth(root);
        return this.maxDiameter;
    }

    calculateDepth(node) {
        if (node === null) {
            return 0; // Null nodes contribute 0 to depth
        }

        const left = this.calculateDepth(node.left);  // Left subtree depth
        const right = this.calculateDepth(node.right); // Right subtree depth

        this.maxDiameter = Math.max(this.maxDiameter, left + right); // Update the max diameter

        return Math.max(left, right) + 1; // Return the current depth
    }
}
```

## Approach 3: Iterative DFS Using Stack

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
class Solution {
    diameterOfBinaryTree(root) {
        if (root === null) {
            return 0; // Edge case: empty tree
        }

        const stack = [];
        const depthMap = new Map();
        let maxDiameter = 0;

        stack.push(root);
        while (stack.length > 0) {
            const current = stack[stack.length - 1];
            if (current === null) {
                stack.pop();
                continue;
            }

            if ((current.left !== null && !depthMap.has(current.left)) || 
                (current.right !== null && !depthMap.has(current.right))) {
                if (current.left !== null) stack.push(current.left);
                if (current.right !== null) stack.push(current.right);
            } else {
                stack.pop();
                const left = depthMap.get(current.left) || 0;
                const right = depthMap.get(current.right) || 0;
                maxDiameter = Math.max(maxDiameter, left + right);
                depthMap.set(current, Math.max(left, right) + 1);
            }
        }

        return maxDiameter;
    }
}
```

