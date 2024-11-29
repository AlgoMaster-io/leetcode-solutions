# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
csharp
```csharp
// Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    public int PathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return 0; // Base case: empty tree
        }

        // Calculate paths including the current root and recurse for left and right subtrees
        return Dfs(root, targetSum) + PathSum(root.left, targetSum) + PathSum(root.right, targetSum);
    }

    private int Dfs(TreeNode node, int targetSum) {
        if (node == null) {
            return 0; // Base case: empty node
        }

        int count = 0;
        if (node.val == targetSum) {
            count++; // Path ends at the current node
        }

        // Check paths including left and right children
        count += Dfs(node.left, targetSum - node.val);
        count += Dfs(node.right, targetSum - node.val);

        return count;
    }
}
```

## Approach 2: Optimized Using Prefix Sum (Dictionary)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h) for recursion stack and O(n) for Dictionary storage
using System.Collections.Generic;

public class Solution {
    public int PathSum(TreeNode root, int targetSum) {
        var prefixSumMap = new Dictionary<int, int>();
        prefixSumMap[0] = 1; // Base case: one way to get sum 0
        return Dfs(root, 0, targetSum, prefixSumMap);
    }

    private int Dfs(TreeNode node, int currentSum, int targetSum, Dictionary<int, int> prefixSumMap) {
        if (node == null) {
            return 0; // Base case: empty node
        }

        currentSum += node.val;
        int paths = prefixSumMap.ContainsKey(currentSum - targetSum) ? prefixSumMap[currentSum - targetSum] : 0; // Count paths ending at this node

        // Update the prefix sum map for this node
        if (!prefixSumMap.ContainsKey(currentSum)) {
            prefixSumMap[currentSum] = 0;
        }
        prefixSumMap[currentSum]++;

        // Recurse for left and right children
        paths += Dfs(node.left, currentSum, targetSum, prefixSumMap);
        paths += Dfs(node.right, currentSum, targetSum, prefixSumMap);

        // Backtrack: remove the current node's contribution to the prefix sum
        prefixSumMap[currentSum]--;

        return paths;
    }
}
```

