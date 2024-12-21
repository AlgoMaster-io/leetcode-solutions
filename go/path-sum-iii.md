### [Path Sum III - LeetCode](https://leetcode.com/problems/path-sum-iii/)

#### Approaches:
- [Approach 1: Brute Force with Recursion](#approach-1-brute-force-with-recursion)
- [Approach 2: Prefix Sum Optimization](#approach-2-prefix-sum-optimization)

---

## Approach 1: Brute Force with Recursion

### Intuition
The brute force approach to solving this problem is to consider each node as the starting point of a path and then check all the paths that start from it. For each node, we check whether any path starting from this node sums up to the target value.

### Steps
1. Define a recursive function `countPaths` that will count paths that sum up to the given target starting from the current node.
2. At each node, check if the node itself satisfies the target sum. If so, count it. Then, for each child of the current node, subtract the node's value from the target and repeat.
3. Check paths starting from every node in the tree using the helper method `countPaths`.
4. Recurse through the tree using `pathSum` method which calls `countPaths` at each node as the starting node.

### Code
```go
// TreeNode represents a node in the binary tree.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Returns the number of paths that sum up to target starting from the given node.
func countPaths(node *TreeNode, target int) int {
    if node == nil {
        return 0
    }

    // Check if the current node completes a path with the required sum.
    count := 0
    if node.Val == target {
        count = 1
    }

    // Account for paths from left and right child.
    count += countPaths(node.Left, target-node.Val)
    count += countPaths(node.Right, target-node.Val)

    return count
}

// Main method to count paths in the entire tree.
func pathSum(root *TreeNode, target int) int {
    if root == nil {
        return 0
    }

    // Count paths with current root, and separately from left and right subtrees.
    return countPaths(root, target) + pathSum(root.Left, target) + pathSum(root.Right, target)
}
```

### Complexity
- **Time Complexity:** \(O(n^2)\) in the worst case where `n` is the number of nodes. For each node, we might end up visiting all other nodes in its subtree.
- **Space Complexity:** \(O(n)\) in the worst case due to the recursion stack.

---

## Approach 2: Prefix Sum Optimization

### Intuition
To optimize, we can use the prefix sum technique combined with a hash map. The idea is to use a map to store the cumulative sum up to each node and see if there exists a prior prefix sum such that the difference equals the target sum.

### Steps
1. Utilize a recursive function `helper` passing the current node, the cumulative sum up to the current node, and a map to count the occurrences of each prefix sum.
2. For each node, calculate the cumulative sum from the root to the current node.
3. Check if there is a prefix sum that, when deducted from the current cumulative sum, equals the target sum. This implies that some path leading up to the current node sums to the target.
4. Recurse for left and right children, updating the current path sum.
5. Ensure the map state reflects the current state before and after entering the recursion for each node, handling the counts of seen prefix sums accurately.

### Code
```go
// Utility function employs prefix sum optimization.
func pathSumOptimized(root *TreeNode, targetSum int) int {
    prefixSumMap := map[int]int{0: 1}
    return helper(root, 0, targetSum, prefixSumMap)
}

// Helper function for managing recursive calls and prefix sum logic.
func helper(node *TreeNode, currentSum, target int, prefixSumMap map[int]int) int {
    if node == nil {
        return 0
    }

    // Update the current path sum.
    currentSum += node.Val
    count := 0

    // Check if there is a prefix sum that leads to the target sum.
    count += prefixSumMap[currentSum-target]

    // Update the map with the new prefix sum.
    prefixSumMap[currentSum] += 1

    // Recurse through the left and right children.
    count += helper(node.Left, currentSum, target, prefixSumMap)
    count += helper(node.Right, currentSum, target, prefixSumMap)

    // Remove current node's sum from the prefix sum count to avoid affecting other nodes.
    prefixSumMap[currentSum] -= 1

    return count
}
```

### Complexity
- **Time Complexity:** \(O(n)\), where `n` is the number of nodes since each node is processed only once.
- **Space Complexity:** \(O(h)\), for the hash map and recursion stack, where `h` is the height of the tree. In the worst case, `h` can be `n`.

