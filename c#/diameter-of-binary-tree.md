## [Diameter of Binary Tree - LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/)

### Approaches:
1. [Naive Approach (Calculate Height of Subtrees)](#naive-approach)
2. [Optimized Approach (Single Pass)](#optimized-approach)

---

### Naive Approach

The naive approach involves calculating the height of the left and right subtrees for each node and using this information to compute the diameter. For each node, the diameter is the sum of the heights of its left and right subtrees. This is done recursively for every node in the tree.

**Intuition:**
- For each node, the potential diameter that passes through it is the height of its left subtree plus the height of its right subtree.
- By calculating the potential diameter at each node, we can update a global maximum to find the largest possible diameter.

#### Code:

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Solution {
    private int maxDiameter = 0;

    public int DiameterOfBinaryTree(TreeNode root) {
        CalculateHeight(root);
        return maxDiameter;
    }

    private int CalculateHeight(TreeNode node) {
        if (node == null) 
            return 0;

        // Calculate height of left and right subtrees
        int leftHeight = CalculateHeight(node.left);
        int rightHeight = CalculateHeight(node.right);

        // Potential diameter through this node
        int diameterThroughNode = leftHeight + rightHeight;

        // Update global maximum diameter
        if (diameterThroughNode > maxDiameter) {
            maxDiameter = diameterThroughNode;
        }

        // Return height of the tree from the current node
        return 1 + Math.Max(leftHeight, rightHeight);
    }
}
```

**Time Complexity:** O(N^2) - For each node, computing the height function takes O(N) in the worst-case scenario since the height function is called repeatedly for each node during the calculation.

**Space Complexity:** O(N) - Due to recursion stack.

---

### Optimized Approach

In this optimized approach, we aim to reduce unnecessary calculations by computing the height of subtrees and diameter in a single traversal.

**Intuition:**
- By combining the calculation of subtree height and diameter into a single recursive function, we can save time by eliminating repeated calculations.

#### Code:

```csharp
public class Solution {
    private int maxDiameter = 0;

    public int DiameterOfBinaryTree(TreeNode root) {
        CalculateHeightAndUpdateDiameter(root);
        return maxDiameter;
    }

    private int CalculateHeightAndUpdateDiameter(TreeNode node) {
        if (node == null) 
            return 0;

        // Recur for left and right subtrees
        int leftHeight = CalculateHeightAndUpdateDiameter(node.left);
        int rightHeight = CalculateHeightAndUpdateDiameter(node.right);

        // Calculate diameter passing through this node
        int diameterThroughNode = leftHeight + rightHeight;

        // Update the maximum diameter
        maxDiameter = Math.Max(maxDiameter, diameterThroughNode);

        // Return height of the node
        return 1 + Math.Max(leftHeight, rightHeight);
    }
}
```

**Time Complexity:** O(N) - Each node is visited once.

**Space Complexity:** O(N) - Due to recursion stack.

In this optimized solution, by simplifying the process into a single traversal, we achieve better performance, especially for larger trees.

