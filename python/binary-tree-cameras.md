# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches
- [Greedy DFS Approach with States Analysis](#greedy-dfs-approach-with-states-analysis)

### Greedy DFS Approach with States Analysis

#### Intuition

The problem of placing the minimum number of cameras in a binary tree can be approached using a greedy strategy with a depth-first search (DFS) and a keen focus on the states of tree nodes. We can classify each node in the tree into one of three states:

1. **Covered without Camera (C):** The node is covered, either by a camera placed on one of its children or by its parent.
2. **Covered with a Camera (f):** The node has a camera.
3. **Not Covered (u):** The node is not covered by a camera, and it is neither a camera itself.

The key observation here is to place a camera in such a way that covers the most number of nodes, ideally covering itself and its parent to minimize the total number of cameras used. We rely on post-order DFS traversal because this allows us to make decisions starting from the leaf nodes upwards to the root, ensuring all nodes are evaluated for optimal camera placement.

#### Code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def minCameraCover(self, root: TreeNode) -> int:
        # Helper function for the DFS traversal.
        def dfs(node: TreeNode) -> tuple:
            if not node:
                # If the node is null, it's considered as covered without a camera.
                return (0, 0, float('inf'))

            # Post-order traversal
            L = dfs(node.left)
            R = dfs(node.right)
            
            # Min cameras needed to cover the subtree with this node covered without camera
            covered_no_cam = L[1] + R[1]
            # Min cameras needed where this node has a camera
            covered_with_cam = min(min(L) + min(R)) + 1
            # Min cameras needed for the subtree where this node is not covered (forcing child cams)
            not_covered = min(L[2] + min(R[1], R[2]), 
                              R[2] + min(L[1], L[2]))

            return covered_no_cam, covered_with_cam, not_covered
        
        # Calculate the minimum cameras starting from the root
        result = dfs(root)
        # The minimum of the covering root without a camera and with a camera gives the result
        return min(result[1], result[2])

```

#### Explanation

- **Base Case:** If the node is `null`, it is considered covered without a camera (`(0, 0, inf)`) because it doesn’t need a camera, and putting a camera in a `null` node is not practical.
  
- For each node, three pieces of information are tracked:
  - `covered_no_cam`: Minimum number of cameras needed if this node is covered itself without a camera.
  - `covered_with_cam`: Minimum number of cameras needed if this node itself has a camera.
  - `not_covered`: Minimum number of cameras needed where this node’s children are enforcing coverage.

- The function returns the minimum value between covering the root with a camera and without a camera since the root initially is not covered.

#### Complexity Analysis

- **Time Complexity:** `O(N)`, where `N` is the number of nodes in the binary tree. Every node is visited once.

- **Space Complexity:** `O(H)`, where `H` is the height of the binary tree for the recursion stack. In the worst case (unbalanced tree), this could be `O(N)`.

