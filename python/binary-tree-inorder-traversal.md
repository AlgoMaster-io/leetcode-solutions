# [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Table of Contents
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Stack](#iterative-approach-using-stack)
3. [Morris Traversal Approach](#morris-traversal-approach)

## Recursive Approach

### Intuition
The recursive approach is the most straightforward way to approach the problem. In an inorder traversal, we visit the left subtree first, then the root node, and finally the right subtree. This naturally lends itself to a recursive solution where the traversal order is inherently preserved.

### Steps
1. Define a helper function `dfs(node)` that performs a depth-first search.
2. If the node is null, simply return.
3. Recursively call the helper function on the left child.
4. Append the node's value to the result list.
5. Recursively call the helper function on the right child.
6. Return the accumulated result list.

### Code
```python
def inorderTraversal(root):
    def dfs(node):
        if not node:
            return
        # Traverse the left subtree
        dfs(node.left)
        # Visit the root node
        result.append(node.val)
        # Traverse the right subtree
        dfs(node.right)
    
    result = [] 
    dfs(root)
    return result
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the binary tree. Each node is visited exactly once.
- **Space Complexity:** O(N), the space used by the recursion stack in the worst case (skewed tree).

## Iterative Approach using Stack

### Intuition
Using a stack, we can simulate the recursive function's call stack. This approach iteratively simulates an inorder traversal by using a stack to keep track of nodes while traversing down the left children.

### Steps
1. Initialize an empty stack and a pointer `curr` pointing to the root node.
2. While `curr` is not null or the stack is not empty:
   - Push the current node onto the stack and set `curr` to its left child, until reaching the leftmost node.
   - Pop a node from the stack, visit it by appending its value to the result list.
   - Set `curr` to the right child of the popped node.
3. Continue this until all nodes are visited and the stack is empty.

### Code
```python
def inorderTraversal(root):
    result, stack = [], []
    curr = root
    
    while curr or stack:
        # Reach the left most Node of the current Node
        while curr:
            stack.append(curr)
            curr = curr.left
        
        # Current must be None at this point
        curr = stack.pop()
        # Visit the node
        result.append(curr.val)
        # We have visited the node and its left subtree.
        # Now, it's right subtree's turn.
        curr = curr.right
        
    return result
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes. Each node is pushed and popped once from the stack.
- **Space Complexity:** O(N), in the worst case, we might have to store all nodes in the stack (for a completely unbalanced tree).

## Morris Traversal Approach

### Intuition
The Morris Traversal is an advanced technique that allows inorder tree traversal without using any extra space (i.e., without recursive stack or explicit stack). It uses the tree's null links to form a temporary threaded binary tree that helps us traverse the tree.

### Steps
1. Initialize the current node as root.
2. While the current node is not null:
   - If the current node has no left child, visit this node and move to the right child.
   - If the current node has a left child, find the inorder predecessor of the current node (rightmost node in the left subtree).
   - If the predecessor's right link is null, make the current node the right child of the predecessor and move to the left child of the current node.
   - If the predecessor's right link is already pointing to the current node (thread is created), revert the changes (remove the thread), visit the current node, and move to the right child of the current node.
3. Continue the process until the current node becomes null.

### Code
```python
def inorderTraversal(root):
    result, curr = [], root
    
    while curr:
        if curr.left is None:
            # If there is no left child, visit this node
            result.append(curr.val)
            # Move to right child
            curr = curr.right
        else:
            # Find the inorder predecessor of current
            pre = curr.left
            while pre.right and pre.right != curr:
                pre = pre.right
            
            # Make current as right child of its inorder predecessor
            if pre.right is None:
                pre.right = curr
                curr = curr.left
            else:
                # Revert the changes made
                pre.right = None
                result.append(curr.val)
                curr = curr.right
                
    return result
```

### Complexity Analysis
- **Time Complexity:** O(N), as all nodes are visited once.
- **Space Complexity:** O(1), because no extra space (stack/recursion) is used. The space used is constant regardless of input size.

Each approach provides a unique perspective and trade-off between simplicity and space optimization. Recursive is easy but uses space for recursion, iterative provides more control without recursion, and Morris utilizes the structure of the tree itself for zero additional space.

