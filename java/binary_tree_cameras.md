# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)
public class Solution {
    private int cameras = 0;
    
    public int minCameraCover(TreeNode root) {
        // If root returns 0, it means it needs a camera
        return dfs(root) == 0 ? cameras + 1 : cameras;
    }
    
    // Helper function for recursive traversal
    private int dfs(TreeNode node) {
        if (node == null) {
            return 1; // This node is covered
        }
        
        int left = dfs(node.left);
        int right = dfs(node.right);
        
        if (left == 0 || right == 0) {
            cameras++;
            return 2; // Placing a camera at this node
        }
        
        return (left == 2 || right == 2) ? 1 : 0;
        // 2 indicates child has a camera, 1 means this node is covered
        // 0 indicates this node needs a camera
    }
}

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    private static final int HAS_CAMERA = 0;
    private static final int COVERED = 1;
    private static final int NEEDS_CAMERA = 2;
    
    public int minCameraCover(TreeNode root) {
        HashMap<TreeNode, Integer[]> dp = new HashMap<>();
        
        return dfs(root, dp) == NEEDS_CAMERA ? dp.get(root)[HAS_CAMERA] + 1 : dp.get(root)[HAS_CAMERA];
    }
    
    private int dfs(TreeNode node, HashMap<TreeNode, Integer[]> dp) {
        if (node == null) {
            return COVERED;
        }
        
        if (!dp.containsKey(node)) {
            Integer[] state = new Integer[3];

            int left = dfs(node.left, dp);
            int right = dfs(node.right, dp);
            
            if (left == NEEDS_CAMERA || right == NEEDS_CAMERA) {
                state[HAS_CAMERA] = dp.getOrDefault(node, new Integer[]{0, 0, 0})[HAS_CAMERA] + 1;
                dp.put(node, state);
                return HAS_CAMERA;
            }

            if (left == HAS_CAMERA || right == HAS_CAMERA) {
                state[COVERED] = dp.getOrDefault(node, new Integer[]{0, 0, 0})[COVERED];
                dp.put(node, state);
                return COVERED;
            }

            state[NEEDS_CAMERA] = dp.getOrDefault(node, new Integer[]{0, 0, 0})[NEEDS_CAMERA];
            dp.put(node, state);
            return NEEDS_CAMERA;
        }

        return dp.get(node)[0];
    }
}

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.

