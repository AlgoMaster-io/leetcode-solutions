# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```typescript
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

class Solution {
    private cameras: number = 0;
    
    minCameraCover(root: TreeNode | null): number {
        // If root returns 0, it means it needs a camera
        return this.dfs(root) === 0 ? this.cameras + 1 : this.cameras;
    }
    
    // Helper function for recursive traversal
    private dfs(node: TreeNode | null): number {
        if (node === null) {
            return 1; // This node is covered
        }
        
        const left: number = this.dfs(node.left);
        const right: number = this.dfs(node.right);
        
        if (left === 0 || right === 0) {
            this.cameras++;
            return 2; // Placing a camera at this node
        }
        
        return (left === 2 || right === 2) ? 1 : 0;
        // 2 indicates child has a camera, 1 means this node is covered
        // 0 indicates this node needs a camera
    }
}
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

class Solution {
    private static readonly HAS_CAMERA = 0;
    private static readonly COVERED = 1;
    private static readonly NEEDS_CAMERA = 2;
    
    minCameraCover(root: TreeNode | null): number {
        const dp = new Map<TreeNode | null, number[]>();
        
        return this.dfs(root, dp) === Solution.NEEDS_CAMERA ? dp.get(root)![Solution.HAS_CAMERA] + 1 : dp.get(root)![Solution.HAS_CAMERA];
    }
    
    private dfs(node: TreeNode | null, dp: Map<TreeNode | null, number[]>): number {
        if (node === null) {
            return Solution.COVERED;
        }
        
        if (!dp.has(node)) {
            const state = [0, 0, 0];

            const left = this.dfs(node.left, dp);
            const right = this.dfs(node.right, dp);
            
            if (left === Solution.NEEDS_CAMERA || right === Solution.NEEDS_CAMERA) {
                state[Solution.HAS_CAMERA] = (dp.get(node) ?? [0, 0, 0])[Solution.HAS_CAMERA] + 1;
                dp.set(node, state);
                return Solution.HAS_CAMERA;
            }

            if (left === Solution.HAS_CAMERA || right === Solution.HAS_CAMERA) {
                state[Solution.COVERED] = (dp.get(node) ?? [0, 0, 0])[Solution.COVERED];
                dp.set(node, state);
                return Solution.COVERED;
            }

            state[Solution.NEEDS_CAMERA] = (dp.get(node) ?? [0, 0, 0])[Solution.NEEDS_CAMERA];
            dp.set(node, state);
            return Solution.NEEDS_CAMERA;
        }

        return dp.get(node)![0];
    }
}
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.

