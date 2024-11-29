# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    private int diameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return diameter;
    }

    private int depth(TreeNode node) {
        if (node == null) {
            return 0; // Base case: null node has depth 0
        }

        int leftDepth = depth(node.left);  // Calculate depth of left subtree
        int rightDepth = depth(node.right); // Calculate depth of right subtree

        // Update the diameter (maximum path length between two nodes)
        diameter = Math.max(diameter, leftDepth + rightDepth);

        // Return the depth of the current subtree
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    private int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        calculateDepth(root);
        return maxDiameter;
    }

    private int calculateDepth(TreeNode node) {
        if (node == null) {
            return 0; // Null nodes contribute 0 to depth
        }

        int left = calculateDepth(node.left);  // Left subtree depth
        int right = calculateDepth(node.right); // Right subtree depth

        maxDiameter = Math.max(maxDiameter, left + right); // Update the max diameter

        return Math.max(left, right) + 1; // Return the current depth
    }
}
```

## Approach 3: Iterative DFS Using Stack

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
import java.util.Stack;
import java.util.HashMap;

public class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0; // Edge case: empty tree
        }

        Stack<TreeNode> stack = new Stack<>();
        HashMap<TreeNode, Integer> depthMap = new HashMap<>();
        int maxDiameter = 0;

        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode current = stack.peek();
            if (current == null) {
                stack.pop();
                continue;
            }

            if ((current.left != null && !depthMap.containsKey(current.left)) || 
                (current.right != null && !depthMap.containsKey(current.right))) {
                if (current.left != null) stack.push(current.left);
                if (current.right != null) stack.push(current.right);
            } else {
                stack.pop();
                int left = depthMap.getOrDefault(current.left, 0);
                int right = depthMap.getOrDefault(current.right, 0);
                maxDiameter = Math.max(maxDiameter, left + right);
                depthMap.put(current, Math.max(left, right) + 1);
            }
        }

        return maxDiameter;
    }
}
```