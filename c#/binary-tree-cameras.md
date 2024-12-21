# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches
1. [Depth-First Search with Greedy Approach](#depth-first-search-with-greedy-approach)

### Depth-First Search with Greedy Approach

**Intuition:**

The problem is about placing the minimum number of cameras to monitor all nodes in the binary tree. A camera placed on a node can monitor the node itself, its parent, and its immediate children. The idea is to perform a Depth-First Search (DFS) traversal from the leaves towards the root, making greedy decisions about where to place cameras.

- Start by thinking of the appearance of the tree as a bottom-up process, where the decision to place a camera is dependent on whether the children themselves are covered or have cameras. 
- Use a post-order DFS traversal because you need to make decisions based on the state of a node's children before deciding for the node itself.

**Steps:**

1. Define a DFS function that returns the state of each node:
   - Return 0 if the node needs a camera.
   - Return 1 if the node is covered.
   - Return 2 if the node has a camera.
   
2. Traverse the tree in post-order fashion:
   - If a child returns 0 (needs a camera), place a camera at the current node.
   - If at least one child returns 2 (has a camera), the current node is covered.
   - If all children are covered (return 1), the current node needs a camera but does not place one because it expects its parent to place one for it.

3. After the traversal, make sure the root is covered; if it’s uncovered (root returning 0), place one final camera at the root.

**Code:**

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    private enum NodeState { NeedsCamera, Covered, HasCamera }
    private int cameraCount = 0;

    public int MinCameraCover(TreeNode root) {
        if (DFS(root) == NodeState.NeedsCamera) {
            // The root itself needs a camera
            cameraCount++;
        }
        return cameraCount;
    }
    
    private NodeState DFS(TreeNode node) {
        if (node == null) {
            // A null node is considered covered
            return NodeState.Covered;
        }

        NodeState left = DFS(node.left);
        NodeState right = DFS(node.right);
        
        if (left == NodeState.NeedsCamera || right == NodeState.NeedsCamera) {
            // If any child needs a camera, place one at the current node
            cameraCount++;
            return NodeState.HasCamera;
        }
        
        if (left == NodeState.HasCamera || right == NodeState.HasCamera) {
            // If any child has a camera, the current node is covered
            return NodeState.Covered;
        }
        
        // If both children are covered but none has a camera, this node needs a camera
        return NodeState.NeedsCamera;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(H), where H is the height of the tree, which accounts for the stack space in the DFS traversal. In the worst case, it could be O(N) for a skewed tree.

