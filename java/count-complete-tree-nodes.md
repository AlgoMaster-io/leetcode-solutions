# [Leetcode 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

In this problem, we are tasked with counting the number of nodes in a complete binary tree. A complete tree is one where each level, except possibly the last, is completely filled, and all nodes are as far left as possible.

## Approaches
- [Approach 1: Simple Tree Traversal](#approach-1-simple-tree-traversal)
- [Approach 2: Binary Search and Depth Calculation](#approach-2-binary-search-and-depth-calculation)

---

### Approach 1: Simple Tree Traversal

**Intuition:**
The most straightforward method to count all the nodes in a tree is to traverse the tree and count each node. In a normal binary tree, this approach works perfectly fine.

#### Steps:
1. Use Depth-First Search (DFS) to traverse each node starting from the root.
2. For every node visited, increment a count.
3. The total count after the complete traversal will give the number of nodes in the tree.

#### Code:
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
    public int countNodes(TreeNode root) {
        // Base case, if tree is empty
        if (root == null) return 0;

        // Count is 1 for the current node + left subtree + right subtree
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity:** O(h), where h is the height of the tree. This is due to the recursion stack.

---

### Approach 2: Binary Search and Depth Calculation

**Intuition:**
Given the properties of a complete tree, we can use a more efficient method leveraging the structure. The idea is to take advantage of the depth of the tree and binary search to determine missing nodes in the last level.

#### Steps:
1. Calculate the depth (measured by the number of edges) of the tree using the left-most path.
2. If the tree is empty, return 0.
3. Otherwise, perform binary search on the last level to check if nodes exist:
   - Half of the potential nodes can be skipped at each level of the last row, providing savings.
4. Calculate the total number of nodes using full levels and the nodes you've confirmed in the last level.

#### Code:
```java
class Solution {
    // Function to compute tree depth
    private int computeDepth(TreeNode node) {
        int depth = 0;
        while (node.left != null) {
            node = node.left;
            depth++;
        }
        return depth;
    }
    
    // Function to check if a node exists given the index and depth
    private boolean exists(int idx, int depth, TreeNode node) {
        int left = 0, right = (int)Math.pow(2, depth) - 1;
        for (int i = 0; i < depth; ++i) {
            int pivot = left + (right - left) / 2;
            if (idx <= pivot) {
                node = node.left;
                right = pivot;
            } else {
                node = node.right;
                left = pivot + 1;
            }
        }
        return node != null;
    }
    
    public int countNodes(TreeNode root) {
        // If tree is empty
        if (root == null) return 0;

        int depth = computeDepth(root);
        // If the tree has only one level
        if (depth == 0) return 1;

        // Binary search on last level
        int left = 0, right = (int)Math.pow(2, depth) - 1;
        while (left <= right) {
            int pivot = left + (right - left) / 2;
            if (exists(pivot, depth, root)) {
                left = pivot + 1;
            } else {
                right = pivot - 1;
            }
        }

        // Total nodes = nodes of full levels + nodes in the last level
        return (int)Math.pow(2, depth) - 1 + left;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** O(log^2(n)), where n is the number of nodes. We perform a log(n) depth computation and an additional log(n) binary search for each depth level.
- **Space Complexity:** O(1), as it uses constant extra space.

