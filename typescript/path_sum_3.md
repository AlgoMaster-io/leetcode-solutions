# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
typescript
```typescript
// Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function pathSum(root: TreeNode | null, targetSum: number): number {
    if (root === null) {
        return 0; // Base case: empty tree
    }

    // Calculate paths including the current root and recurse for left and right subtrees
    return dfs(root, targetSum) + pathSum(root.left, targetSum) + pathSum(root.right, targetSum);
}

function dfs(node: TreeNode | null, targetSum: number): number {
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
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h) for recursion stack and O(n) for HashMap storage

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function pathSum(root: TreeNode | null, targetSum: number): number {
    const prefixSumMap: Map<number, number> = new Map();
    prefixSumMap.set(0, 1); // Base case: one way to get sum 0
    return dfs(root, 0, targetSum, prefixSumMap);
}

function dfs(node: TreeNode | null, currentSum: number, targetSum: number, prefixSumMap: Map<number, number>): number {
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
    prefixSumMap.set(currentSum, (prefixSumMap.get(currentSum) || 0) - 1);

    return paths;
}
```

