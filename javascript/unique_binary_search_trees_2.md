# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
```javascript
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack

function TreeNode(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
}

var generateTrees = function(n) {
    if (n === 0) return [];
    return generateTreesHelper(1, n);
};

function generateTreesHelper(start, end) {
    const allTrees = [];
    if (start > end) {
        allTrees.push(null);
        return allTrees;
    }

    // Iterate through each number as the root
    for (let i = start; i <= end; i++) {
        // Generate all left and right subtrees
        const leftTrees = generateTreesHelper(start, i - 1);
        const rightTrees = generateTreesHelper(i + 1, end);

        // Combine left and right subtrees with the root i
        for (const left of leftTrees) {
            for (const right of rightTrees) {
                const currentTree = new TreeNode(i);
                currentTree.left = left;
                currentTree.right = right;
                allTrees.push(currentTree);
            }
        }
    }
    return allTrees;
}
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
```javascript
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table

function TreeNode(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
}

var generateTrees = function(n) {
    if (n === 0) return [];
    const memo = new Map();
    return generateTreesHelper(1, n, memo);
};

function generateTreesHelper(start, end, memo) {
    const allTrees = [];
    if (start > end) {
        allTrees.push(null);
        return allTrees;
    }

    const memoKey = `${start}-${end}`;
    if (memo.has(memoKey)) {
        return memo.get(memoKey);
    }

    // Iterate through each number as the root
    for (let i = start; i <= end; i++) {
        // Generate all left and right subtrees
        const leftTrees = generateTreesHelper(start, i - 1, memo);
        const rightTrees = generateTreesHelper(i + 1, end, memo);

        // Combine left and right subtrees with the root i
        for (const left of leftTrees) {
            for (const right of rightTrees) {
                const currentTree = new TreeNode(i);
                currentTree.left = left;
                currentTree.right = right;
                allTrees.push(currentTree);
            }
        }
    }

    memo.set(memoKey, allTrees);
    return allTrees;
}
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

