# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approaches:
1. [Naive Inorder Traversal - Collect and Process](#approach-1)
2. [Optimized Inorder Traversal - One pass](#approach-2)

---

## Approach 1: Naive Inorder Traversal - Collect and Process

### Intuition:
A Binary Search Tree (BST) has the property that in an inorder traversal, it produces a sorted order of its elements. This characteristic can be used to solve for the minimum distance between any two nodes. The idea is to perform an inorder traversal, collect the values into an array, and then find the minimum difference between consecutive elements in this sorted array.

### Steps:
1. Perform an inorder traversal of the BST and collect all the node values into a list.
2. Iterate over the list and compute the difference between each consecutive pair of elements.
3. Track the minimum difference found during this iteration.

### Python Code:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def minDiffInBST(root: TreeNode) -> int:
    # Inorder traversal to get sorted node values
    def inorder(node: TreeNode):
        if not node:
            return []
        # Traverse left subtree, node itself, and then right subtree
        return inorder(node.left) + [node.val] + inorder(node.right)
    
    # Fetch the sorted values
    values = inorder(root)
    
    # Initialize the minimum difference as a large number
    min_diff = float('inf')
    
    # Go through sorted values and find the minimum difference
    for i in range(1, len(values)):
        min_diff = min(min_diff, values[i] - values[i-1])

    return min_diff
```

### Complexity Analysis:
- **Time Complexity**: O(N), where N is the number of nodes in the BST. This is due to the inorder traversal.
- **Space Complexity**: O(N), for storing the sorted values of BST nodes.

---

## Approach 2: Optimized Inorder Traversal - One pass

### Intuition:
Instead of storing all the node values in a list, we can keep track of the previous node during the inorder traversal. This allows us to calculate the difference on-the-fly, thus reducing the space complexity. We maintain a global variable `min_diff` to store the minimum difference observed so far and update it during the traversal.

### Steps:
1. Use a helper function for inorder traversal that processes the minimum difference on-the-fly.
2. Keep track of the previous node (initialize to a very small value or None) and compute the difference with the current node.
3. Update `min_diff` if the computed difference is smaller than the current `min_diff`.

### Python Code:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def minDiffInBST(root: TreeNode) -> int:
    # Initialize previous node and minimum difference
    prev = [None]  # Use a list to keep reference
    min_diff = [float('inf')]  # Use a list to keep reference
    
    def inorder(node: TreeNode):
        if not node:
            return
        # Traverse the left subtree
        inorder(node.left)
        
        # If previous node exists, compute difference and update min_diff
        if prev[0] is not None:
            min_diff[0] = min(min_diff[0], node.val - prev[0])
        
        # Update prev node to current node value
        prev[0] = node.val
        
        # Traverse the right subtree
        inorder(node.right)
    
    # Call the inorder traversal helper
    inorder(root)
    
    return min_diff[0]
```

### Complexity Analysis:
- **Time Complexity**: O(N), where N is the number of nodes in the BST. This is due to the single inorder traversal.
- **Space Complexity**: O(1), since we store only a fixed amount of extra information (two variables) and do not use additional data structures proportional to the input size. Note that recursion stack in function calls contributes O(H) for the space complexity, where H is the height of the tree.

