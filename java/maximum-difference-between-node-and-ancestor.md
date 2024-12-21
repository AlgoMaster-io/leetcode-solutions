## [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

### Approaches:
1. [Recursive DFS Approach](#recursive-dfs-approach)
2. [Recursive DFS with Min-Max Tracking](#recursive-dfs-with-min-max-tracking)

---

### Recursive DFS Approach

**Intuition:**
The problem is asking us to find the maximum difference between a node and its ancestor. This suggests a tree traversal technique where we either calculate the max difference for each node recursively or store the maximum/minimum value as we traverse.

Let's start by using a Depth-First Search (DFS) approach where for each node, we compute the maximum difference using its direct ancestors (i.e., the nodes along its path to the root).

**Steps:**
1. Perform a DFS traversal starting from the root node.
2. For each node, calculate the maximum difference between the node's value and the maximum and minimum values observed so far in the DFS path.
3. Store and update the global maximum difference if it's greater than the previously recorded difference.
4. Traverse both left and right children, updating the path's maximum and minimum values accordingly.

**Code:**
```java
class Solution {
    public int maxAncestorDiff(TreeNode root) {
        // Start DFS from the root node with its own value as both min and max.
        return dfs(root, root.val, root.val);
    }
    
    private int dfs(TreeNode node, int minVal, int maxVal) {
        if (node == null) {
            // If node is null, return the difference between max and min values found so far.
            return maxVal - minVal;
        }
        
        // Update min and max values on this path as we go deeper into the tree.
        minVal = Math.min(minVal, node.val);
        maxVal = Math.max(maxVal, node.val);
        
        // Travel down both children and get their max differences.
        int leftMaxDiff = dfs(node.left, minVal, maxVal);
        int rightMaxDiff = dfs(node.right, minVal, maxVal);
        
        // Return the maximum difference obtained from either subtree.
        return Math.max(leftMaxDiff, rightMaxDiff);
    }
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree, as we visit each node once.  
**Space Complexity:** O(H), where H is the height of the tree, representing the maximum depth of the recursive call stack.

---

### Recursive DFS with Min-Max Tracking

**Intuition:**
While the first approach is efficient, we can further simplify the understanding by keeping explicit track of the minimum and maximum values along the path as we recursively search for the answer. The code remains similar, but distinctly highlights the importance of carrying along the max and min values.

**Steps:**
1. We start from the root.
2. As we dive deeper into the tree, update the min and max values seen so far.
3. For each node, calculate the difference using the updated min and max.
4. Accumulate the differences as the DFS proceeds, ensuring on each step to obtain the maximum possible value.

**Code:**
```java
class Solution {
    public int maxAncestorDiff(TreeNode root) {
        return helper(root, root.val, root.val);
    }
    
    private int helper(TreeNode node, int curMin, int curMax) {
        if (node == null) {
            // Return the accumulated max difference of this path.
            return curMax - curMin;
        }

        // Update current path's min and max based on the current node value.
        curMin = Math.min(curMin, node.val);
        curMax = Math.max(curMax, node.val);

        // Recursively query both subtrees to continue tracking min and max.
        int leftDiff = helper(node.left, curMin, curMax);
        int rightDiff = helper(node.right, curMin, curMax);

        // Determine the maximum difference across both children.
        return Math.max(leftDiff, rightDiff);
    }
}
```

**Time Complexity:** O(N) - We traverse the entire tree once.  
**Space Complexity:** O(H) - Due to the recursion stack, where H is the height of the tree.

This solution utilizes direct min-max tracking, simplifying the DFS recursion logic and highlighting the min-max updates clearly, similar to the previous approach but with more explicit tracking.



