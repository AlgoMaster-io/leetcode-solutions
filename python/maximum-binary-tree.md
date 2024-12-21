# [Leetcode 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches
- [Approach 1: Recursive Construction](#approach-1-recursive-construction)
- [Approach 2: Iterative Construction using Stack (Optimized)](#approach-2-iterative-construction-using-stack-optimized)

### Approach 1: Recursive Construction

**Intuition:**
The problem can directly be mirrored into a recursive approach. The largest element in the current list becomes the root, and the list is split into two sublists for the left and right subtrees. Recursively build the same tree structure for these sublists.

#### Steps:
1. Identify the maximum value in the current list, and use it as the root node of the binary tree/subtree.
2. Split the list into two sublists: one from the start to the maximum index (exclusive), and the other from the maximum index+1 to the end.
3. Recursively call the function on these two sublists to construct the left and right subtrees, respectively.
4. Connect these recursively constructed trees back to the root node and return the root node.

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def constructMaximumBinaryTree(nums):
    # Base case: if the nums list is empty, return None
    if not nums:
        return None

    # Find the index of the maximum element in the list
    max_index = nums.index(max(nums))

    # Create a new tree node with the maximum value
    root = TreeNode(nums[max_index])
    
    # Recursively construct the left subtree
    root.left = constructMaximumBinaryTree(nums[:max_index])

    # Recursively construct the right subtree
    root.right = constructMaximumBinaryTree(nums[max_index + 1:])
    
    return root
```

**Time Complexity:** O(n^2) in the worst case, where n is the length of the array. Each recursive call requires scanning the entire list to find the maximum.

**Space Complexity:** O(n) for the recursion stack in the worst case of an unbalanced tree.

### Approach 2: Iterative Construction using Stack (Optimized)

**Intuition:**
Instead of performing numerous recursive calls and repeatedly scanning sublists, this approach uses a stack to build the tree iteratively. The idea is to ensure that nodes are always populated in a manner where each newly encountered number attaches appropriately to its previous maximum, taking advantage of the Monotonic Stack principle by maintaining a stack of nodes in decreasing order.

#### Steps:
1. Iterate over each number in the list, while using a stack.
2. For each number, pop all smaller elements from the stack, since they cannot be the parent of the current node.
3. The last popped element becomes the left child of the current number.
4. If the stack is not empty, the current number becomes the right child of the stack’s top element.
5. Push the current number onto the stack, represented as a TreeNode.
6. The root of the tree is the first element in the stack after processing all elements.

```python
def constructMaximumBinaryTree(nums):
    # Initialize an empty stack
    stack = []

    # Iterate over each number in the nums list
    for num in nums:
        # Create a node for the current number
        current = TreeNode(num)

        # While the stack is not empty and the current node value is greater than the top of the stack
        while stack and stack[-1].val < num:
            # Pop the top element from the stack and make it the left child of the current node
            current.left = stack.pop()

        # If the stack is not empty, assign the current node as the right child of the top node in the stack
        if stack:
            stack[-1].right = current

        # Push the current node onto the stack
        stack.append(current)

    # The first element in the stack is the root of the maximum binary tree
    return stack[0]
```

**Time Complexity:** O(n), since each element is pushed and popped from the stack once.

**Space Complexity:** O(n), for storing the stack of nodes.

