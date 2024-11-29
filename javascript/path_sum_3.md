# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
javascript
```javascript
// Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

function pathSum(root, targetSum) {
    if (root === null) {
        return 0; // Base case: empty tree
    }

    // Calculate paths including the current root and recurse for left and right subtrees
    return dfs(root, targetSum) + pathSum(root.left, targetSum) + pathSum(root.right, targetSum);
};

function dfs(node, targetSum) {
    if (node === null) {
        return 0; // Base case: empty node
    }

    let count = 0;
    if (node.val === targetSum) {
        count++; // Path ends at the current node
    }

    // Check paths including left and right children
    count += dfs(node.left, targetSum - node.val);
    count += dfs(node.right, targetSum - node.val);

    return count;
}
```

## Approach 2: Optimized Using Prefix Sum (HashMap)

### Solution
javascript
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h) for recursion stack and O(n) for HashMap storage

function pathSum(root, targetSum) {
    const prefixSumMap = new Map();
    prefixSumMap.set(0, 1); // Base case: one way to get sum 0
    return dfs(root, 0, targetSum, prefixSumMap);
};

function dfs(node, currentSum, targetSum, prefixSumMap) {
    if (node === null) {
        return 0; // Base case: empty node
    }

    currentSum += node.val;
    let paths = prefixSumMap.get(currentSum - targetSum) || 0; // Count paths ending at this node

    // Update the prefix sum map for this node
    prefixSumMap.set(currentSum, (prefixSumMap.get(currentSum) || 0) + 1);

    // Recurse for left and right children
    paths += dfs(node.left, currentSum, targetSum, prefixSumMap);
    paths += dfs(node.right, currentSum, targetSum, prefixSumMap);

    // Backtrack: remove the current node's contribution to the prefix sum
    prefixSumMap.set(currentSum, prefixSumMap.get(currentSum) - 1);

    return paths;
}
```

