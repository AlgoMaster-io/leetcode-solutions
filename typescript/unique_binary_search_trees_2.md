# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
typescript
```typescript
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val !== undefined ? val : 0;
        this.left = left !== undefined ? left : null;
        this.right = right !== undefined ? right : null;
    }
}

function generateTrees(n: number): Array<TreeNode | null> {
    if (n === 0) return [];
    return generateTreesHelper(1, n);
}

function generateTreesHelper(start: number, end: number): Array<TreeNode | null> {
    const allTrees: Array<TreeNode | null> = [];
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
        for (let left of leftTrees) {
            for (let right of rightTrees) {
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
typescript
```typescript
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val !== undefined ? val : 0;
        this.left = left !== undefined ? left : null;
        this.right = right !== undefined ? right : null;
    }
}

class Solution {
    private memo: Map<string, Array<TreeNode | null>>;

    constructor() {
        this.memo = new Map<string, Array<TreeNode | null>>();
    }

    generateTrees(n: number): Array<TreeNode | null> {
        if (n === 0) return [];
        return this.generateTreesHelper(1, n);
    }

    private generateTreesHelper(start: number, end: number): Array<TreeNode | null> {
        const allTrees: Array<TreeNode | null> = [];
        if (start > end) {
            allTrees.push(null);
            return allTrees;
        }

        const memoKey = `${start}-${end}`;
        if (this.memo.has(memoKey)) {
            return this.memo.get(memoKey)!;
        }

        // Iterate through each number as the root
        for (let i = start; i <= end; i++) {
            // Generate all left and right subtrees
            const leftTrees = this.generateTreesHelper(start, i - 1);
            const rightTrees = this.generateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            for (let left of leftTrees) {
                for (let right of rightTrees) {
                    const currentTree = new TreeNode(i);
                    currentTree.left = left;
                    currentTree.right = right;
                    allTrees.push(currentTree);
                }
            }
        }

        this.memo.set(memoKey, allTrees);
        return allTrees;
    }
}
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

