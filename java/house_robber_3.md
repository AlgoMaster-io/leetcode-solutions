# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    private HashMap<TreeNode, Integer> memo = new HashMap<>();

    public int rob(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (memo.containsKey(root)) {
            return memo.get(root);
        }

        // if we rob this root, we cannot rob its direct children
        int robRoot = root.val;
        if (root.left != null) {
            robRoot += rob(root.left.left) + rob(root.left.right);
        }
        if (root.right != null) {
            robRoot += rob(root.right.left) + rob(root.right.right);
        }

        // if we do not rob this root, we are free to rob its children
        int skipRoot = rob(root.left) + rob(root.right);

        // Choose the maximum money we can rob
        int result = Math.max(robRoot, skipRoot);
        memo.put(root, result); // Memoize the result for current node

        return result;
    }
}
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(h), where h is the height of the tree
public class Solution {
    public int rob(TreeNode root) {
        int[] result = robSub(root);
        return Math.max(result[0], result[1]);
    }

    private int[] robSub(TreeNode root) {
        if (root == null) {
            return new int[]{0, 0}; // Entry for {not rob, rob}
        }

        int[] left = robSub(root.left);
        int[] right = robSub(root.right);

        // if we don't rob this node, we can choose to rob or not to rob its children
        int notRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

        // if we rob this node, we cannot rob its children
        int rob = root.val + left[0] + right[0];

        return new int[]{notRob, rob}; // Return array containing {not rob, rob}
    }
}
```


