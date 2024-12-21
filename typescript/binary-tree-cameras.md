# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches:
1. [Recursive Approach with Basic Intuition](#recursive-approach-with-basic-intuition)
2. [Greedy Approach with DFS Optimization](#greedy-approach-with-dfs-optimization)

### Recursive Approach with Basic Intuition

In this approach, we will use a basic recursive strategy to decide where to place cameras. The goal is to ensure that each node in the tree is monitored, either directly by a camera or indirectly by being in proximity to a camera.

**Intuition:**
- We can think about the problem using a postorder traversal, where we first evaluate the children before the parent.
- A node will have three states:
  1. It has a camera.
  2. It is covered but doesn't have a camera.
  3. It is not covered.
  
The overall idea is to determine the minimum number of cameras needed to monitor all nodes by evaluating these states via a recursive function.

**Steps:**
1. Use a recursive helper function to traverse the tree in a postorder manner.
2. At each node, decide whether a camera is needed based on the states of its children.
3. Return the state of the current node and update the camera count accordingly.

**Time Complexity:** O(N) where N is the number of nodes in the tree because we visit each node once.

**Space Complexity:** O(H) where H is the height of the tree due to the recursion stack.

```typescript
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

function minCameraCover(root: TreeNode | null): number {
    let cameras = 0;
    
    function postorder(node: TreeNode | null): number {
        if (node === null) return 2;

        const left = postorder(node.left);
        const right = postorder(node.right);

        // If either of the child nodes is not covered, place a camera here
        if (left === 0 || right === 0) {
            cameras++;
            return 1;
        }
        
        // If either child has a camera, the current node is covered
        if (left === 1 || right === 1) {
            return 2;
        }
        
        // If both children are covered but do not have a camera, the current node is not covered
        return 0;
    }
    
    // If root is not covered after initial traversal, place a camera at the root
    if (postorder(root) === 0) {
        cameras++;
    }
    
    return cameras;
}
```

### Greedy Approach with DFS Optimization

**Intuition:**
- A more optimal approach is to take a greedy strategy where we make the decision of placing cameras by peeking at the parent's need for coverage after both children states are evaluated.
- Again, the tree traversal is postorder, but this time we optimize the decision-making process by ensuring that we minimize the number of cameras used by taking advantage of both children states.

**Steps:**
1. Use the same recursive traversal but optimize the decision by assuming that if both children are covered without cameras, then a camera might be needed at the parent.
2. Similar to previous logic but ensure that leaf nodes that are not covered prompt camera placement efficiently.

**Time Complexity:** O(N) since each node is visited once.

**Space Complexity:** O(H), which is space required for the recursion stack. 

```typescript
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

function minCameraCover(root: TreeNode | null): number {
    let cameraCount = 0;

    function dfs(node: TreeNode | null): number {
        if (node === null) return 1; // Consider null nodes as covered without cameras

        let left = dfs(node.left);
        let right = dfs(node.right);

        if (left === 0 || right === 0) {
            // If either child needs camera, place camera at this node
            cameraCount++;
            return 2; // Node has a camera
        } else if (left === 2 || right === 2) {
            // If any child has a camera, this node is covered
            return 1;
        } else {
            // This node is not covered when both children are covered without cameras
            return 0;
        }
    }

    // Cover the root if needed
    if (dfs(root) === 0) {
        cameraCount++;
    }

    return cameraCount;
}
```

You may try one approach or another based on the tree structure to optimize camera placement. The key is to analyze nodes that are directly observed vs. indirectly observed and make decisions to minimize the camera count globally.

