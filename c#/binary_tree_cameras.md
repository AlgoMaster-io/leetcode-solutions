
# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)
public class Solution {
    private int cameras = 0;
    
    public int MinCameraCover(TreeNode root) {
        // If root returns 0, it means it needs a camera
        return Dfs(root) == 0 ? cameras + 1 : cameras;
    }
    
    // Helper function for recursive traversal
    private int Dfs(TreeNode node) {
        if (node == null) {
            return 1; // This node is covered
        }
        
        int left = Dfs(node.left);
        int right = Dfs(node.right);
        
        if (left == 0 || right == 0) {
            cameras++;
            return 2; // Placing a camera at this node
        }
        
        return (left == 2 || right == 2) ? 1 : 0;
        // 2 indicates child has a camera, 1 means this node is covered
        // 0 indicates this node needs a camera
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    private const int HAS_CAMERA = 0;
    private const int COVERED = 1;
    private const int NEEDS_CAMERA = 2;
    
    public int MinCameraCover(TreeNode root) {
        var dp = new Dictionary<TreeNode, int[]>();
        
        return Dfs(root, dp) == NEEDS_CAMERA ? dp[root][HAS_CAMERA] + 1 : dp[root][HAS_CAMERA];
    }
    
    private int Dfs(TreeNode node, Dictionary<TreeNode, int[]> dp) {
        if (node == null) {
            return COVERED;
        }
        
        if (!dp.ContainsKey(node)) {
            var state = new int[3];

            int left = Dfs(node.left, dp);
            int right = Dfs(node.right, dp);
            
            if (left == NEEDS_CAMERA || right == NEEDS_CAMERA) {
                state[HAS_CAMERA] = dp.GetValueOrDefault(node, new int[]{0, 0, 0})[HAS_CAMERA] + 1;
                dp[node] = state;
                return HAS_CAMERA;
            }

            if (left == HAS_CAMERA || right == HAS_CAMERA) {
                state[COVERED] = dp.GetValueOrDefault(node, new int[]{0, 0, 0})[COVERED];
                dp[node] = state;
                return COVERED;
            }

            state[NEEDS_CAMERA] = dp.GetValueOrDefault(node, new int[]{0, 0, 0})[NEEDS_CAMERA];
            dp[node] = state;
            return NEEDS_CAMERA;
        }

        return dp[node][0];
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.

