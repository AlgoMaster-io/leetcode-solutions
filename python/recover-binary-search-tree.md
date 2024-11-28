# Recover Binary Search Tree
You are given the root of a binary search tree (BST) where exactly two nodes have been swapped by mistake. Your task is to recover the tree without changing its structure.

### Constraints:
- The number of nodes in the tree is in the range [2, 1000].
- -2^31 <= Node.val <= 2^31 - 1

### Examples
```javascript
Input: root = [1,3,null,null,2]
Output: [3,1,null,null,2]
Explanation: 1 and 3 are swapped, so the tree must be corrected to be a valid BST.

Input: root = [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]
Explanation: 2 and 3 are swapped, so the tree must be corrected to be a valid BST.
```

## Approaches to Solve the Problem
### Approach 1: In-order Traversal with DFS (Recursive)
##### Intuition:
A Binary Search Tree (BST) has the property that its in-order traversal results in a sorted sequence of values. If two nodes in the BST are swapped by mistake, the in-order traversal will contain two out-of-order values. The goal is to detect these two nodes and swap them back.

Key Concepts:
- In a valid BST, the in-order traversal is in ascending order.
- When two nodes are swapped, the in-order traversal will show two out-of-order pairs. The first one will identify the first misplaced node, and the second one will identify the second misplaced node.

Steps:
1. Perform an in-order traversal of the tree.
2. During traversal, track the previous node.
3. If you find a node where prev.val > current.val, mark the nodes involved in the swap.
   - The first node will be the larger node in the first violation.
   - The second node will be the smaller node in the second violation (or immediately in the first violation).
4. After finding the two nodes, swap their values to restore the BST.
##### Visualization:
For the tree:

```rust
      3
     / \
    1   4
       /
      2

The in-order traversal is [1, 3, 2, 4]. The two swapped nodes are 3 and 2. Swapping them back will result in a valid BST.      
```
##### Time Complexity:
O(n), where n is the number of nodes in the tree. We visit each node once during the in-order traversal.
##### Space Complexity:
O(h), where h is the height of the tree. This is due to the recursion stack, which will be O(log n) for balanced trees and O(n) in the worst case (unbalanced tree).
##### Python Code:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        self.first = self.second = self.prev = None
        
        def inorder(node):
            if not node:
                return
            
            # In-order traversal: Left, Root, Right
            inorder(node.left)
            
            # Detect a violation where prev.val > node.val
            if self.prev and self.prev.val > node.val:
                if not self.first:
                    self.first = self.prev  # First violation (larger value)
                self.second = node  # Second violation (smaller value)
            
            # Move to the next node
            self.prev = node
            
            inorder(node.right)
        
        # Perform in-order traversal to find the two swapped nodes
        inorder(root)
        
        # Swap the values of the two nodes to fix the BST
        if self.first and self.second:
            self.first.val, self.second.val = self.second.val, self.first.val
```
### Approach 2: Iterative In-order Traversal with Stack
##### Intuition:
We can perform the same in-order traversal iteratively using a stack to avoid recursion depth issues. This approach is useful when working with large trees where recursion may hit the stack limit.

The logic remains the same: track the previous node and detect violations of the BST property (prev.val > node.val).
Use a stack to perform in-order traversal iteratively.

Steps:
1. Use a stack to traverse the tree in in-order sequence.
2. As you traverse, compare each node’s value with the previous node’s value.
3. Identify the two nodes where the BST property is violated.
4. After finding the two nodes, swap their values.
##### Time Complexity:
O(n), where n is the number of nodes. Each node is visited once.
##### Space Complexity:
O(h), where h is the height of the tree. The stack stores the nodes from the root to the current leaf.
##### Python Code:
```python
class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        stack = []
        first = second = prev = None
        current = root
        
        # Iterative in-order traversal
        while stack or current:
            while current:
                stack.append(current)
                current = current.left
            
            current = stack.pop()
            
            # Detect a violation where prev.val > current.val
            if prev and prev.val > current.val:
                if not first:
                    first = prev
                second = current
            
            prev = current
            current = current.right
        
        # Swap the values of the two nodes to fix the BST
        if first and second:
            first.val, second.val = second.val, first.val
```
### Approach 3: Morris In-order Traversal (Optimal Space Complexity)
##### Intuition:
The Morris Traversal algorithm is a clever way to traverse a tree without using extra space (i.e., no stack or recursion). It works by temporarily modifying the tree to establish links (known as threading) and then restores the tree to its original structure after the traversal.

- We traverse the tree without using recursion or a stack, achieving O(1) space complexity.
- The key is to create temporary links (threads) between nodes to traverse the tree in in-order without extra space.

Steps:
1. Initialize the current node as the root.
2. For each node, if it has a left child, find the rightmost node in its left subtree and create a temporary link back to the current node.
3. If the current node does not have a left child, visit it and move to the right child.
4. After visiting, restore the tree by removing the temporary links (threads).
5. Use the same logic as before to detect the two swapped nodes and correct the BST.
##### Time Complexity:
O(n), because we visit each node exactly once.
##### Space Complexity:
O(1), because we do not use any additional space apart from a few variables.
##### Python Code:
```python
class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        first = second = prev = None
        current = root
        
        while current:
            if current.left:
                # Find the rightmost node in the left subtree or the predecessor
                predecessor = current.left
                while predecessor.right and predecessor.right != current:
                    predecessor = predecessor.right
                
                # Create a thread to the current node
                if not predecessor.right:
                    predecessor.right = current
                    current = current.left
                else:
                    # Visit the current node and break the thread
                    if prev and prev.val > current.val:
                        if not first:
                            first = prev
                        second = current
                    
                    prev = current
                    predecessor.right = None  # Remove the thread
                    current = current.right
            else:
                # Visit the current node if there's no left child
                if prev and prev.val > current.val:
                    if not first:
                        first = prev
                    second = current
                
                prev = current
                current = current.right
        
        # Swap the values of the two nodes to fix the BST
        if first and second:
            first.val, second.val = second.val, first.val
```
### Edge Cases
1. Single Node: A tree with a single node will always be a valid BST, so no changes are necessary.
2. Root and Child Swapped: If the root is swapped with one of its children, the solution must correctly identify and swap them back.
3. Complete vs. Skewed Trees: The algorithm should handle both balanced (complete) and unbalanced (skewed) trees.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Recursive In-order Traversal (DFS)			      | O(n)      | O(h)             |
| Iterative In-order Traversal (Stack)			      | O(n)      | O(h)             |
| Morris In-order Traversal					      | O(n)      | O(1)             |

- Morris In-order Traversal achieves the optimal space complexity of O(1) while maintaining linear time complexity. However, it temporarily modifies the tree.
- Both Recursive In-order and Iterative In-order Traversal approaches use O(h) space, where h is the height of the tree.

- The Recursive In-order Traversal is intuitive and easy to implement but may cause recursion stack overflow for large trees.
- The Iterative In-order Traversal avoids recursion but uses a stack, which is better suited for larger trees where recursion depth might be a concern.
- Morris In-order Traversal is space-optimized, achieving O(1) space complexity, but involves modifying the tree temporarily.