# [Leetcode 979: Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Approaches
1. [Depth First Search with Excess Calculation](#depth-first-search-with-excess-calculation)

## Depth First Search with Excess Calculation

**Intuition:**

The problem involves distributing coins in order to reach a state where each node in the binary tree (including the root) holds exactly one coin. If a node has more than one coin, it can give some of them to adjacent nodes. If a node lacks a coin, it can request one from its adjacent nodes.

You'll need to use Depth First Search (DFS) to explore this tree, using the property that you can calculate how many moves you need by considering the number of excess or deficit coins each node must balance with its parent.

- Each node's "excess" is calculated by the number of coins present minus 1 (since one is needed to stay at the node).
- This excess or deficit is propagated upwards as you return from recursive DFS calls, making it possible to compute the number of moves required at each step.

Here's how you can implement this:

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int moves;

    public int distributeCoins(TreeNode root) {
        moves = 0;
        dfs(root);
        return moves;
    }

    private int dfs(TreeNode node) {
        if (node == null) return 0;

        // Perform postorder traversal: first process left subtree, then right subtree
        int leftExcess = dfs(node.left);
        int rightExcess = dfs(node.right);

        // Calculate the total excess coins at the current node
        int excess = node.val + leftExcess + rightExcess - 1;

        // Accumulate the number of moves needed: absolute value of excess at left and right children
        moves += Math.abs(leftExcess) + Math.abs(rightExcess);

        // Return the excess at this node to the parent call
        return excess;
    }
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree. The DFS touches each node exactly once.

**Space Complexity:** O(H), where H is the height of the tree. The space is used by the recursion stack. In the worst case (unbalanced tree), H could be as large as N. In the best case (balanced tree), H is log(N).

