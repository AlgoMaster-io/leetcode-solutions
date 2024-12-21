# [Leetcode 437: Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Solution Using Prefix Sum](#approach-2-optimized-solution-using-prefix-sum)

### Approach 1: Brute Force

#### Intuition
The brute force solution can be derived by traversing each node of the tree and for every node explore every path which starts from the current node and calculate the path sum. If the path sum equals the target, we increase our count. The idea is essentially a double DFS where one DFS traverses the nodes and for each node, another DFS computes the sum of all paths starting from that node.

#### Code
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def pathSum(root: TreeNode, targetSum: int) -> int:
    # Helper function to explore paths from a given node
    def countPaths(node, currentSum):
        if not node:
            return 0
        
        # Include the current node into the path's sum
        currentSum += node.val
        
        # Check if the current path's sum equals the target sum
        totalPaths = 1 if currentSum == targetSum else 0
        
        # Continue the path into the left and right subtrees
        totalPaths += countPaths(node.left, currentSum)
        totalPaths += countPaths(node.right, currentSum)
        
        return totalPaths
    
    # Main function to traverse each node
    if not root:
        return 0
    
    # Count paths starting from the current node
    totalPaths = countPaths(root, 0)
    
    # Count paths for the left and right subtrees
    totalPaths += pathSum(root.left, targetSum)
    totalPaths += pathSum(root.right, targetSum)
    
    return totalPaths
```
#### Complexity
- **Time Complexity:** O(N^2) - In the worst case, for each node, we may check N nodes in the paths.
- **Space Complexity:** O(N) - Space used by the recursion stack.

### Approach 2: Optimized Solution Using Prefix Sum

#### Intuition
To optimize, we should avoid recomputing the sum of paths that have already been computed for different nodes. This can be achieved using a prefix sum method. The idea is to use a hashmap to store the cumulative sum and its occurrences before reaching the current node. By using the prefix sum technique, we can determine the number of valid paths ending at the current node in constant time.

#### Code
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def pathSum(root: TreeNode, targetSum: int) -> int:
    def dfs(node, running_sum, target, pre_sum):
        if not node:
            return 0
        
        # Update the running sum
        running_sum += node.val
        
        # Calculate the number of paths that end at the current node and sum to target
        total_paths = pre_sum.get(running_sum - target, 0)
        
        # Record the current running sum in the prefix sum dictionary
        pre_sum[running_sum] = pre_sum.get(running_sum, 0) + 1
        
        # Recurse to the left and right children
        total_paths += dfs(node.left, running_sum, target, pre_sum)
        total_paths += dfs(node.right, running_sum, target, pre_sum)
        
        # After processing the node, remove the running sum from the prefix sum (backtrack)
        pre_sum[running_sum] -= 1
        
        return total_paths
    
    # Initialize the prefix sum dictionary with 0 sum to account for sum from root
    pre_sum = {0: 1}
    return dfs(root, 0, targetSum, pre_sum)
```
#### Complexity
- **Time Complexity:** O(N) - Each node is visited once, resulting in a linear time complexity.
- **Space Complexity:** O(N) - The space is used by the hash map storing prefix sums.

This approach provides a significant improvement in performance over the brute-force method by utilizing a more efficient data structure to track path sums.

