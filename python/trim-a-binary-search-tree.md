### [Leetcode Problem 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

#### Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Stack](#iterative-approach-using-stack)

---

### Recursive Approach

The recursive approach is straightforward due to the nature of binary search trees (BSTs). We can take advantage of the properties of BSTs to trim the unwanted nodes (i.e., nodes having values outside the range `[low, high]`).

#### Intuition

- If a node's value is less than `low`, then the entire left subtree should be discarded because all values there will be less than `low`.
- If a node's value is greater than `high`, then the entire right subtree should be discarded for the same reason.
- Otherwise, the node is within range, and we will recursively trim its left and right subtrees.

This approach uses the recursive depth-first search (DFS) pattern to explore all nodes.

#### Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def trimBST(root: TreeNode, low: int, high: int) -> TreeNode:
    # Base case: If root is None, there's nothing to trim
    if not root:
        return None
    
    if root.val < low:
        # If current node's value is less than low, recursively trim right subtree
        # because left subtree will definitely be less than low
        return trimBST(root.right, low, high)
    elif root.val > high:
        # If current node's value is greater than high, recursively trim left subtree
        # because right subtree will definitely be greater than high
        return trimBST(root.left, low, high)
    
    # If current node's value is between low and high:
    # Trim both the subtrees
    root.left = trimBST(root.left, low, high)
    root.right = trimBST(root.right, low, high)
    
    # Return the current node since it's within the range
    return root
```

#### Complexity Analysis

- **Time Complexity**: O(N), where N is the number of nodes in the tree. In the worst case, we may have to visit all nodes.
- **Space Complexity**: O(H), where H is the height of the tree. This accounts for the recursion stack space.

---

### Iterative Approach Using Stack

An iterative approach can achieve the same result using a stack to simulate the function call stack used in recursion.

#### Intuition

- Use a stack to perform iterative tree traversal.
- As with the recursive approach, decide whether to keep or discard subtrees based on current node value relative to `low` and `high`.

This approach is similar to a DFS using a stack.

#### Code

```python
def trimBST(root: TreeNode, low: int, high: int) -> TreeNode:
    if not root:
        return None
    
    # Trim the root node to make sure root is within [low, high]
    while root and (root.val < low or root.val > high):
        if root.val < low:
            root = root.right
        elif root.val > high:
            root = root.left

    # Use stack for iterative DFS
    stack = [root]
    
    while stack:
        node = stack.pop()
        
        # Trimming the left child
        while node and node.left and node.left.val < low:
            node.left = node.left.right
        if node and node.left:
            stack.append(node.left)
        
        # Trimming the right child
        while node and node.right and node.right.val > high:
             node.right = node.right.left
        if node and node.right:
            stack.append(node.right)

    return root
```

#### Complexity Analysis

- **Time Complexity**: O(N), where N is the number of nodes. Every node is visited at most twice.
- **Space Complexity**: O(H), where H is the height of the tree. In the worst case of a skewed tree, H could be N. This is due to the stack holding nodes.

By following these solutions, you can effectively trim a binary search tree by eliminating nodes with values outside a specified inclusive range `[low, high]`.

