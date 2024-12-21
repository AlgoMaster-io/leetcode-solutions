# [Leetcode 95: Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)

### Recursive Approach

When trying to solve this problem, notice that every number `i` can be the root of the tree. Once you pick `i` as a root, the numbers less than `i` can be part of the left subtree and numbers greater than `i` can be part of the right subtree. Recursively generate all combinations of left and right subtrees and pair them to form a full tree for each root.

#### Intuition:

- Every number from 1 to `n` can be the root of a binary search tree.
- For a root `i`, the numbers `1` to `i-1` can form the left subtree.
- The numbers `i+1` to `n` can form the right subtree.
- Recursively generate left and right subtrees for each root and combine them.

```typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function generateTrees(n: number): TreeNode[] {
    if (n === 0) return [];

    // Function to generate all trees from start to end
    const generate = (start: number, end: number): TreeNode[] => {
        if (start > end) return [null];
        
        const trees: TreeNode[] = [];
        
        // Pick `i` as current root
        for (let i = start; i <= end; i++) {
            // Recursively generate all left subtrees for numbers less than `i`
            const leftTrees = generate(start, i - 1);
            // Recursively generate all right subtrees for numbers greater than `i`
            const rightTrees = generate(i + 1, end);
            
            // Combine each left and right subtree with root `i`
            for (let left of leftTrees) {
                for (let right of rightTrees) {
                    const root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    trees.push(root);
                }
            }
        }
        
        return trees;
    };
    
    return generate(1, n);
}
```

**Time Complexity**: O(n*4^n/√n), as this problem deals with Catalan numbers.

**Space Complexity**: O(4^n/√n) due to the recursion stack and result storage.


### Dynamic Programming with Memoization

To optimize the recursive approach, use memoization to cache results of subproblems, so the function does not recompute trees for the same start and end values repeatedly.

#### Intuition:

- Use a hashmap to store the computed unique trees for every pair `(start, end)`.
- If the trees for the current pair `(start, end)` are already calculated, return the cached result before trying to calculate again.

```typescript
function generateTreesMemoization(n: number): TreeNode[] {
    if (n === 0) return [];

    // Cache for storing result of subproblems
    const memo: Map<string, TreeNode[]> = new Map();

    const generate = (start: number, end: number): TreeNode[] => {
        // Unique key for the current range
        const key = `${start},${end}`;
        if (memo.has(key)) return memo.get(key)!;
        
        if (start > end) return [null];
        
        let trees: TreeNode[] = [];
        
        // Pick `i` as current root
        for (let i = start; i <= end; i++) {
            const leftTrees = generate(start, i - 1);
            const rightTrees = generate(i + 1, end);
            
            for (let left of leftTrees) {
                for (let right of rightTrees) {
                    const root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    trees.push(root);
                }
            }
        }
        
        // Store in memo
        memo.set(key, trees);
        return trees;
    };
    
    return generate(1, n);
}
```

**Time Complexity**: O(n*4^n/√n), due to Catalan numbers influence, but memoization helps avoid recalculations.

**Space Complexity**: O(4^n/√n) due to caching subproblem trees, besides the recursion stack.

