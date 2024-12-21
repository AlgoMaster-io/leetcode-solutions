# [Leetcode 124: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approaches:

1. [Recursive Depth First Search with Global Variable](#approach-1)
2. [Optimized Recursive DFS with Global Variable and Local Calculation](#approach-2)

### Approach 1: Recursive Depth First Search with Global Variable

#### Intuition:
The problem requires us to find the maximum path sum for a given binary tree where the path can start and end at any node. A naive approach is to consider every possible path in the tree and calculate their sums, however, this approach is not feasible due to the exponential number of paths. Instead, we can use a recursive approach that calculates the maximum path sum by considering each node as the potential root of the maximum path. 

At each node, we will:
1. Calculate the maximum path sum **through** the left child.
2. Calculate the maximum path sum **through** the right child.
3. Update the global maximum path sum considering the path through the node and both of its children.

#### Steps:
1. Use a helper function with depth-first search (DFS) that returns the maximum path sum starting from the given node.
2. For each node, calculate the path sum from the left and right children.
3. Calculate the maximum sum that can be obtained by splitting at the current node.
4. Update the global maximum path sum if the current split gives a higher value.
5. Return the maximum gain if we continue from the current node upwards.

```javascript
var maxPathSum = function(root) {
    let maxSum = -Infinity; // Global variable to store the maximum path sum
    
    function maxGain(node) {
        if (!node) return 0; // Base case: If node is null, max gain is 0

        // Recursive case: Calculate maximum gain from left and right subtrees
        let leftGain = Math.max(maxGain(node.left), 0);
        let rightGain = Math.max(maxGain(node.right), 0);
        
        // Check the maximum possible path using the current node
        let currentPathSum = node.val + leftGain + rightGain;

        // Update global max, if needed
        maxSum = Math.max(maxSum, currentPathSum);

        // Return maximum gain if we continue from this node upwards
        return node.val + Math.max(leftGain, rightGain);
    }

    maxGain(root); // Execute the DFS starting from root
    return maxSum; // Return the result stored in maxSum
};
```

- **Time Complexity:** O(N), where N is the number of nodes. We traverse each node once.
- **Space Complexity:** O(H), where H is the height of the tree. This space is occupied by the recursion stack.

### Approach 2: Optimized Recursive DFS with Global Variable and Local Calculation

#### Intuition:
This solution is an optimized version of the first approach. The key insight is that for each node, we only need to consider the maximum path sum starting from its left or right child to calculate the potential path sum split at that node. This approach efficiently combines the recursive logic with computation to ensure that the solution is derived in a single traversal of the tree.

#### Steps:
1. Just as in approach 1, employ a recursive function to traverse the tree in a depth-first manner.
2. At each node, calculate left and right max gains recursively, ensuring non-negative contribution.
3. For each node, update the global maximum path sum by evaluating the node itself, considering both children as part of the path.
4. Finally, return the computed maximum path sum.

```javascript
var maxPathSum = function(root) {
    let maxSum = Number.MIN_SAFE_INTEGER; // Initial global max

    function maxGain(node) {
        if (!node) return 0; // Base case

        // Left and right maximum path sums
        let leftGain = Math.max(maxGain(node.left), 0);
        let rightGain = Math.max(maxGain(node.right), 0);

        // Max path via current node
        let currentPath = node.val + leftGain + rightGain;
        
        // Update global max if necessary
        maxSum = Math.max(maxSum, currentPath);

        // Return max gain if continuing from current node
        return node.val + Math.max(leftGain, rightGain);
    }

    maxGain(root);
    return maxSum;
};
```

- **Time Complexity:** O(N), since each node is processed exactly once.
- **Space Complexity:** O(H), with H being the maximum height of the tree due to recursion stack usage. 

This optimized approach is highly efficient, ideal for handling large binary trees and guarantees the correct result through a single pass of the entire tree structure.

