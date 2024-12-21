## [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### Table of Contents
- [Approach 1: Inorder Traversal and Array Construction](#approach-1-inorder-traversal-and-array-construction)
- [Approach 2: Inorder Traversal with Early Stopping](#approach-2-inorder-traversal-with-early-stopping)

### Approach 1: Inorder Traversal and Array Construction

**Intuition**:  
The property of a Binary Search Tree (BST) is that an inorder traversal visits nodes in ascending order. By performing an inorder traversal, you can construct an array of all elements of the BST in sorted order and directly access the k-th element.

```python
def kthSmallest(root, k):
    def inorder(node):
        if not node:
            return []
        # Inorder traversal: left, root, right
        return inorder(node.left) + [node.val] + inorder(node.right)

    # Construct sorted list via inorder traversal
    sorted_elements = inorder(root)
    # The k-th smallest will be at index k-1
    return sorted_elements[k - 1]
```

**Time Complexity**: O(N), where N is the number of nodes in the tree, as we visit each node once.

**Space Complexity**: O(N), where N is the number of nodes due to storage of the inorder traversal result.

### Approach 2: Inorder Traversal with Early Stopping

**Intuition**:  
Instead of constructing the entire list of sorted elements, traverse the BST in an inorder manner and keep a counter to track the number of elements processed. Stop the traversal as soon as the k-th element is encountered, which reduces unnecessary traversals and space usage.

```python
def kthSmallest(root, k):
    def inorder(node):
        if not node:
            return None

        # Traverse left subtree
        left = inorder(node.left)
        if left is not None:
            return left
        
        # Process current node
        self.count += 1
        if self.count == k:
            return node.val
        
        # Traverse right subtree
        return inorder(node.right)

    self.count = 0
    return inorder(root)
```

**Time Complexity**: Average case O(H + k), where H is the height of the tree since we potentially have to traverse only part of it given early stopping. Worst case is O(N).

**Space Complexity**: O(H), where H is the height of the tree due to the recursion stack during traversal.

Each solution progressively optimizes by reducing space and computation where possible, moving from a naive complete traversal to optimized early stopping.

