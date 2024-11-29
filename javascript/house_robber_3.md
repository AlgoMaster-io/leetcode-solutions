# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    constructor() {
        this.memo = new Map();
    }

    rob(root) {
        if (root === null) {
            return 0;
        }
        if (this.memo.has(root)) {
            return this.memo.get(root);
        }

        // if we rob this root, we cannot rob its direct children
        let robRoot = root.val;
        if (root.left !== null) {
            robRoot += this.rob(root.left.left) + this.rob(root.left.right);
        }
        if (root.right !== null) {
            robRoot += this.rob(root.right.left) + this.rob(root.right.right);
        }

        // if we do not rob this root, we are free to rob its children
        const skipRoot = this.rob(root.left) + this.rob(root.right);

        // Choose the maximum money we can rob
        const result = Math.max(robRoot, skipRoot);
        this.memo.set(root, result); // Memoize the result for current node

        return result;
    }
}
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(h), where h is the height of the tree
class Solution {
    rob(root) {
        const result = this.robSub(root);
        return Math.max(result[0], result[1]);
    }

    robSub(root) {
        if (root === null) {
            return [0, 0]; // Entry for [not rob, rob]
        }

        const left = this.robSub(root.left);
        const right = this.robSub(root.right);

        // if we don't rob this node, we can choose to rob or not to rob its children
        const notRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

        // if we rob this node, we cannot rob its children
        const rob = root.val + left[0] + right[0];

        return [notRob, rob]; // Return array containing [not rob, rob]
    }
}
```

