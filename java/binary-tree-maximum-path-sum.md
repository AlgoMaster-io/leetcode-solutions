# [Binary Tree Maximum Path Sum - LeetCode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approaches
1. [Recursive Brute Force](#approach-1-recursive-brute-force)
2. [Optimized Recursive with Global Maximum](#approach-2-optimized-recursive-with-global-maximum)

---

## Approach 1: Recursive Brute Force

### Intuition
The basic idea is to traverse each node of the binary tree and determine all possible paths that can be formed with this node as part of the path. Note that a path can start and terminate at any node, however, this approach might result in revisiting nodes multiple times causing inefficiency.

### Detailed Explanation
- We define a recursive function that will explore the different possible paths in the left and right subtrees.
- At each node, consider four possibilities:
  1. Node alone (no children).
  2. Node with its left subtree.
  3. Node with its right subtree.
  4. Node with both left and right subtree as the change in path.
- The maximum of these four possibilities gives the maximum path sum at that node.
- Keep track of the global maximum path sum encountered during the traversal.

### Code
```java
class Solution {
    // Brute force approach
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return Integer.MIN_VALUE;
        }
        // Calculate from the root
        int currentMax = findMaxPathThroughNode(root);
        // Determine the maximum path sum in both children separately
        int leftMax = maxPathSum(root.left);
        int rightMax = maxPathSum(root.right);
        // The result is the maximum among current node path, left subtree, and right subtree.
        return Math.max(currentMax, Math.max(leftMax, rightMax));
    }
    
    private int findMaxPathThroughNode(TreeNode node) {
        if (node == null) {
            return Integer.MIN_VALUE;
        }
        int left = Math.max(0, findMaxPathThroughNode(node.left)); // Max path sum on left
        int right = Math.max(0, findMaxPathThroughNode(node.right)); // Max path sum on right
        return node.val + left + right;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N^2) where N is the number of nodes because each node invocation leads to exploring its children in separate recursions.
- **Space Complexity**: O(H) where H is the height of the tree, representing the depth of the recursion stack.

---

## Approach 2: Optimized Recursive with Global Maximum

### Intuition
To optimize, we need to ensure that we only compute each subtree path sum once per node. This can be achieved by using a helper function that returns not only the maximum path sum that can include the ancestor of the current node, but also updates a global maximum variable which tracks the most optimal path seen so far.

### Detailed Explanation
- We use a helper function `maxGain` that computes the maximum gain from each node.
- Use a global variable `maxSum` to keep track of the maximum path sum encountered.
- If the contribution from a subtree is negative, it is better to avoid including that subtree.
- At each node, compute the max gain through the node which can be passed to its parent.
- This is done by taking the maximum of 0 and the gain from the left and right subtree, adding the current node's value.

### Code
```java
class Solution {
    private int maxSum = Integer.MIN_VALUE; // Global variable to track the maximum path sum

    public int maxPathSum(TreeNode root) {
        calculateMaxGain(root);
        return maxSum;
    }

    private int calculateMaxGain(TreeNode node) {
        if (node == null) {
            return 0;
        }

        // Find maximum path sum starting from the left and right child
        // If the path sum is negative, discard it by taking max with 0
        int leftGain = Math.max(calculateMaxGain(node.left), 0);
        int rightGain = Math.max(calculateMaxGain(node.right), 0);

        // Calculate the new possible maximum path sum with current node as the highest node
        int currentMaxPath = node.val + leftGain + rightGain;

        // Update the global maximum path sum
        maxSum = Math.max(maxSum, currentMaxPath);

        // Return the node's gain as part of the path; max path sum including this node
        return node.val + Math.max(leftGain, rightGain);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N) since each node is processed exactly once.
- **Space Complexity**: O(H) where H is the height of the tree for the recursion stack.

---

