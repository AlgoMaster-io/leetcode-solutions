# [Leetcode 114: Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Stack](#iterative-approach-using-stack)
3. [Morris Traversal (In-Place O(1) Space)](#morris-traversal-in-place)

### Recursive Approach

#### Intuition:
The goal is to flatten the binary tree into a linked list following pre-order traversal. In this approach, we use a recursive method to traverse and rearrange nodes.

1. Recursively flatten the left and right subtrees.
2. Store the right subtree, and move the left subtree to where the right subtree should be.
3. Traverse to the end of the new right subtree and attach the previously stored right subtree.

#### Code:
```python
class Solution:
    def flatten(self, root):
        if not root:
            return
        
        # Flatten the left and right subtree
        self.flatten(root.left)
        self.flatten(root.right)

        # Store the right subtree
        right_subtree = root.right
        
        # Move left subtree to the right
        root.right = root.left
        root.left = None

        # Find the end of the new right subtree
        current = root
        while current.right:
            current = current.right

        # Attach the previously stored right subtree
        current.right = right_subtree
```

#### Complexity:
- **Time Complexity:** O(n), where n is the number of nodes in the tree, since each node is visited once.
- **Space Complexity:** O(h), where h is the height of the tree, due to the recursion stack.

---

### Iterative Approach using Stack

#### Intuition:
This approach uses a stack to iteratively manage nodes similar to pre-order traversal:

1. Use a stack to store nodes, starting with the root.
2. Iteratively process nodes, adjusting pointers to flatten.
3. Push the right and then left children to the stack (to mimic pre-order traversal).

#### Code:
```python
class Solution:
    def flatten(self, root):
        if not root:
            return
        
        stack = [root]
        
        while stack:
            current = stack.pop()
            
            if current.right:
                stack.append(current.right)
            if current.left:
                stack.append(current.left)

            # Point the current node's right to the top of the stack (next pre-order node)
            if stack:
                current.right = stack[-1]
            current.left = None
```

#### Complexity:
- **Time Complexity:** O(n), where n is the number of nodes.
- **Space Complexity:** O(n), for the stack storing nodes.

---

### Morris Traversal (In-Place O(1) Space)

#### Intuition:
The Morris traversal technique enables in-place transformation without the use of extra space. Here, we borrow the threaded binary tree concept:

1. For each node, find the rightmost node of its left subtree.
2. Link this rightmost node to the current node's right child.
3. Move the left subtree to the right and proceed to the next right node.

#### Code:
```python
class Solution:
    def flatten(self, root):
        current = root
        
        while current:
            if current.left:
                # Find the rightmost node of the left subtree
                pre = current.left
                while pre.right:
                    pre = pre.right
                
                # Connect rightmost node of left subtree to the current's right subtree
                pre.right = current.right
                
                # Move left subtree to the right
                current.right = current.left
                current.left = None
                
            # Move on to the right
            current = current.right
```

#### Complexity:
- **Time Complexity:** O(n), where n is the number of nodes.
- **Space Complexity:** O(1), in-place transformation with no additional space.

---

Each of these approaches provides a unique way to solve the problem, ranging from basic to optimal, with the Morris Traversal being the most efficient in terms of space.

