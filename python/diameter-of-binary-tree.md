# Diameter of Binary Tree
Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

### Constraints:
- The number of nodes in the tree is in the range [1, 10^4].
- -100 <= Node.val <= 100

### Examples
```javascript
Input: root = [1,2,3,4,5]
Output: 3
Explanation: The path [4,2,1,3] or [5,2,1,3] has 3 edges.

Input: root = [1,2]
Output: 1
```

## Approaches to Solve the Problem
### Approach 1: Recursive DFS (Depth-First Search)
##### Intuition:
The diameter of a binary tree at any node is the sum of the heights (depths) of its left and right subtrees. Thus, the longest path through any node is the sum of the maximum depths of its left and right children.

We can solve this problem recursively using DFS. As we calculate the height of each subtree, we can also compute the diameter at each node by summing the heights of the left and right subtrees.

Steps:
1. For each node, the diameter is the sum of the depths of the left and right subtrees.
2. We use a helper function depth to recursively compute the depth of the tree.
3. While calculating the depth of each subtree, we also update the global maximum diameter (self.max_diameter).
4. The final diameter will be the maximum diameter found during the recursive traversal.

Key Concepts:
- Height of a node: The length of the longest path from the node to a leaf.
- Diameter at a node: The sum of the heights of its left and right subtrees.
##### Visualization:
For the tree:

```rust
      1
     / \
    2   3
   / \
  4   5

The height of node 2 is max(1,1) + 1 = 2, and the height of node 1 is max(2,1) + 1 = 3. The diameter at node 1 is 2 + 1 = 3, which is the longest path in the tree.  
```
##### Time Complexity:
O(n), where n is the number of nodes in the tree. We visit each node exactly once to compute its height and diameter.
##### Space Complexity:
O(h), where h is the height of the tree. The space is used by the recursion stack, and the maximum depth of the recursion stack is the height of the tree. In the worst case (skewed tree), the height could be n.
##### Python Code:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        # To store the maximum diameter
        self.max_diameter = 0

        # Helper function to calculate the depth of the tree
        def depth(node):
            if not node:
                return 0
            # Recursively calculate the height of the left and right subtrees
            left_depth = depth(node.left)
            right_depth = depth(node.right)
            
            # The diameter at the current node is the sum of left_depth and right_depth
            self.max_diameter = max(self.max_diameter, left_depth + right_depth)
            
            # Return the height of the node (1 + max depth of left or right subtree)
            return max(left_depth, right_depth) + 1

        # Start the depth calculation
        depth(root)
        
        # The maximum diameter found during the traversal
        return self.max_diameter
```
### Approach 2: Iterative DFS with Stack (Non-Recursive)
##### Intuition:
We can implement the same DFS logic iteratively using a stack, which allows us to avoid recursion and explicitly manage the stack.

Steps:
1. Initialize a stack to simulate the recursive traversal.
2. Use a dictionary to store the depths of the nodes as they are computed.
3. For each node, after computing the depths of its left and right subtrees, calculate the diameter at that node.
4. Track the maximum diameter during the traversal.
##### Time Complexity:
O(n), where n is the number of nodes, because we visit each node exactly once.
##### Space Complexity:
O(n), due to the stack and the dictionary used to store node depths.
##### Python Code:
```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        stack = [(root, False)]  # (node, visited)
        depths = {}
        max_diameter = 0
        
        while stack:
            node, visited = stack.pop()
            
            if node:
                if visited:
                    # When visiting the node again, calculate the depth
                    left_depth = depths.get(node.left, 0)
                    right_depth = depths.get(node.right, 0)
                    depths[node] = max(left_depth, right_depth) + 1
                    
                    # Update the max diameter
                    max_diameter = max(max_diameter, left_depth + right_depth)
                else:
                    # First time visiting the node: add it back as visited
                    stack.append((node, True))
                    # Then add left and right children to the stack
                    stack.append((node.right, False))
                    stack.append((node.left, False))
        
        return max_diameter
```
### Edge Cases
1. Single Node: If the tree has only one node, the diameter is 0 since there are no edges.
2. Linear Tree: If the tree is completely unbalanced (like a linked list), the diameter will be the number of nodes minus one, which is the height of the tree.
3. Empty Tree: If the tree is empty, the diameter should be 0.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Recursive DFS (Depth Calculation)			      | O(n)      | O(h)             |
| Iterative DFS with Stack			      | O(n)      | O(n)             |

The Recursive DFS approach is the most intuitive and efficient for this problem. It calculates the diameter in a single traversal by computing the height of each subtree while maintaining a global diameter value.