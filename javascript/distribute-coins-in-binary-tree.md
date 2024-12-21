# [Leetcode 979: Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Table of Contents
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Optimized DFS with Intuition](#approach-2-optimized-dfs-with-intuition)

---

## Approach 1: Depth First Search (DFS)

### Intuition
The goal is to ensure each node in the binary tree has exactly one coin while minimizing the moves required to achieve this state. We need to distribute or collect coins within the tree, which involves calculating the balance of coins needed or excess at each node.

The basic DFS approach involves traversing each node, calculating the balance of coins for each node (coins present - 1) and accumulating the absolute value of these balances (which represent moves) to achieve the balance state by considering the subtree.

### Implementation

```javascript
function distributeCoins(root) {
    let moves = 0;

    // Helper DFS function that processes each node
    const dfs = (node) => {
        if (node === null) return 0;

        // Calculate the balance of coins for left and right subtree
        let leftBalance = dfs(node.left);
        let rightBalance = dfs(node.right);

        // Add the absolute balance amounts of left and right to moves
        moves += Math.abs(leftBalance) + Math.abs(rightBalance);

        // Return the balance for the current node's subtree
        return node.val + leftBalance + rightBalance - 1;
    };

    // Start DFS from the root
    dfs(root);

    return moves;
}
```

### Time Complexity
- **O(N)**, where N is the number of nodes in the tree. We visit each node exactly once.

### Space Complexity
- **O(H)**, where H is the height of the tree, due to the recursion stack. In the worst case (skewed tree), this can be O(N), but typically O(log N) for balanced trees.

---

## Approach 2: Optimized DFS with Intuition

### Intuition
The DFS approach already presents an optimal solution because it effectively calculates the balance of coins needed at each node by visiting each subtree only once and using the properties of binary trees. An optimized variant would ensure we minimize extra variable storage, focusing on readability and making efficient use of the recursive calls by embedding all logic into return statements.

### Enhanced Explanation
Note that each subtree handles its internal "balance" first, and by virtue of post-order DFS, higher nodes can depend on their children to already be balanced or have clear needs, meaning:
- If a subtree is in excess or deficit, we adjust the main count accordingly by returning an accumulated balance.
- The total moves needed are simply the sum of absolute values of adjustments as they bubble up from leaves to root.

### Implementation

```javascript
function distributeCoinsOptimized(root) {
    let moves = 0;

    // DFS function with inlined operations for clarity
    const dfs = (node) => {
        if (!node) return 0;  // Base case: If node is null, no moves required and it's balanced
        
        // Calculate balances of left and right subtrees
        const leftBalance = dfs(node.left);
        const rightBalance = dfs(node.right);

        // Calculate the moves required
        moves += Math.abs(leftBalance) + Math.abs(rightBalance);

        // Coins to return: current node's coins + left and right balances - 1 (for the node itself)
        return node.val + leftBalance + rightBalance - 1;
    };

    dfs(root);

    return moves;
}
```

### Time Complexity
- **O(N)**: Same rationale as before; each node is processed once.

### Space Complexity
- **O(H)**: Again, we consider the recursion depth due to the call stack, where H is the tree height.

In both approaches, the DFS strategy inherently optimizes our paths and balance needs per node, using post-order traversal effectively to ensure an optimal result with minimal moves.

