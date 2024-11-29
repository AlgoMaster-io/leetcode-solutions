# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    private int diameter = 0;

    public int DiameterOfBinaryTree(TreeNode root) {
        Depth(root);
        return diameter;
    }

    private int Depth(TreeNode node) {
        if (node == null) {
            return 0; // Base case: null node has depth 0
        }

        int leftDepth = Depth(node.left);  // Calculate depth of left subtree
        int rightDepth = Depth(node.right); // Calculate depth of right subtree

        // Update the diameter (maximum path length between two nodes)
        diameter = Math.Max(diameter, leftDepth + rightDepth);

        // Return the depth of the current subtree
        return Math.Max(leftDepth, rightDepth) + 1;
    }
}
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    private int maxDiameter = 0;

    public int DiameterOfBinaryTree(TreeNode root) {
        CalculateDepth(root);
        return maxDiameter;
    }

    private int CalculateDepth(TreeNode node) {
        if (node == null) {
            return 0; // Null nodes contribute 0 to depth
        }

        int left = CalculateDepth(node.left);  // Left subtree depth
        int right = CalculateDepth(node.right); // Right subtree depth

        maxDiameter = Math.Max(maxDiameter, left + right); // Update the max diameter

        return Math.Max(left, right) + 1; // Return the current depth
    }
}
```

## Approach 3: Iterative DFS Using Stack

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public int DiameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0; // Edge case: empty tree
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();
        Dictionary<TreeNode, int> depthMap = new Dictionary<TreeNode, int>();
        int maxDiameter = 0;

        stack.Push(root);
        while (stack.Count > 0) {
            TreeNode current = stack.Peek();
            if (current == null) {
                stack.Pop();
                continue;
            }

            if ((current.left != null && !depthMap.ContainsKey(current.left)) || 
                (current.right != null && !depthMap.ContainsKey(current.right))) {
                if (current.left != null) stack.Push(current.left);
                if (current.right != null) stack.Push(current.right);
            } else {
                stack.Pop();
                int left = depthMap.GetValueOrDefault(current.left, 0);
                int right = depthMap.GetValueOrDefault(current.right, 0);
                maxDiameter = Math.Max(maxDiameter, left + right);
                depthMap[current] = Math.Max(left, right) + 1;
            }
        }

        return maxDiameter;
    }
}
```

