# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)

class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = this.right = null;
    }
}

var minCameraCover = function(root) {
    let cameras = 0;
    
    const dfs = (node) => {
        if (!node) {
            return 1; // This node is covered
        }
        
        const left = dfs(node.left);
        const right = dfs(node.right);
        
        if (left === 0 || right === 0) {
            cameras++;
            return 2; // Placing a camera at this node
        }
        
        return (left === 2 || right === 2) ? 1 : 0;
        // 2 indicates child has a camera, 1 means this node is covered
        // 0 indicates this node needs a camera
    };
    
    return dfs(root) === 0 ? cameras + 1 : cameras;
};
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = this.right = null;
    }
}

var minCameraCover = function(root) {
    const HAS_CAMERA = 0, COVERED = 1, NEEDS_CAMERA = 2;
    let dp = new Map();
    
    const dfs = (node) => {
        if (!node) {
            return COVERED;
        }
        
        if (!dp.has(node)) {
            const state = new Array(3).fill(0);
            const left = dfs(node.left);
            const right = dfs(node.right);
            
            if (left === NEEDS_CAMERA || right === NEEDS_CAMERA) {
                state[HAS_CAMERA] = (dp.get(node) ? dp.get(node)[HAS_CAMERA] : 0) + 1;
                dp.set(node, state);
                return HAS_CAMERA;
            }

            if (left === HAS_CAMERA || right === HAS_CAMERA) {
                state[COVERED] = dp.get(node) ? dp.get(node)[COVERED] : 0;
                dp.set(node, state);
                return COVERED;
            }

            state[NEEDS_CAMERA] = dp.get(node) ? dp.get(node)[NEEDS_CAMERA] : 0;
            dp.set(node, state);
            return NEEDS_CAMERA;
        }

        return dp.get(node)[0];
    };
    
    return dfs(root) === NEEDS_CAMERA ? (dp.get(root) ? dp.get(root)[HAS_CAMERA] : 0) + 1 : (dp.get(root) ? dp.get(root)[HAS_CAMERA] : 0);
};
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.


