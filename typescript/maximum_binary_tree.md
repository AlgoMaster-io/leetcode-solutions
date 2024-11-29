# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approach 1: Recursive Construction

### Solution

typescript
```typescript
// Time Complexity: O(n^2) in the worst case (unbalanced tree) and O(n log n) on average
// Space Complexity: O(n) (due to recursion stack)

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val: number, left: TreeNode | null = null, right: TreeNode | null = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    constructMaximumBinaryTree(nums: number[]): TreeNode | null {
        return this.buildTree(nums, 0, nums.length - 1);
    }

    private buildTree(nums: number[], left: number, right: number): TreeNode | null {
        if (left > right) {
            return null; // Base case: no elements in the range
        }

        // Find the index of the maximum element in the current range
        let maxIndex = left;
        for (let i = left + 1; i <= right; i++) {
            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }

        // Create the root node with the maximum value
        const root = new TreeNode(nums[maxIndex]);

        // Recursively construct the left and right subtrees
        root.left = this.buildTree(nums, left, maxIndex - 1);
        root.right = this.buildTree(nums, maxIndex + 1, right);

        return root;
    }
}
```

## Approach 2: Monotonic Stack (Optimal)

### Solution

typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val: number, left: TreeNode | null = null, right: TreeNode | null = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    constructMaximumBinaryTree(nums: number[]): TreeNode | null {
        const stack: TreeNode[] = [];

        for (const num of nums) {
            const current = new TreeNode(num);

            // Maintain the stack such that the current node is the parent
            // of the last node in the stack if it has a smaller value
            while (stack.length > 0 && stack[stack.length - 1].val < num) {
                current.left = stack.pop()!;
            }

            // The last node in the stack becomes the right child of the current node
            if (stack.length > 0) {
                stack[stack.length - 1].right = current;
            }

            // Push the current node onto the stack
            stack.push(current);
        }

        // The root of the tree is the bottom-most node in the stack
        return stack[0];
    }
}
```

