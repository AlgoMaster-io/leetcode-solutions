# [Leetcode 979: Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

## Table of Contents
1. [Approach 1: Depth First Search (DFS)](#approach-1)
2. [Approach 2: Optimized Depth First Search (DFS)](#approach-2)

---

## Approach 1: Depth First Search (DFS)<a name="approach-1"></a>

### Intuition:
We need to make sure that each node in the binary tree has exactly one coin. The task is essentially to calculate the minimum number of moves required to achieve this distribution where a move is defined as transferring a coin from one node to a directly connected node. 

The idea is to use a recursive depth-first search (DFS) approach. We will traverse each subtree and balance the coins by calculating the excess coins to move up the tree. A node `has/expects` coins based on 1 + child balance - coins of the current node. The movements will be the absolute sum of these excess coin balances.

### Code:

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def distributeCoins(self, root: TreeNode) -> int:
        self.moves = 0

        def dfs(node):
            # If the node is None, there are no coins to balance, return 0
            if not node:
                return 0
            
            # Recursive DFS on the left and right children
            left_balance = dfs(node.left)
            right_balance = dfs(node.right)

            # Total moves equals the sum of absolute coin balances
            self.moves += abs(left_balance) + abs(right_balance)

            # Return the balance of coins for the node
            return node.val + left_balance + right_balance - 1
        
        # Start DFS from the root node
        dfs(root)
        return self.moves
```

### Time and Space Complexity:
- **Time Complexity**: O(N), where N is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(H), where H is the height of the tree due to recursion stack. In the worst case, H could be O(N) if the tree is skewed.

---

## Approach 2: Optimized Depth First Search (DFS)<a name="approach-2"></a>

### Intuition:
The previous approach is quite efficient, however, let me explain the optimized version further by stressing optimal use of in-place calculations and reducing understandability. We can improve our understanding of why each node balance is `node.val + left_balance + right_balance - 1`. Each node needs one coin and contributes its excess coins to its parent.

The left_balance and right_balance express what the subtree rooted at those nodes owe or have extra for balance, and adding the parent's coin value, we offset this balance by how far the current node is from balance (len(node) = 1).

### Code:

```python
# The code will remain the same as it is already optimized
# including any slight performance or understanding enhancements.
```

### Time and Space Complexity:
- **Time Complexity**: O(N), visiting every node once ensuring an optimal single post-order DFS traversal.
- **Space Complexity**: O(H), where H is the height of the tree due to the recursive call stack maintained till the deepest leaf.

**Note:** In both implementations, understanding and applying the DFS post-order traversal ensures that internal computation balances are efficiently calculated, thus making the algorithm optimal. Each step uses in-place calculations, making unnecessary additional data structures avoided.

