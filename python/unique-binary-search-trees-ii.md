# [LeetCode Problem 95: Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Navigation
- [Approach 1: Recursive Solution with Detailed Intuition](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)

## Approach 1: Recursive Solution with Detailed Intuition

### Intuition:
The problem requires generating all structurally unique BSTs (binary search trees) that store values 1 to n. A recursive approach suits well here since we can define a BST rooted at 'r' as: 

- The left subtree can consist of any of the BSTs generated from 1 to r-1.
- The right subtree can consist of any of the BSTs generated from r+1 to n.

Thus, for each possible root 'r', recursively construct the left and right subtrees.

### Code:
```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val: int = 0, left: 'Optional[TreeNode]' = None, right: 'Optional[TreeNode]' = None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        if n == 0:
            return []
        
        def generate_trees(start, end):
            trees = []
            # if start > end, then the tree is empty
            if start > end:
                trees.append(None)
                return trees
            
            # Iterate through all numbers as root between start and end
            for root_val in range(start, end + 1):
                # Generate all left and right subtrees
                left_trees = generate_trees(start, root_val - 1)
                right_trees = generate_trees(root_val + 1, end)
                
                # Connecting left and right subtrees to current root
                for l in left_trees:
                    for r in right_trees:
                        root = TreeNode(root_val)
                        root.left = l
                        root.right = r
                        trees.append(root)
            
            return trees
        
        return generate_trees(1, n)
```

### Time and Space Complexity:
- **Time Complexity:** O(4^n / n^(3/2)). This is linked to the nth Catalan number, which arises due to the nature of the recursion splitting.
- **Space Complexity:** O(4^n / n^(3/2)), primarily due to the storage of all the BSTs.

## Approach 2: Dynamic Programming with Memoization

### Intuition:
To optimize the recursive approach, we can utilize memoization to store already computed results for specific ranges `[start, end]`. This prevents repeated calculations and improves efficiency. By systematically storing results from smaller ranges and building up the solution, we effectively apply the Dynamic Programming paradigm to improve performance significantly.

### Code:
```python
from typing import Dict, Tuple

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val: int = 0, left: 'Optional[TreeNode]' = None, right: 'Optional[TreeNode]' = None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        if n == 0:
            return []

        def generate_trees(start, end, memo: Dict[Tuple[int, int], List[Optional[TreeNode]]]):
            if start > end:
                return [None]
            
            if (start, end) in memo:
                return memo[(start, end)]

            all_trees = []
            for root_val in range(start, end + 1):
                left_trees = generate_trees(start, root_val - 1, memo)
                right_trees = generate_trees(root_val + 1, end, memo)

                for l in left_trees:
                    for r in right_trees:
                        root = TreeNode(root_val)
                        root.left = l
                        root.right = r
                        all_trees.append(root)

            memo[(start, end)] = all_trees
            return all_trees

        return generate_trees(1, n, {})

```

### Time and Space Complexity:
- **Time Complexity:** O(4^n / n^(3/2)). Memoization enhances efficiency, though the inherent Catalan number growth of solutions retains the complexity in the same order.
- **Space Complexity:** O(n^2), due to the storage of intermediate results in the memo dictionary.

Incorporating these techniques helps us understand and derive solutions tailored for constructing and enumerating unique BSTs efficiently.

