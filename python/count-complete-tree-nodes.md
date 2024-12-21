[Leetcode Problem 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

### Approaches:
1. [Recursive Traversal](#approach-1-recursive-traversal)
2. [Binary Search on Tree Height](#approach-2-binary-search-on-tree-height)

---

## Approach 1: Recursive Traversal
The first approach to solve this problem is a straightforward recursive traversal of the entire tree. In this method, we recursively traverse the whole tree to count each node. This approach behaves like a depth-first search traversal in preorder or postorder sequence.

### Intuition:
- In a complete binary tree, all levels are completely filled except possibly for the last level, which is filled from the left.
- However, to count the nodes we can simply traverse every node and increment a counter as we progress.
- This is simple to implement because it involves a base case where if a node is `None`, it returns a count of 0, else returns 1 plus the count of left and right subtree nodes.

### Implementation:
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def countNodes(root: TreeNode) -> int:
    # Base case: if current node is null, return 0
    if root is None:
        return 0
    
    # Return the count of nodes in left subtree + right subtree + 1 (for the root itself)
    return 1 + countNodes(root.left) + countNodes(root.right)
```

### Complexity Analysis:
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. This is because we potentially visit every node once.
- **Space Complexity**: O(H), where H is the height of the tree. This space is used by the call stack in the worst case when the tree is skewed.

---

## Approach 2: Binary Search on Tree Height
The most efficient way to solve the problem leverages the properties of a complete binary tree to avoid counting each node. Instead, we use a binary search on the last level of the tree to determine the exact number of nodes.

### Intuition:
- A complete binary tree can be thought of as a perfect binary tree except for the last level, which can be filled partially.
- Begin by calculating the height `h` of the tree (number of edges on the longest path from root to the deepest node). A complete tree with height `h` has 2^h - 1 nodes if fully filled up to its last level.
- Use binary search to determine the number of nodes on the last level. This requires identifying where the last node actually is.
- We calculate the path to the `mid` node in the last level and verify its presence using this path.

### Implementation:
```python
def countNodes(root: TreeNode) -> int:
    # Function to compute the depth of the leftmost path (equivalent to the tree height)
    def compute_depth(node):
        depth = 0
        while node.left:
            node = node.left
            depth += 1
        return depth

    # Function to determine if a given node index exists in the last level
    def exists(index, d, node):
        left, right = 0, 2**d - 1
        for _ in range(d):
            pivot = (left + right) // 2
            if index <= pivot:
                node = node.left
                right = pivot
            else:
                node = node.right
                left = pivot + 1
        return node is not None

    # Edge case: empty tree
    if not root:
        return 0

    d = compute_depth(root)  # Total depth of the tree
    if d == 0:
        return 1  # Tree with only one node - root

    # Nodes before the last level
    nodes_before_last = (2**d) - 1

    # Binary search to count nodes on the last level
    left, right = 0, 2**d - 1
    while left <= right:
        pivot = (left + right) // 2
        if exists(pivot, d, root):
            left = pivot + 1
        else:
            right = pivot - 1

    # Total nodes = nodes before last level + nodes on the current level
    return nodes_before_last + left
```

### Complexity Analysis:
- **Time Complexity**: O(log^2 N), where N is the number of nodes. The computation of depth and binary search both take O(log N) and each involves a tree traversal.
- **Space Complexity**: O(1), as the method calculates number of nodes with only a few extra variables.

