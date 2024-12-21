# [95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

## Recursive Approach

### Intuition
The idea is to generate all possible unique BSTs for numbers between 1 to n. We can recursively construct the trees by choosing each number as the root once and recursively form left and right subtrees using the numbers that are smaller and larger than the chosen root, respectively.

### Detailed Steps
1. Define a function `generateTrees(start, end)` that generates all BSTs using numbers from `start` to `end`.
2. The base case is if `start > end`, return `[null]` because there's no number to form a tree.
3. Initialize an empty list to store all trees.
4. Iterate over each number `i` from `start` to `end`:
   - Consider `i` as the root.
   - Generate all left subtrees using numbers smaller than `i`.
   - Generate all right subtrees using numbers larger than `i`.
   - Combine each left and right subtree with the root `i` to form a new tree.
5. Return the list of all possible trees for the range.

### Implementation

```javascript
function generateTrees(n) {
    if (n === 0) return [];
    
    // Helper function to generate all BSTs from start to end
    function generateTreesRange(start, end) {
        if (start > end) return [null]; // Base case
        
        const allTrees = []; // List to store all generated trees
        for (let i = start; i <= end; i++) { // i is chosen as root
            // Generate all left and right subtrees
            const leftTrees = generateTreesRange(start, i - 1);
            const rightTrees = generateTreesRange(i + 1, end);
            
            // Combine each left and right trees with root i
            for (let left of leftTrees) {
                for (let right of rightTrees) {
                    const root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    allTrees.push(root); // Add new tree to list
                }
            }
        }
        return allTrees;
    }
    
    return generateTreesRange(1, n); // Generate trees for values 1 to n
}
```

### Time Complexity
The time complexity is `O(C_n * n)` where `C_n` is the nth Catalan number. The generation of trees involves constructing each tree, and each tree construction is approximately linear in the number of nodes.

### Space Complexity
The space complexity is also `O(C_n * n)` for storing the list of trees.

## Dynamic Programming Approach

### Intuition
Using dynamic programming, we can store results to avoid repeated computation. We can use a 2D DP table where `dp[start][end]` stores all unique BSTs using numbers between `start` to `end`.

### Detailed Steps
1. Create a DP table `dp` where `dp[i][j]` keeps track of all unique BSTs that can be generated from numbers `i` to `j`.
2. Iterate `length` from 1 to `n`.
3. For each `start` from 1 to `n - length + 1`:
   - Determine the `end` as `start + length - 1`.
   - For each root `i` from `start` to `end`, compute all possible trees:
     - Combine trees from `dp[start][i-1]` and `dp[i+1][end]` as left and right subtrees, respectively.
4. Return `dp[1][n]`, which contains all unique BSTs for 1 to `n`.

### Implementation

```javascript
function generateTrees(n) {
    if (n === 0) return [];
    
    const dp = Array.from({ length: n + 1 }, () => Array(n + 1).fill([]));
    
    for (let length = 1; length <= n; length++) {
        for (let start = 1; start <= n - length + 1; start++) {
            const end = start + length - 1;
            const allTrees = [];
            
            for (let i = start; i <= end; i++) {
                const leftTrees = i > start ? dp[start][i - 1] : [null];
                const rightTrees = i < end ? dp[i + 1][end] : [null];
                
                for (let left of leftTrees) {
                    for (let right of rightTrees) {
                        const root = new TreeNode(i);
                        root.left = left;
                        root.right = right;
                        allTrees.push(root);
                    }
                }
            }
            
            dp[start][end] = allTrees;
        }
    }
    
    return dp[1][n];
}
```

### Time Complexity
The time complexity is reduced compared to the recursive approach by using memorization, making it more efficient, but still reliant on Catalan number growth.

### Space Complexity
The space complexity is `O(n^2 * C_n)` due to storing intermediate results in the DP table.

