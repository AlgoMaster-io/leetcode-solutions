# [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Optimized Recursive Approach with HashMap for Inorder Index](#approach-2-optimized-recursive-approach-with-hashmap-for-inorder-index)

---

## Approach 1: Recursive Approach

### Intuition
The basic idea behind reconstructing the binary tree from inorder and postorder traversals is to make use of the properties of these traversals:
- In a postorder traversal, the last element is always the root of the tree.
- In an inorder traversal, the root element splits the list into left subtree and right subtree.

Using the above properties, we can reconstruct the binary tree recursively:
1. Identify the root node from the last element in the current postorder list.
2. Locate the index of this root node in the inorder list which divides the tree into left and right subtrees.
3. Recursively construct the right and left subtrees.

### Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(inorder, postorder):
    if not inorder or not postorder:
        return None
    
    # The last element in the current postorder list is the root
    root_value = postorder.pop()
    root = TreeNode(root_value)
    
    # Find the index of the root in inorder list
    root_index_in_inorder = inorder.index(root_value)
    
    # Recursively build the right and left subtree
    # IMPORTANT: Because we are popping from the end of postorder list, we must build right subtree first
    root.right = buildTree(inorder[root_index_in_inorder+1:], postorder)
    root.left = buildTree(inorder[:root_index_in_inorder], postorder)
    
    return root
```

### Time Complexity
- The time complexity is O(n^2) in the worst case, mainly due to the O(n) index calls while searching for the root in the `inorder` list.

### Space Complexity
- The space complexity is O(n), which is mainly used for the recursion stack.

---

## Approach 2: Optimized Recursive Approach with HashMap for Inorder Index

### Intuition
The above approach can be optimized by using a HashMap to store the indices of elements in the inorder list, enabling O(1) access time to find the root index. This will help reduce the time complexity from O(n^2) to O(n).

### Solution

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(inorder, postorder):
    # Create a map to get index of any element in `inorder` in O(1) time
    inorder_index_map = {value: idx for idx, value in enumerate(inorder)}
    
    def buildTreeHelper(in_left, in_right):
        # if there is no elements to construct the tree
        if in_left > in_right:
            return None
        
        # pick up the last element as a root from postorder
        root_value = postorder.pop()
        root = TreeNode(root_value)
        
        # root splits inorder list into left and right subtrees
        index = inorder_index_map[root_value]

        # build right subtree first because we are reducing the postorder index
        root.right = buildTreeHelper(index + 1, in_right)
        root.left = buildTreeHelper(in_left, index - 1)
        
        return root
    
    # start from the whole range of inorder
    return buildTreeHelper(0, len(inorder) - 1)

```

### Time Complexity
- The time complexity is O(n) since each element is processed once.

### Space Complexity
- The space complexity is O(n) due to the HashMap storage and recursion stack used.

