# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Solutions
- [Approach 1: Greedy Postorder Traversal](#approach-1-greedy-postorder-traversal)

### Approach 1: Greedy Postorder Traversal

#### Intuition
The problem aims to place the minimum number of cameras such that every node in the binary tree is monitored. A node can be monitored if:
- It has a camera.
- It is a child of a node with a camera.
- It is a parent of a node with a camera.

By using a postorder traversal (left, right, root) strategy, we can decide optimally where to place cameras as we consider each node's children before the node itself. This enables us to make greedy decisions where cameras are placed at the lowest depth first (if required), reducing the total number of cameras needed.

The idea is to use a state-based approach:
- A leaf node will force a camera to be put at its parent.
- If a node is not covered by a camera, it must trigger installing a camera on the node itself to cover itself and its direct children.

To facilitate the explanation, we consider three states for a node during a traversal:
1. **State 0:** Node needs a camera.
2. **State 1:** Node is covered without a camera.
3. **State 2:** Node has a camera.

Let's define these states as we perform a postorder traversal:
- If a node's child indicates a need for a camera (`State 0`), put a camera on the current node (`State 2`).
- If any child is covered and does not need a camera (`State 1`), this node needs no camera (`State 1`).
- If both children are covered, and neither needs a camera, mark this node as needing a camera (`State 0`).

#### Code
```javascript
var minCameraCover = function(root) {
    // Define states for the nodes
    const NEED_CAMERA = 0;
    const COVERED = 1;
    const HAS_CAMERA = 2;
    
    let cameras = 0;

    // Postorder traversal helper function
    const postorderTraversal = function(node) {
        // If there's no node, it doesn't need a camera
        if (node === null) {
            return COVERED;
        }
        
        const leftState = postorderTraversal(node.left);
        const rightState = postorderTraversal(node.right);

        // If either child needs a camera, the current node must have a camera
        if (leftState === NEED_CAMERA || rightState === NEED_CAMERA) {
            cameras++;
            return HAS_CAMERA;
        }
        
        // If any child has a camera, the current node is covered
        if (leftState === HAS_CAMERA || rightState === HAS_CAMERA) {
            return COVERED;
        }
        
        // When children are both covered but don't need a camera,
        // it means the current node is currently not covered
        return NEED_CAMERA;
    };

    // If the root itself needs a camera at the end, increment cameras
    if (postorderTraversal(root) === NEED_CAMERA) {
        cameras++;
    }

    return cameras;
};
```

#### Time Complexity
- The time complexity is O(n), where n is the number of nodes in the tree, since we visit each node only once in postorder traversal.

#### Space Complexity
- The space complexity is O(h), where h is the height of the tree due to the recursion stack in a postorder traversal approach. In the worst case, for a skewed tree, this can be O(n).

In conclusion, this approach efficiently covers all nodes using a minimum number of cameras by leveraging a greedy strategy via postorder traversal.

