# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches
1. [Basic Recursive Approach](#basic-recursive-approach)
2. [Greedy DFS Approach](#greedy-dfs-approach)

### Basic Recursive Approach

#### Intuition

In this problem, we need to cover all nodes of a binary tree with the minimum number of cameras. A straightforward brute force approach would be to consider placing a camera at every node, which clearly isn't efficient. Instead, we can use a recursive approach where we attempt to minimize the number of cameras required by deciding the optimal placement of each camera during the traversal of the tree.

We third introduce states for nodes:
- `0` if the node is covered
- `1` if the node is monitored by a camera placed at one of its children
- `2` if the node has a camera

The base idea is:
- A leaf node would need a camera at its parent.
- A parent of a leaf can cover itself and the leaf if it has a camera. 
- Each node decides whether it needs a camera based on whether its children are covered or need a camera.

#### Java Code

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
    private int numCameras = 0;

    public int minCameraCover(TreeNode root) {
        // If the root itself is not covered, we need an additional camera
        if (dfs(root) == 0) {
            numCameras++;
        }
        return numCameras;
    }

    private int dfs(TreeNode node) {
        if (node == null) {
            // If node is null, it's considered to be covered
            return 1;
        }

        int left = dfs(node.left);
        int right = dfs(node.right);

        // If any child is not covered, we need to place a camera at this node
        if (left == 0 || right == 0) {
            numCameras++;
            return 2;
        }
        
        // If any child has a camera, this node is considered to be covered
        if (left == 2 || right == 2) {
            return 1;
        }

        // If both the children are covered but do not have a camera, this node is currently not covered.
        return 0;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree due to the recursion stack.

### Greedy DFS Approach

#### Intuition

This approach is similar to the basic recursive approach but optimizes further by attempting to make fewer recursive calls with more direct decisions at each node. We use a greedy strategy where we "push" the camera placement decision from leaf nodes upwards as we recursively return results. This avoids unnecessary redundant checks when it comes to covering already covered nodes or leaves.

The idea is to serve similar states but efficiently using post-order traversal decisions:
- When a leaf is reached, it should let its parent know it needs coverage.
- If either child of a node is not covered, place a camera at this node.
- If any child is covered by a camera, mark this node as covered.

This simplification helps avoid additional field/state in the DFS function or the need to backtrack excessively.

#### Java Code

```java
class Solution {
    private int numCameras = 0;
    
    public int minCameraCover(TreeNode root) {
        // Again, check if the root needs a camera  
        if (dfs(root) == 0) {
            numCameras++;
        }
        return numCameras;
    }

    private int dfs(TreeNode node) {
        if (node == null) {
            // Null nodes are inherently covered
            return 1;
        }

        int left = dfs(node.left);
        int right = dfs(node.right);

        if (left == 0 || right == 0) {
            // If any child needs coverage, place a camera here
            numCameras++;
            return 2;
        }

        if (left == 2 || right == 2) {
            // If there are cameras on the children, this node is covered
            return 1;
        }

        // If children are covered but this node isn't, signal up it needs coverage
        return 0;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), each node is processed once.
- **Space Complexity**: O(H), due to the recursion stack.

By following these approaches, we aim to iteratively improve the performance to find the minimum number of cameras that can monitor all nodes of the binary tree in the most efficient manner.

