# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    private Dictionary<TreeNode, int> memo = new Dictionary<TreeNode, int>();

    public int Rob(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (memo.ContainsKey(root)) {
            return memo[root];
        }

        // if we rob this root, we cannot rob its direct children
        int robRoot = root.val;
        if (root.left != null) {
            robRoot += Rob(root.left.left) + Rob(root.left.right);
        }
        if (root.right != null) {
            robRoot += Rob(root.right.left) + Rob(root.right.right);
        }

        // if we do not rob this root, we are free to rob its children
        int skipRoot = Rob(root.left) + Rob(root.right);

        // Choose the maximum money we can rob
        int result = System.Math.Max(robRoot, skipRoot);
        memo[root] = result; // Memoize the result for current node

        return result;
    }
}
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(h), where h is the height of the tree
public class Solution {
    public int Rob(TreeNode root) {
        int[] result = RobSub(root);
        return System.Math.Max(result[0], result[1]);
    }

    private int[] RobSub(TreeNode root) {
        if (root == null) {
            return new int[]{0, 0}; // Entry for {not rob, rob}
        }

        int[] left = RobSub(root.left);
        int[] right = RobSub(root.right);

        // if we don't rob this node, we can choose to rob or not to rob its children
        int notRob = System.Math.Max(left[0], left[1]) + System.Math.Max(right[0], right[1]);

        // if we rob this node, we cannot rob its children
        int rob = root.val + left[0] + right[0];

        return new int[]{notRob, rob}; // Return array containing {not rob, rob}
    }
}
```

