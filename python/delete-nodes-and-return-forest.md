# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

## Approaches
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)

## Approach 1: Recursive DFS

### Intuition
The problem essentially involves removing certain nodes from a binary tree and treating each disconnected subtree as a separate tree in a forest. By utilizing Depth-First Search (DFS) to explore the tree, we can carry out this operation recursively. At each node, we decide whether it should be deleted or retained, and pass this decision recursively to its children. If a node is deleted, its children are added to the resultant forest if they exist. 

### Detailed Steps:
1. **Base Case & Return**: If the current node is `None`, simply return `None`.

2. **DFS Traversal**: Traverse the left and right children using a recursive DFS approach.

3. **Check Deletion Status**: 
   - If the current node is part of the `to_delete` set, its value signifies that it is to be deleted.
     - If the left child exists, it should be added to the resultant forest.
     - Similarly, if the right child exists, it should also be added to the forest.
     - Return `None` to indicate that this node has been deleted.

4. **Retain Subtree**:
   - If the node is not to be deleted, simply attach its children to it as usual.
   - Return the current node to the previous level of recursion/tree.

5. **Initial Root Consideration**: Before calling the DFS approach, consider if the root itself should be added to the forest.

6. **Output the Forest**: At the end, collect all root nodes of created subtrees or existing retained trees.

### Python Code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def delNodes(root, to_delete):
    # Set for fast lookup of nodes to delete
    to_delete_set = set(to_delete)
    # Result list to hold roots of trees in the forest
    result = []
    
    # Helper function for DFS traversal
    def dfs(node):
        if not node:
            return None
        
        # Recursively call left and right children
        node.left = dfs(node.left)
        node.right = dfs(node.right)
        
        # If the node is to be deleted
        if node.val in to_delete_set:
            # Append non-null children to the result list
            if node.left:
                result.append(node.left)
            if node.right:
                result.append(node.right)
            # Return None to indicate this node has been deleted
            return None
        # Return the node itself if not deleted
        return node
    
    # Before doing anything, consider root
    if dfs(root):
        result.append(root)
    
    return result
```

### Time Complexity
- Each node is processed once: O(n), where n is the number of nodes in the binary tree.

### Space Complexity
- O(n) for the recursion stack in the worst case of a skewed tree.
- Additionally, O(n) space is used for storing nodes in the `result` list.

