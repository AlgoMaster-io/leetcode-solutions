# [LeetCode 437: Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix Sum Method (Optimized)](#approach-2-prefix-sum-method-optimized)

### Approach 1: Brute Force

#### Intuition
The problem is asking us to find the number of paths that sum to a given value, where the path can start from any node and go downwards, not necessarily rooted at the root. The brute force approach is to consider each node as a potential starting point, and try every possible path from that node going downwards. 

1. For each node, consider it as the start of a path.
2. Use a helper function to calculate all paths starting from the current node that sum up to the given sum.
3. Recursively apply the same logic for each node in the tree.

This approach, however straightforward, is not optimal as it carries an exponential time complexity since we are recalculating the sum paths repeatedly for overlapping nodes.

#### Code

```csharp
public class Solution {
    public int PathSum(TreeNode root, int targetSum) {
        if (root == null) 
            return 0;
        // Calculate paths from the current node
        int pathsFromRoot = CountPathsFromNode(root, targetSum);
        // Calculate paths from the left and right child subtrees
        int pathsFromLeft = PathSum(root.left, targetSum);
        int pathsFromRight = PathSum(root.right, targetSum);
        // Total paths are the current root paths plus left and right subtree paths
        return pathsFromRoot + pathsFromLeft + pathsFromRight;
    }
    
    private int CountPathsFromNode(TreeNode node, int targetSum) {
        if (node == null)
            return 0;
        int totalPaths = 0;
        // If current node value equals targetSum, we found a path
        if (node.val == targetSum)
            totalPaths++;
        // Continue to find paths in left and right subtrees with the updated targetSum
        totalPaths += CountPathsFromNode(node.left, targetSum - node.val);
        totalPaths += CountPathsFromNode(node.right, targetSum - node.val);
        return totalPaths;
    }
}
```

#### Time and Space Complexity
- **Time Complexity:** O(n^2) - For every node, we calculate path sum for all its descendants.
- **Space Complexity:** O(n) - Recursion stack space for the unbalanced tree.

### Approach 2: Prefix Sum Method (Optimized)

#### Intuition
To optimize, we can utilize a prefix sum technique, which allows us to consider each path ending at a node. By keeping track of the sum of all the node values from the root to the current node, we can deduce the presence of any path with the given sum by maintaining a hashmap to record whether there is a prefix with the necessary difference. 

1. Start from the root node and explore all paths using DFS.
2. As we move along the path, keep the running prefix sum in a hashmap.
3. The hashmap stores the number of times each prefix appears.
4. The current path sum minus the target will give a prefix that satisfies the path sum condition.
5. Using backtracking, ensure the current path sum is rolled back when moving up the recursion stack.

#### Code

```csharp
public class Solution {
    public int PathSum(TreeNode root, int sum) {
        if (root == null) return 0;
        // Dictionary to store prefix sums and their counts
        var prefixSumCount = new Dictionary<int, int>();
        // Initialize with 0 prefix sum to handle the path that equals to the target from root node directly
        prefixSumCount[0] = 1;
        // Start DFS traversal with initial sum of 0
        return DFS(root, 0, sum, prefixSumCount);
    }
    
    private int DFS(TreeNode node, int currentSum, int target, Dictionary<int, int> prefixSumCount) {
        if (node == null) return 0;
        
        // Update the current path sum by including the current node's value
        currentSum += node.val;
        // Calculate the number of valid paths ending at the current node
        int numPathsToCurrent = prefixSumCount.ContainsKey(currentSum - target) ? prefixSumCount[currentSum - target] : 0;
        // Add the current sum to the dictionary of prefix sums
        if (!prefixSumCount.ContainsKey(currentSum)) {
            prefixSumCount[currentSum] = 0;
        }
        prefixSumCount[currentSum]++;
        
        // Dive into the subtrees for further exploration
        int result = numPathsToCurrent + 
                     DFS(node.left, currentSum, target, prefixSumCount) +
                     DFS(node.right, currentSum, target, prefixSumCount);
        
        // After coming back from the dfs call, revert the changes we did
        prefixSumCount[currentSum]--;
        
        return result;
    }
}
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - We traverse each node exactly once.
- **Space Complexity:** O(n) - HashMap to store prefix sums, and recursion stack space for the unbalanced tree. 

By using this optimized approach, we greatly increase efficiency and drastically reduce unnecessary calculations compared to the brute force.

