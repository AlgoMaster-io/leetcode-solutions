# [Problem 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approaches
- [Approach 1: Recursive Solution with Slicing](#approach-1-recursive-solution-with-slicing)
- [Approach 2: Optimized Recursive Solution with Index Map](#approach-2-optimized-recursive-solution-with-index-map)

## Approach 1: Recursive Solution with Slicing

### Intuition
In this approach, we use the properties of preorder and inorder traversals. 
- The first element of the preorder traversal is always the root.
- In the inorder traversal, the elements to the left of the root (in the inorder list) are the left subtree, and the elements to the right are the right subtree.

We can recursively identify the root, divide the traversal lists, and construct the tree.

### Steps
1. Base Case: If the preorder or inorder list is empty, return `None`.
2. Identify the root from the first element of the preorder list.
3. Find the index of this root in the inorder list to separate left and right subtrees.
4. Recursively construct the left and right subtrees by slicing the lists.
5. Return the constructed tree node.

### Code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    if not preorder or not inorder:
        return None
    
    # The first element in preorder is the root
    root_val = preorder[0]
    root = TreeNode(root_val)
    
    # Find the index of the root in inorder array
    mid_idx = inorder.index(root_val)
    
    # Build left and right subtrees
    root.left = buildTree(preorder[1:mid_idx+1], inorder[:mid_idx])
    root.right = buildTree(preorder[mid_idx+1:], inorder[mid_idx+1:])
    
    return root
```

### Complexity Analysis
- **Time Complexity:** O(N^2) in the worst case due to slicing and searching for the index. Each recursive call searches in the inorder list.
- **Space Complexity:** O(N) due to the recursion stack.

## Approach 2: Optimized Recursive Solution with Index Map

### Intuition
To optimize the previous approach, we can utilize a hashmap to store the index of each value from the inorder list. This way, finding the root index in the inorder sequence will be in O(1) time instead of O(N).

### Steps
1. Use a hashmap to store the indices of each value from the inorder traversal.
2. Maintain the current root index of preorder as a shared variable or use a helper function with an index parameter.
3. Recursively construct the tree using helpers that utilize the index map to split the inorder list efficiently.

### Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    # Hashmap for quick index retrieval
    inorder_index_map = {val: idx for idx, val in enumerate(inorder)}
    
    def array_to_tree(pre_left, pre_right, in_left, in_right):
        if pre_left > pre_right:
            return None
        
        # take the current root value from preorder
        root_val = preorder[pre_left]
        root = TreeNode(root_val)
        
        # retrieve the root index from the inorder index map
        mid_idx = inorder_index_map[root_val]
        
        # calculate the size of the left subtree
        left_tree_size = mid_idx - in_left
        
        # build left and right subtrees recursively
        root.left = array_to_tree(pre_left + 1, pre_left + left_tree_size, in_left, mid_idx - 1)
        root.right = array_to_tree(pre_left + left_tree_size + 1, pre_right, mid_idx + 1, in_right)
        
        return root
    
    return array_to_tree(0, len(preorder) - 1, 0, len(inorder) - 1)
```

### Complexity Analysis
- **Time Complexity:** O(N) since each node is processed once, and finding indices using a hashmap is O(1).
- **Space Complexity:** O(N) due to the recursion stack and storage for the hashmap.

