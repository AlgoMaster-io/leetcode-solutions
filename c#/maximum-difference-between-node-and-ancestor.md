# [LeetCode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

## Approaches
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Iterative Depth-First Search (DFS) using Stack](#approach-2-iterative-depth-first-search-dfs-using-stack)

---

## Approach 1: Recursive Depth-First Search (DFS)

### Intuition
The primary intuition behind this solution is to perform a DFS where we track the minimum and maximum values observed from the root of the tree to the current node. At each node, we calculate the maximum difference as the absolute difference between the current node's value and both the tracked minimum and maximum values. The maximum difference for the entire tree is obtained by recursively updating these values for each node.

### Steps
1. Use a recursive helper function that takes a binary tree node, a current minimum, and a current maximum as arguments.
2. For each node, compute the maximum difference using the current node's value and the recorded minimum and maximum values.
3. Update the minimum and maximum values to include the current node's value.
4. Recurse into both the left and right subtrees updating these values.
5. Return the maximum difference encountered.

### Code
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
    public int MaxAncestorDiff(TreeNode root) {
        return Dfs(root, root.val, root.val);
    }
    
    private int Dfs(TreeNode node, int curMin, int curMax) {
        if (node == null) {
            // Base case: No node means no difference to calculate, return zero.
            return 0;
        }
        
        // Calculate current potential maximum difference
        int currentDiff = Math.Max(Math.Abs(node.val - curMin), Math.Abs(node.val - curMax));
        
        // Update current min and max values
        curMin = Math.Min(curMin, node.val);
        curMax = Math.Max(curMax, node.val);
        
        // Recurse on left and right subtrees
        int leftDiff = Dfs(node.left, curMin, curMax);
        int rightDiff = Dfs(node.right, curMin, curMax);
        
        // Return the maximum of these differences
        return Math.Max(currentDiff, Math.Max(leftDiff, rightDiff));
    }
}
```

### Time Complexity
- **O(N)**, where N is the number of nodes in the tree. Every node is visited once.

### Space Complexity
- **O(H)**, where H is the height of the tree due to the recursion stack. In the worst case of a skewed tree, it can go up to O(N).

---

## Approach 2: Iterative Depth-First Search (DFS) using Stack

### Intuition
This approach uses an iterative method with an explicit stack to simulate the DFS traversal. The basic mechanism is the same as the recursive approach: keep track of the minimum and maximum values from the root to the current node and update the maximum difference.

### Steps
1. Use a stack to perform iterative DFS, where each stack element includes the current node, current minimum, and maximum values.
2. For each node encountered, calculate the maximum difference using the node's value against the current min and max.
3. Push the left and right children of the node to the stack with updated min and max values.

### Code
```csharp
public class Solution {
    public int MaxAncestorDiff(TreeNode root) {
        Stack<(TreeNode, int, int)> stack = new Stack<(TreeNode, int, int)>();
        stack.Push((root, root.val, root.val));
        int maxDiff = 0;
        
        while (stack.Count > 0) {
            var (node, curMin, curMax) = stack.Pop();
            
            if (node != null) {
                int currentDiff = Math.Max(Math.Abs(node.val - curMin), Math.Abs(node.val - curMax));
                maxDiff = Math.Max(maxDiff, currentDiff);
                
                curMin = Math.Min(curMin, node.val);
                curMax = Math.Max(curMax, node.val);
                
                stack.Push((node.left, curMin, curMax));
                stack.Push((node.right, curMin, curMax));
            }
        }
        
        return maxDiff;
    }
}
```

### Time Complexity
- **O(N)**, where N is the number of nodes in the tree. Each node is visited once.

### Space Complexity
- **O(H)**, where H is the height of the tree due to the stack. In the worst case of a skewed tree, it can go up to O(N).

