# Validate Binary Search Tree
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A Binary Search Tree (BST) is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.
### Constraints:
- The number of nodes in the tree is in the range [1, 10^4].
- -2^31 <= Node.val <= 2^31 - 1

### Examples
```javascript
Input: root = [2,1,3]
Output: true

Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but the right child's value is 4, which violates the BST property.
```

## Approaches to Solve the Problem
### Approach 1: Recursive Approach with Bounds (Top-Down Validation)
##### Intuition:
The key property of a Binary Search Tree (BST) is that each node’s value must be greater than all nodes in its left subtree and less than all nodes in its right subtree. We can validate a BST by recursively ensuring that each node lies within an allowable value range, which changes as we go deeper into the tree.

- For each node, the value should lie within a specific range (min_val < node.val < max_val).
- For the left child, the max_val becomes the current node’s value.
- For the right child, the min_val becomes the current node’s value.

Steps:
1. Define a helper function that takes a node and its allowable range (min_val, max_val).
2. If the node is None, return True (an empty tree is valid).
3. Check if the node’s value lies within the range (min_val < node.val < max_val).
4. Recursively validate the left and right subtrees, updating the range accordingly.
5. If both subtrees are valid, return True. Otherwise, return False.
##### Visualization:
```rust
For the tree:

      5
     / \
    1   4
       / \
      3   6

- Initially, the range for the root is (-∞, ∞).
- For the left child of 5, the range is (-∞, 5), and for the right child, it’s (5, ∞).
- The value 4 is in the right subtree but does not satisfy the condition 4 > 5, so the tree is invalid.
```
##### Time Complexity:
O(n), where n is the number of nodes in the tree. We visit each node once.
##### Space Complexity:
O(h), where h is the height of the tree. The space complexity is proportional to the recursion depth, which is O(log n) for a balanced tree and O(n) in the worst case (unbalanced tree).
##### Python Code:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # Helper function to validate the tree
        def validate(node, min_val, max_val):
            if not node:
                return True
            if not (min_val < node.val < max_val):
                return False
            return (validate(node.left, min_val, node.val) and 
                    validate(node.right, node.val, max_val))
        
        # Call the helper function with initial bounds
        return validate(root, float('-inf'), float('inf'))
```
### Approach 2: In-Order Traversal (DFS)
##### Intuition:
In a Binary Search Tree (BST), an in-order traversal of the nodes should produce a sorted sequence of values in ascending order. If the in-order traversal yields values that are not in ascending order, the tree is not a valid BST.

- Traverse the tree in-order (left → root → right).
- Keep track of the previous node’s value.
- If any node’s value is less than or equal to the previous node’s value, the tree is invalid.

Steps:
1. Perform an in-order traversal of the tree.
2. For each node, compare its value to the previous node’s value (starting with None for the first node).
3. If the current node’s value is less than or equal to the previous node’s value, return False.
4. If the entire traversal is valid, return True.
##### Visualization:
```rust
For the tree:
      2
     / \
    1   3

The in-order traversal gives [1, 2, 3], which is in ascending order, so the tree is valid.

For the tree:
      5
     / \
    1   4
       / \
      3   6
The in-order traversal gives [1, 5, 3, 4, 6], which is not in ascending order, so the tree is invalid.
```
##### Time Complexity:
- O(n), where n is the number of nodes. We visit each node once.
##### Space Complexity:
- O(h), where h is the height of the tree. The space complexity is proportional to the recursion depth or the stack size in the iterative version.
##### Python Code:
```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        self.prev = None
        
        def inorder(node):
            if not node:
                return True
            if not inorder(node.left):
                return False
            if self.prev is not None and node.val <= self.prev:
                return False
            self.prev = node.val
            return inorder(node.right)
        
        return inorder(root)
```
### Approach 3: Iterative In-order Traversal with Stack
##### Intuition:
The in-order traversal can also be implemented iteratively using a stack. This approach avoids recursion depth issues by explicitly managing the stack.

- Use a stack to simulate in-order traversal.
- Keep track of the previous node’s value, and for each node, ensure the current node’s value is greater than the previous one.

Steps:
1. Initialize an empty stack and set the previous node value to None.
2. Use a while loop to traverse the tree in-order.
3. For each node, compare its value with the previous node’s value.
4. If the current node’s value is less than or equal to the previous node’s value, return False.
5. If the traversal completes without violations, return True.
##### Time Complexity:
- O(n), where n is the number of nodes. Each node is visited once.
##### Space Complexity:
- O(h), where h is the height of the tree. The stack size is proportional to the height of the tree.
##### Python Code:
```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        stack = []
        prev = None
        
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            if prev is not None and root.val <= prev:
                return False
            prev = root.val
            root = root.right
        
        return True
```
### Edge Cases
1. Single Node Tree: A tree with only one node is always a valid BST.
2. Empty Tree: An empty tree (root == None) is considered a valid BST.
3. Left Skewed Tree: A tree where all nodes have only left children must still follow the BST property.
4. Right Skewed Tree: Similarly, a tree where all nodes have only right children must still follow the BST property.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Recursive with Bounds (Top-Down)			      | O(n)      | O(h)             |
| In-order Traversal (DFS Recursive) (Early)				      | O(n)      | O(h)             |
| Iterative In-order Traversal (Stack) (Stack)				      | O(n)      | O(h)             |

All three approaches have the same time complexity of O(n), where n is the number of nodes in the tree, and the space complexity is O(h), where h is the height of the tree. The choice between these approaches depends on personal preference or constraints such as recursion depth limits.

- The Recursive with Bounds approach is intuitive and uses the property of a valid BST where each node must lie within a certain range.
- The In-order Traversal (DFS) approach leverages the fact that in-order traversal of a valid BST should yield sorted values.
- The Iterative In-order Traversal approach avoids recursion and uses a stack to simulate the in-order traversal.