# [Leetcode 437: Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sum Approach](#prefix-sum-approach)

### Brute Force Approach

#### Intuition:
The brute force approach involves iterating over each node and trying to find all paths starting from each node that sum up to the given `targetSum`. For each node, we perform a DFS traversal while counting paths that result in the `targetSum`.

#### Steps:
1. For each node, you calculate path sums starting from the node itself.
2. Use a helper function to recursively calculate path sums from the current node.
3. Use another function to iterate over each node and treat it as a starting point.

#### Java Code:
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    // Main function to call to count paths summing to targetSum
    public int pathSum(TreeNode root, int targetSum) {
        if (root == null) return 0;

        // Count paths starting from the current node
        int countFromRoot = pathSumFrom(root, targetSum);
        
        // Recursively check for paths starting from the left and right children
        int countLeft = pathSum(root.left, targetSum);
        int countRight = pathSum(root.right, targetSum);
        
        // Return total count of paths from current node, left child, and right child
        return countFromRoot + countLeft + countRight;
    }
    
    // Helper function to count paths with sum equal to target from given node
    private int pathSumFrom(TreeNode node, int targetSum) {
        if (node == null) return 0;
        
        int count = 0;
        
        // Check if the current node's value is equal to targetSum
        if (node.val == targetSum) count++;
        
        // Continue checking for sum with the left child and right child
        count += pathSumFrom(node.left, targetSum - node.val);
        count += pathSumFrom(node.right, targetSum - node.val);
        
        return count;
    }
}
```

#### Time Complexity:
- \(O(n^2)\) where \(n\) is the number of nodes in the tree. In the worst case, each node is visited \(n\) times.

#### Space Complexity:
- \(O(n)\) for the recursion stack in the worst case if the tree is completely unbalanced.

### Prefix Sum Approach

#### Intuition:
This optimized approach uses a hashmap to store the prefix sum frequencies encountered so far while traversing the tree. The idea is to leverage the prefix sum technique to quickly find if there exists a subpath ending in the current node that sums up to the `targetSum`.

#### Steps:
1. Use a recursive function to traverse the tree.
2. For each node, calculate its prefix sum.
3. Use a hashmap to check whether there exists a prefix sum such that removing it from the current prefix sum would give us the `targetSum`.
4. Update the prefix sum count in the hashmap while going deeper into the recursion and undo changes when backtracking.

#### Java Code:
```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefixSumCount = new HashMap<>();
        prefixSumCount.put(0L, 1);  // Initialize with base case
        return dfs(root, 0, targetSum, prefixSumCount);
    }

    private int dfs(TreeNode node, long currentSum, int targetSum, Map<Long, Integer> prefixSumCount) {
        if (node == null) return 0;
        
        // Update the current prefix sum
        currentSum += node.val;
        
        // Calculate the number of valid paths ending at the current node
        int numPathsToCurrent = prefixSumCount.getOrDefault(currentSum - targetSum, 0);
        
        // Update the map for the current prefix sum
        prefixSumCount.put(currentSum, prefixSumCount.getOrDefault(currentSum, 0) + 1);
        
        // Recur for left and right subtrees and accumulate results
        int result = numPathsToCurrent + 
                     dfs(node.left, currentSum, targetSum, prefixSumCount) +
                     dfs(node.right, currentSum, targetSum, prefixSumCount);
        
        // Backtrack to ensure the prefixSumCount is correctly maintained
        prefixSumCount.put(currentSum, prefixSumCount.get(currentSum) - 1);
        
        return result;
    }
}
```

#### Time Complexity:
- \(O(n)\) since each node is visited once.

#### Space Complexity:
- \(O(n)\) for the hashmap storing the prefix sums and for the recursion stack in the worst case.

