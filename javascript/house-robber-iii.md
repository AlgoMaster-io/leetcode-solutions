# [Leetcode 337: House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Recursive with Memoization](#recursive-with-memoization)
3. [Optimized Tree DP](#optimized-tree-dp)

---

### Recursive Approach

**Intuition:**

The problem follows a recursive strategy similar to the House Robber problem on a linear street, but here we need to account for the binary tree structure, where no two directly connected nodes can be robbed simultaneously. At each node, we decide whether to rob it or not based on the maximum money we can collect.

- If we rob the current node, we cannot rob its children.
- If we do not rob the current node, we take the maximum of the results produced by robbing or not robbing each of its children.

The recursive base case is when we reach a leaf node, where we return 0 since there are no houses to rob.

```javascript
function rob(root) {
    function robSubtree(node) {
        // If the node is null, return 0 since no money can be robbed
        if (!node) return 0;

        // Option 1: Rob the current node and skip its children
        let moneyIfRobbed = node.val;
        if (node.left) {
            moneyIfRobbed += robSubtree(node.left.left) + robSubtree(node.left.right);
        }
        if (node.right) {
            moneyIfRobbed += robSubtree(node.right.left) + robSubtree(node.right.right);
        }

        // Option 2: Do not rob the current node and perform the regular recursion on the children
        let moneyIfNotRobbed = robSubtree(node.left) + robSubtree(node.right);

        // Return the maximum money possible by robbing or not robbing
        return Math.max(moneyIfRobbed, moneyIfNotRobbed);
    }

    return robSubtree(root);
}
```

- **Time Complexity:** O(2^n) in the worst case since each node can have two possibilities (robbed or not).
- **Space Complexity:** O(n) due to the recursion stack.

### Recursive with Memoization

**Intuition:**

To optimize the recursive solution, we store results of subproblems in a map to avoid redundant calculations. By remembering the maximum amount for each node, subproblems are not recalculated.

```javascript
function rob(root) {
    const memo = new Map();

    function robSubtree(node) {
        if (!node) return 0;
        if (memo.has(node)) return memo.get(node);

        let moneyIfRobbed = node.val;
        if (node.left) {
            moneyIfRobbed += robSubtree(node.left.left) + robSubtree(node.left.right);
        }
        if (node.right) {
            moneyIfRobbed += robSubtree(node.right.left) + robSubtree(node.right.right);
        }

        const moneyIfNotRobbed = robSubtree(node.left) + robSubtree(node.right);

        const result = Math.max(moneyIfRobbed, moneyIfNotRobbed);
        memo.set(node, result);
        return result;
    }

    return robSubtree(root);
}
```

- **Time Complexity:** O(n) where n is the number of nodes since we're memoizing the results.
- **Space Complexity:** O(n) for the memoization map and recursion stack.

### Optimized Tree DP

**Intuition:**

We use a tree dynamic programming approach where each node returns two values:
- Maximum money if this node is not robbed.
- Maximum money if this node is robbed.

This makes the computation straightforward in O(n) time since each node is evaluated once.

```javascript
function rob(root) {
    function robSubtree(node) {
        if (!node) return [0, 0];

        // Post-order traversal: process left and right children
        const left = robSubtree(node.left);
        const right = robSubtree(node.right);

        // If the current node is not robbed, take the max of robbing or not robbing the children
        const notRobbed = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

        // If the current node is robbed, we cannot rob the children
        const robbed = node.val + left[0] + right[0];

        return [notRobbed, robbed];
    }

    const result = robSubtree(root);
    return Math.max(result[0], result[1]);
}
```

- **Time Complexity:** O(n) where n is the number of nodes, since we process each node exactly once.
- **Space Complexity:** O(h) where h is the height of the tree, required for the recursion stack.

This approach ensures the most efficient solution by avoiding unnecessary computations and directly considering the possibilities of robbing or skipping each node.

