# [Leetcode 337: House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approaches
1. [Recursive DFS with Memoization](#approach-1-recursive-dfs-with-memoization)
2. [Optimized DFS with State Variables](#approach-2-optimized-dfs-with-state-variables)

---

## Approach 1: Recursive DFS with Memoization

### Intuition
For this problem, the thief has the choice to rob or not rob each house (node in the binary tree) with the constraint that they cannot rob two directly-linked nodes. The idea is to use DFS to explore each node while keeping track of the maximum amount we can rob. We can either choose to rob the current house and then skip its children or choose not to rob the current house and explore its children for maximum potential. Memoization is used to store results of subproblems to avoid redundant calculations for overlapping subproblems.

### Detailed Steps
1. Use a recursive function to explore each node.
2. For each node, calculate the maximum money by:
   - Robbing this node and getting results from the grandchildren.
   - Not robbing this node and getting results from its children.
3. Use a dictionary to memoize results for each node (subproblem) to avoid recalculating.
4. Return the maximum amount possible for the root node.

### Code

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def rob(root: TreeNode) -> int:
    def dfs(node):
        if not node:
            return 0
        
        if node in memo:
            return memo[node]
        
        # Rob this node
        money_with_current = node.val
        if node.left:
            money_with_current += dfs(node.left.left) + dfs(node.left.right)
        if node.right:
            money_with_current += dfs(node.right.left) + dfs(node.right.right)
        
        # Not rob this node
        money_without_current = dfs(node.left) + dfs(node.right)
        
        # Maximum of robbing current or not
        result = max(money_with_current, money_without_current)
        
        # Memoize this result
        memo[node] = result
        return result

    memo = {}
    return dfs(root)
```

### Complexity
- **Time Complexity**: \(O(N)\) where \(N\) is the number of nodes in the tree, since each node is processed once.
- **Space Complexity**: \(O(N)\) due to the recursion stack and the memoization dictionary.

---

## Approach 2: Optimized DFS with State Variables

### Intuition
In this approach, instead of using a memo dictionary, we can optimize the process by using two variables at each node: `rob_this` and `not_rob_this`. `rob_this` stores the maximum money we can rob if we rob the current node, which means we can't rob its children. `not_rob_this` holds the maximum money we can rob if we don't rob the current node, allowing us to rob its children.

### Detailed Steps
1. Use a post-order DFS approach to calculate `rob_this` and `not_rob_this` for each node.
2. For a null node, both variables are 0.
3. For each node, calculate:
   - `rob_this = node.val + not_rob_left + not_rob_right`
   - `not_rob_this = max(rob_left, not_rob_left) + max(rob_right, not_rob_right)`
4. The result for a node is the maximum of `rob_this` and `not_rob_this`.
5. Return the result for the root node.

### Code

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def rob(root: TreeNode) -> int:
    def dfs(node):
        if not node:
            return (0, 0)
        
        left = dfs(node.left)
        right = dfs(node.right)
        
        # If we rob this node, we cannot rob its children
        rob_this = node.val + left[1] + right[1]
        
        # If we do not rob this node, we can choose to rob or not rob its children
        not_rob_this = max(left) + max(right)
        
        return (rob_this, not_rob_this)

    return max(dfs(root))
```

### Complexity
- **Time Complexity**: \(O(N)\), as we still visit each node once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree, due to the call stack of the recursion.
  
By directly maintaining the two values without a large memoization structure, this approach is often more space-efficient in practice, especially for large trees.

