# [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approaches:
- [Approach 1: Recursion (DFS)](#approach-1-recursion-dfs)
- [Approach 2: Recursion with prefix sum and hashmap](#approach-2-recursion-with-prefix-sum-and-hashmap)

### Approach 1: Recursion (DFS)

#### Intuition:
The problem is asking us to find the number of paths that sum to a given value where the path can start and end anywhere in the binary tree. A simple solution is to do a depth-first search (DFS) on each node and check for path sums that start from every node in the tree.

For each node, we initiate a DFS to look for paths originating from that node that sum up to the target sum. The recursive function continues to search along each possible path while subtracting the current node's value from the target sum.

#### Steps:
1. If a node is `null`, return 0 as there can't be any paths.
2. Calculate the number of valid paths starting from the node (calling `dfs` method).
3. Recursively find paths in the left and right subtrees.
4. Return the total number of paths found.

```javascript
function pathSum(root, sum) {
    if (root === null) return 0;

    function dfs(node, remainingSum) {
        if (node === null) return 0;

        let count = 0;
        if (node.val === remainingSum) count++;

        count += dfs(node.left, remainingSum - node.val);
        count += dfs(node.right, remainingSum - node.val);

        return count;
    }

    return dfs(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
}
```

- **Complexity Analysis:**
  - **Time Complexity:** \(O(n^2)\) in the worst case, where \(n\) is the number of nodes since for each node, we might traverse up to half of the nodes in the tree.
  - **Space Complexity:** \(O(h)\), where \(h\) is the height of the tree due to the recursion stack.

### Approach 2: Recursion with prefix sum and hashmap

#### Intuition:
In this approach, we can use a hashmap to store the cumulative sum up to any node. By keeping track of the sum up to each node, any subtree's path can be calculated by finding the difference between the cumulative sum up to the node of interest and any prior node's cumulative sum. 

The key here is to utilize the property:
- `currentSum - targetSum` gives us the sum we need to look for in the hashmap. If this sum has been seen, then there exists a path from the earlier node to the current node with the `targetSum`.

#### Steps:
1. Use a hashmap to store the cumulative sum at each node and its frequency.
2. Perform DFS traversing through the tree.
3. For each node, calculate the current cumulative sum and update the hashmap.
4. Check if `(currentSum - targetSum)` exists in the hashmap.
5. Recur to both left and right children.
6. After visiting children, remove the current node's path from the hashmap to prevent affecting other paths.

```javascript
function pathSum(root, targetSum) {
    const prefixSumCount = new Map();
    prefixSumCount.set(0, 1);  // Base case: a single node path that sums to targetSum

    function dfs(node, currentSum) {
        if (node === null) return 0;

        currentSum += node.val;
        let count = prefixSumCount.get(currentSum - targetSum) || 0;

        // Record the current path sum into the map with updated frequency
        prefixSumCount.set(currentSum, (prefixSumCount.get(currentSum) || 0) + 1);

        count += dfs(node.left, currentSum);
        count += dfs(node.right, currentSum);

        // Remove the current path sum since we are done exploring that path
        prefixSumCount.set(currentSum, prefixSumCount.get(currentSum) - 1);
        
        return count;
    }

    return dfs(root, 0);
}
```

- **Complexity Analysis:**
  - **Time Complexity:** \(O(n)\) since we visit each node once.
  - **Space Complexity:** \(O(h)\) or \(O(n)\) in the worst case with the recursion stack. The space for the hashmap is limited to \(O(n)\) but in practice much lesser depending on the path sums.

