# [Leetcode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approaches
1. [Inorder Traversal - List of Values](#inorder-traversal---list-of-values)
2. [Recursive Approach - Valid Range Method](#recursive-approach---valid-range-method)
3. [Iterative Inorder Traversal](#iterative-inorder-traversal)

### Inorder Traversal - List of Values

#### Intuition:
The simplest way to verify a binary search tree is to perform an inorder traversal and check if the list of visited nodes is in ascending order (since an inorder traversal of a BST yields nodes sorted in increasing order).

#### Approach:
1. Perform an inorder traversal of the tree.
2. Store the values of the nodes in a list.
3. Check if the list is strictly increasing.

#### Code:
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorderTraversal(node, values):
    if not node:
        return
    inorderTraversal(node.left, values)  # Visit left subtree
    values.append(node.val)  # Visit current node
    inorderTraversal(node.right, values)  # Visit right subtree

def isValidBST(root: TreeNode) -> bool:
    values = []
    inorderTraversal(root, values)
    for i in range(1, len(values)):
        if values[i] <= values[i - 1]:
            return False  # List is not strictly increasing
    return True
```

#### Time Complexity:
- O(N), where N is the number of nodes in the tree, due to the inorder traversal.

#### Space Complexity:
- O(N) for storing the values of nodes.

### Recursive Approach - Valid Range Method

#### Intuition:
Each node in a binary search tree (BST) must conform to the constraints that all nodes in the left subtree are less and all nodes in the right subtree are greater. We can enforce this constraint recursively by ensuring that each node falls within an acceptable range.

#### Approach:
1. Recursively validate each subtree, passing down the range within which the current node's value must lie.
2. For the left child, the valid range is `(min_val, node.val)`.
3. For the right child, the valid range is `(node.val, max_val)`.

#### Code:
```python
def isValidBST(root: TreeNode, min_val=float('-inf'), max_val=float('inf')) -> bool:
    if not root:
        return True
    if not (min_val < root.val < max_val):
        return False  # The node's value does not lie within the valid range
    # Check recursively both subtrees of the current node
    return (isValidBST(root.left, min_val, root.val) and
            isValidBST(root.right, root.val, max_val))
```

#### Time Complexity:
- O(N) since each node is visited exactly once.

#### Space Complexity:
- O(N) due to the recursion stack in the worst case (i.e., linear tree structure).

### Iterative Inorder Traversal

#### Intuition:
An iterative approach using a stack can be employed to simulate the recursive nature of the inorder traversal, which helps us check if the sequence of nodes is in ascending order.

#### Approach:
1. Use a stack to simulate the recursive inorder traversal.
2. Track the previously visited node to ensure the current node's value is greater than the previous node's value.

#### Code:
```python
def isValidBST(root: TreeNode) -> bool:
    stack = []
    prev_val = float('-inf')
    while stack or root:
        while root:
            stack.append(root)
            root = root.left  # Move to the leftmost node
        root = stack.pop()
        if root.val <= prev_val:
            return False  # The current node's value is not greater than prev value
        prev_val = root.val
        root = root.right  # Traverse the right subtree
    return True
```

#### Time Complexity:
- O(N), since we visit each node exactly once.

#### Space Complexity:
- O(N), for the stack in the worst-case (linked list-like tree).

