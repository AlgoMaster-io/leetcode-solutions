## [Leetcode 437: Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### Approaches:
- [Brute Force: DFS on Each Node](#brute-force-dfs-on-each-node)
- [Optimized Approach with Prefix Sum](#optimized-approach-with-prefix-sum)

---

### Brute Force: DFS on Each Node

#### Intuition:
The brute force approach involves initiating a Depth-First Search (DFS) from each node in the binary tree, calculating the sum along different paths starting from each node, and counting those that sum up to the target value. This involves traversing each node multiple times, leading to a significant time complexity, especially in a larger tree.

#### Steps:
1. For each node, calculate the number of paths that sum up to the target value by using DFS.
2. Traverse every node in the tree and apply the DFS to calculate paths starting from that node.
3. Count the total paths that result in the target sum.

```typescript
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
    if (!root) {
        return 0;
    }
    
    // Calculate paths including the current root
    const pathsFromRoot = countPathsWithSum(root, targetSum);
    
    // Explore the rest of the tree (left and right subtrees)
    const pathsOnLeft = pathSum(root.left, targetSum);
    const pathsOnRight = pathSum(root.right, targetSum);
    
    return pathsFromRoot + pathsOnLeft + pathsOnRight;
}

function countPathsWithSum(node: TreeNode | null, targetSum: number): number {
    if (!node) {
        return 0;
    }
    
    let count = 0;

    // If current node value equals target sum, we found a path
    if (node.val === targetSum) {
        count++;
    }
    
    // Continue the search on the left and right with the updated target sum
    count += countPathsWithSum(node.left, targetSum - node.val);
    count += countPathsWithSum(node.right, targetSum - node.val);
    
    return count;
}
```

**Time Complexity:** O(n^2) in the average case, where n is the number of nodes (every node calls DFS that may traverse subtrees).

**Space Complexity:** O(n) in the worst case for tree depth.

---

### Optimized Approach with Prefix Sum

#### Intuition:
To optimize, track the sum of paths using a hash map (or object in TypeScript) to count occurrences of prefix sums. By doing so, we can compute the number of paths that lead to the target sum in constant time for each node. Instead of recalculating paths multiple times, we adjust our running total and check differences without re-calculating every single path.

#### Steps:
1. Use a map to keep track of all prefix sums seen so far and how often they occur.
2. For each node, calculate the current path sum.
3. The difference between the current path sum and the target indicates if there exists a sub-path that equals the target.
4. Update the map to include the current path sum.
5. Recursively continue for left and right children, then backtrack to remove the current path sum before leaving the node.

```typescript
function pathSumOptimized(root: TreeNode | null, targetSum: number): number {
    const prefixSumMap: Record<number, number> = {};
    prefixSumMap[0] = 1; // Base case: One way to have a sum of zero (with no nodes).
    return dfs(root, 0, targetSum, prefixSumMap);
}

function dfs(node: TreeNode | null, currentSum: number, targetSum: number, prefixSumMap: Record<number, number>): number {
    if (!node) {
        return 0;
    }

    // Include the current node's value in the current running sum
    currentSum += node.val;

    // Calculate how many paths along the current path add up to the target sum
    const targetPathSum = currentSum - targetSum;
    let pathsCount = prefixSumMap[targetPathSum] || 0;

    // Add the current sum to the map of prefix sums
    prefixSumMap[currentSum] = (prefixSumMap[currentSum] || 0) + 1;

    // Explore further
    pathsCount += dfs(node.left, currentSum, targetSum, prefixSumMap);
    pathsCount += dfs(node.right, currentSum, targetSum, prefixSumMap);

    // Backtrack: Remove the current path sum to avoid affecting other paths
    prefixSumMap[currentSum]--;

    return pathsCount;
}
```

**Time Complexity:** O(n), where n is the number of nodes, because we visit each node once.

**Space Complexity:** O(n) due to the hash map storing sum counts and the recursion stack in the worst-case scenario (skewed tree).

---

