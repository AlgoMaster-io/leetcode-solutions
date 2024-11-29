# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
```python
# Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
# Space Complexity: Catalan space complexity due to recursion stack

from typing import List, Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        if n == 0:
            return []
        return self.generateTreesHelper(1, n)

    def generateTreesHelper(self, start: int, end: int) -> List[Optional[TreeNode]]:
        allTrees = []
        if start > end:
            allTrees.append(None)
            return allTrees

        # Iterate through each number as the root
        for i in range(start, end + 1):
            # Generate all left and right subtrees
            leftTrees = self.generateTreesHelper(start, i - 1)
            rightTrees = self.generateTreesHelper(i + 1, end)

            # Combine left and right subtrees with the root i
            for left in leftTrees:
                for right in rightTrees:
                    currentTree = TreeNode(i)
                    currentTree.left = left
                    currentTree.right = right
                    allTrees.append(currentTree)

        return allTrees
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
```python
# Time Complexity: Catalan number time complexity
# Space Complexity: O(n^2) for the memoization table

from typing import List, Optional
from collections import defaultdict

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def __init__(self):
        self.memo = defaultdict(list)
    
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        if n == 0:
            return []
        return self.generateTreesHelper(1, n)
    
    def generateTreesHelper(self, start: int, end: int) -> List[Optional[TreeNode]]:
        if start > end:
            return [None]
        
        memo_key = f"{start}-{end}"
        if memo_key in self.memo:
            return self.memo[memo_key]

        allTrees = []

        # Iterate through each number as the root
        for i in range(start, end + 1):
            # Generate all left and right subtrees
            leftTrees = self.generateTreesHelper(start, i - 1)
            rightTrees = self.generateTreesHelper(i + 1, end)

            # Combine left and right subtrees with the root i
            for left in leftTrees:
                for right in rightTrees:
                    currentTree = TreeNode(i)
                    currentTree.left = left
                    currentTree.right = right
                    allTrees.append(currentTree)
        
        self.memo[memo_key] = allTrees
        return allTrees
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

