# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Approaches
- [Approach 1: Depth-First Search with Negative Balance](#approach-1-depth-first-search-with-negative-balance)

## Approach 1: Depth-First Search with Negative Balance

### Intuition
The goal of this problem is to make sure that each node of a binary tree has exactly one coin. If a node has more than one coin, the excess coins should be moved to other nodes that need coins. Movements to neighboring nodes are counted as one move.

To solve this efficiently, we'll traverse the tree using a depth-first search (DFS). For each node, we'll calculate a "balance", which represents the number of excess coins after accounting for the presence of the node itself.

- If a node has 0 coins, it should "request" a coin (-1 balance).
- If it has 1 coin, it is balanced (0 balance).
- If it has more than 1 coin, it can "export" the extra coins (positive balance).

The balance that's carried to the parent would be the sum of its left and right subtrees and its own balance. The total number of moves required is the sum of the absolute values of balances required by each node, as each balance represents an excess or deficit that needs correcting in the binary tree.

### Time Complexity
- O(N), where N is the number of nodes in the tree. Each node is visited once.

### Space Complexity
- O(H), where H is the height of the tree, due to the recursive call stack (O(log N) for balanced trees and O(N) for completely unbalanced trees).

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val)
        this.left = (left === undefined ? null : left)
        this.right = (right === undefined ? null : right)
    }
}

function distributeCoins(root: TreeNode | null): number {
    let totalMoves = 0;

    function dfs(node: TreeNode | null): number {
        // Base case: If the node does not exist, return 0 (no balance)
        if (!node) return 0;

        // Calculate balance for left and right children
        let leftBalance = dfs(node.left);
        let rightBalance = dfs(node.right);

        // Total moves needed include the imbalance from the left and right
        totalMoves += Math.abs(leftBalance) + Math.abs(rightBalance);

        // Calculate this node's balance: total coins - 1 (necessary for the node itself)
        return node.val + leftBalance + rightBalance - 1;
    }

    dfs(root);

    return totalMoves;
}
```

This approach efficiently computes the required number of moves to balance the coins across nodes in a binary tree by leveraging the negative balance concept via DFS. This ensures that the number of operations is minimized by resolving local imbalances before propagating the need across levels.

