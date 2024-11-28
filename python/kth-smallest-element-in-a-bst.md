# Kth Smallest Element in a BST
Given the root of a binary search tree (BST) and an integer k, return the kth smallest element in the BST.

A Binary Search Tree (BST) is a binary tree in which for each node, the value of all nodes in the left subtree is less than the node's value, and the value of all nodes in the right subtree is greater than the node's value.
### Constraints:
- The number of nodes in the tree is n.
- 1 <= k <= n <= 10^4
- 0 <= Node.val <= 10^4

### Examples
```javascript
Input: root = [3,1,4,null,2], k = 1
Output: 1

Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

## Approaches to Solve the Problem
### Approach 1: In-order Traversal (DFS)
##### Intuition:
A Binary Search Tree (BST) has a key property: its in-order traversal gives elements in sorted order (ascending). This means we can perform an in-order traversal of the tree and track the number of elements we’ve visited. Once we reach the kth element, we can stop the traversal and return that element.

- In-order traversal means visiting the left subtree, then the current node, and then the right subtree.
- We can traverse the tree recursively or iteratively and maintain a count of how many nodes we have visited. Once the count reaches k, we return the node’s value.

Steps:
1. Perform an in-order traversal (left → root → right).
2. Keep a counter that increments each time we visit a node.
3. When the counter equals k, return the node’s value as the kth smallest element.
##### Visualization:
```rust
For the tree:

      3
     / \
    1   4
     \
      2

The in-order traversal is [1, 2, 3, 4]. The 1st smallest element is 1, the 2nd smallest is 2, and the 3rd smallest is 3.
```
##### Time Complexity:
O(n), where n is the number of nodes in the tree. In the worst case, we may need to traverse the entire tree.
##### Space Complexity:
O(h), where h is the height of the tree, for the recursion stack in case of a deep tree. In a balanced tree, h is O(log n), and in the worst case (unbalanced tree), h is O(n).
##### Python Code:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        # Helper function for in-order traversal
        def inorder(node):
            if not node:
                return []
            return inorder(node.left) + [node.val] + inorder(node.right)
        
        # Perform in-order traversal and return the k-th element
        result = inorder(root)
        return result[k-1]
```
### Approach 2: Optimized In-order Traversal (Stop Early)
##### Intuition:
The previous solution performs a full in-order traversal and then returns the kth smallest element from the result list. We can optimize this by stopping the traversal early when we reach the kth node.

- Instead of building an entire list of node values, we maintain a counter during the traversal.
- Once the counter reaches k, we return the node’s value immediately, reducing unnecessary traversals.

Steps:
1. Perform an in-order traversal (left → root → right).
2. Keep a counter that increments each time we visit a node.
3. Once the counter reaches k, return the current node’s value and terminate the recursion.
##### Visualization:
```rust
For the tree:

      5
     / \
    3   6
   / \
  2   4
 /
1

The in-order traversal is [1, 2, 3, 4, 5, 6]. For k = 3, the traversal proceeds until we find the 3rd smallest element, which is 3.
```
##### Time Complexity:
- O(k) in the best case when the kth smallest element is found early in the traversal.
- O(n) in the worst case when k is large, and we need to traverse most of the tree.
##### Space Complexity:
- O(h), where h is the height of the tree (for recursion stack). In a balanced tree, h is O(log n), and in the worst case, h is O(n).
##### Python Code:
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        self.count = 0
        self.result = None
        
        def inorder(node):
            if not node or self.result is not None:
                return
            
            inorder(node.left)
            self.count += 1
            if self.count == k:
                self.result = node.val
                return
            inorder(node.right)
        
        inorder(root)
        return self.result
```
### Approach 3: Iterative In-order Traversal with Stack
##### Intuition:
We can implement an iterative in-order traversal using a stack instead of recursion. This avoids recursion depth issues and uses explicit stack management to traverse the tree in-order.

- Use a stack to simulate the recursive call stack.
- Push left nodes onto the stack until we reach the leftmost node.
- Pop from the stack and visit the node, incrementing the counter.
- Once the counter reaches k, return the node’s value.

Steps:
1. Initialize an empty stack.
2. Start from the root and push all the left children onto the stack.
3. Pop the top of the stack, increment the counter, and check if it equals k.
4. If not, move to the right child and repeat the process.
5. Return the value of the node when the counter equals k.
##### Visualization:
```rust
For the tree:

      3
     / \
    1   4
     \
      2

We push nodes onto the stack in the order [3, 1]. Then we pop 1, visit it, and move to 2, and finally visit 3.
```
##### Time Complexity:
- O(k), because we traverse the nodes in order until we find the kth smallest element.
##### Space Complexity:
- O(h), where h is the height of the tree, for the stack space.
##### Python Code:
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        stack = []
        while True:
            # Go as left as possible
            while root:
                stack.append(root)
                root = root.left
            
            # Pop from the stack
            root = stack.pop()
            k -= 1
            if k == 0:
                return root.val
            
            # Move to the right child
            root = root.right
```
### Edge Cases
1. Single Node: If the tree has only one node, k will be 1, and the smallest element is the root itself.
2. k = 1: If k = 1, the smallest element is the leftmost node of the BST.
3. Balanced vs. Unbalanced Tree: The solution should work efficiently for both balanced and unbalanced trees.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| In-order Traversal (DFS)			      | O(n)      | O(n)             |
| Optimized In-order Traversal (Early)				      | O(k)      | O(h)             |
| Iterative In-order Traversal (Stack)				      | O(k)      | O(h)             |

The Optimized In-order Traversal and Iterative In-order Traversal are the most efficient approaches for this problem, with time complexity of O(k) and space complexity of O(h). These methods avoid unnecessary traversals and make the solution more efficient.