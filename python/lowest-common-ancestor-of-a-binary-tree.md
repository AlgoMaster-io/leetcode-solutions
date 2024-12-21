# [Leetcode 236: Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approaches
1. [Recursive Backtracking Approach](#recursive-backtracking)
2. [Iterative Using Parent Pointers](#iterative-using-parent-pointers)
3. [Optimal Single Sweep](#optimal-single-sweep)

---

## 1. Recursive Backtracking Approach

### Intuition
The basic idea is to perform a Depth First Search (DFS) through the tree. The LCA can be found while backtracking by checking if the two nodes p and q are found in different branches, or if one node is a parent of the other.

### Steps:

- Traverse the tree in a DFS manner.
- If you find either of the nodes p or q, return it to the previous recursion level.
- When both the left and right child return a non-null node, it means that the current root is the LCA.
- If only one of the children returns a non-null node, propagate it back to the parent.

### Code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # If we reach the end of a branch, return None.
        if not root:
            return None
        # If we find either p or q, we return it upwards in the recursion.
        if root == p or root == q:
            return root
        
        # Recursively search in the left and right subtrees
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        # If both left and right are non-null, the current root is the LCA
        if left and right:
            return root
        
        # Otherwise return whichever node is non-null
        return left if left else right
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. In the worst case, we traverse all nodes.
- **Space Complexity**: O(H), where H is the height of the tree. This space is used in the call stack because of recursion.

---

## 2. Iterative Using Parent Pointers

### Intuition
We can use an iterative approach and a parent pointer map to track each node's parent. Through this map, we can recreate the path for both nodes from each of them to the root. The first common node in these two paths will be the LCA.

### Steps:

- Traverse the tree in any manner (e.g., BFS) and create a parent pointer map for each node.
- For each node, identify the path from it to the root using the parent map.
- Compare the paths to find the first common ancestor.

### Code

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # This dictionary will keep track of each node's parent
        parent = {root: None}
        stack = [root]
        
        # Traverse the tree using a stack
        while p not in parent or q not in parent:
            node = stack.pop()
            # Record the parent of the left child
            if node.left:
                parent[node.left] = node
                stack.append(node.left)
            # Record the parent of the right child
            if node.right:
                parent[node.right] = node
                stack.append(node.right)
        
        # Collect all ancestors of p using the parent pointers
        ancestors = set()
        while p:
            ancestors.add(p)
            p = parent[p]
        
        # Check the path from q upwards for the first common ancestor
        while q not in ancestors:
            q = parent[q]
        
        return q
```

### Complexity Analysis
- **Time Complexity**: O(N), for creating the parent map and finding the LCA.
- **Space Complexity**: O(N), for storing parent pointers and ancestor set.

---

## 3. Optimal Single Sweep

### Intuition
The optimal approach merges the previous ideas into a single traversal. Using a recursive depth-first approach, it tracks the presence of p and q in the subtree and determines the LCA based on their presence in subtrees.

### Steps:

- Perform a recursive traversal similar to the recursive backtracking and track flags if nodes p and q are found.
- Use a helper function to return the current LCA found or either one of the nodes to propagate upwards.

### Code

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

        def recurse_tree(current_node):
            # Base case.
            if not current_node:
                return False

            # Left Recursion. If left subtree has either p or q.
            left = recurse_tree(current_node.left)

            # Right Recursion. If right subtree has either p or q.
            right = recurse_tree(current_node.right)

            # If the current node is either p or q
            mid = current_node == p or current_node == q

            # If two out of the three flags left, right or mid becomes True.
            if mid + left + right >= 2:
                self.ans = current_node

            # Return True if any one of the three bool values is True.
            return mid or left or right

        # Traverse the tree
        self.ans = None
        recurse_tree(root)
        return self.ans
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree. We traverse each node once.
- **Space Complexity**: O(H), where H is the height of the tree. This is due to the recursion stack.

This optimal approach has the same time and space complexity as the recursive backtracking method but is structured to efficiently manage the potential LCA during the traversal without explicit node tracking structures.

