# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Approaches:

1. [Brute Force](#approach-1-brute-force)
2. [Optimized DFS](#approach-2-optimized-dfs)

---

### Approach 1: Brute Force

**Intuition:**

In this approach, the idea is to recursively look at every subtree and calculate not just the distribution of coins, but also ensure each node ends up with exactly one coin. The recursive structure involves calculating excess coins at each level and passing excess or shortage up to parent nodes.

**Steps:**

1. Traverse the tree using post-order traversal (left, right, root).
2. For each node, calculate excess or shortage of coins: `excess = node->val + left excess + right excess - 1`,
3. Track total moves required per node as `abs(left excess) + abs(right excess)`.
4. Return this excess to the parent to be adjusted there.

This brute-force approach isn't the most efficient due to the repeated recalculations and potential update steps that are more iterative than necessary.

**Complexity Analysis:**

- **Time Complexity:** O(N^2) (`N` being the number of nodes), because for each node we perform recursive calculations that often revisit each subtree multiple times.
- **Space Complexity:** O(H) where `H` is the height of the tree, owing to the recursive call stack.

```cpp
// Brute Force Approach
class Solution {
public:
    int distributeCoins(TreeNode* root) {
        if (!root) return 0;

        // Total moves initialized as zero
        int totalMoves = 0;

        // Helper function to compute excess and increment total moves
        function<int(TreeNode*)> calculateMoves = [&](TreeNode* node) -> int {
            if (!node) return 0;
            
            // Compute excess on the left and right
            int leftExcess = calculateMoves(node->left);
            int rightExcess = calculateMoves(node->right);
            
            // Update moves with absolute excess of children
            totalMoves += abs(leftExcess) + abs(rightExcess);
            
            // Return the total excess from this node
            return node->val + leftExcess + rightExcess - 1;
        };

        calculateMoves(root);
        return totalMoves;
    }
};
```

---

### Approach 2: Optimized DFS

**Intuition:**

This approach uses a more efficient DFS that calculates two key properties in one traversal: the number of moves required and the net excess of coins. By handling both aspects in one recursive function, it becomes possible to avoid redundant traversals of the tree.

**Steps:**

1. Use a post-order DFS function that:
   - Calculates the net excess or shortage of coins at current node: `excess = node->val + excess_left + excess_right - 1`.
   - Accumulates moves required within the same DFS.
2. Directly traverse and compute from the bottom of the tree towards the root ensuring each subtree follows the constraints.

This approach minimizes redundant calculations and consolidates all distribution logic within a single recursive function, an optimization over simply being iterative in calculating moves.

**Complexity Analysis:**

- **Time Complexity:** O(N). Each node is visited exactly once.
- **Space Complexity:** O(H), which is the height of the tree reflecting the recursive call stack space.

```cpp
// Optimized DFS Approach
class Solution {
public:
    int totalMoves = 0;

    int distributeCoins(TreeNode* root) {
        distribute(root);
        return totalMoves;
    }

    int distribute(TreeNode* node) {
        if (!node) return 0;

        // Compute excess coins for left and right children
        int leftExcess = distribute(node->left);
        int rightExcess = distribute(node->right);
        
        // Update total moves with absolute values of excess
        totalMoves += abs(leftExcess) + abs(rightExcess);
        
        // Current node's excess = node's coins + excess from children - 1
        return node->val + leftExcess + rightExcess - 1;
    }
};
```

---

