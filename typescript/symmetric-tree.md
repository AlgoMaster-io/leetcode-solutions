# [Leetcode Problem 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

## Solutions

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Queue](#iterative-approach-using-queue)

---

## Recursive Approach

The recursive approach is the most intuitive and straightforward way to determine if a binary tree is symmetric. The idea is to check if the left subtree is a mirror of the right subtree.

### Intuition:

1. A tree is considered symmetric if the left and right subtrees are mirrors of each other.
2. To check if two trees are mirrors:
   - They must have the same root value.
   - The left subtree of each tree should be a mirror of the right subtree of the other tree.
   
3. Begin by checking the left and right children of the root. If they match, their children must also be mirrors of each other.

The recurrence involves comparing nodes: `isMirror(a, b)`, where `a`'s left is checked with `b`'s right and `a`'s right with `b`'s left. This naturally leads to a recursion.

### Typescript Solution:

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

function isSymmetric(root: TreeNode | null): boolean {
    if (!root) return true;

    const isMirror = (t1: TreeNode | null, t2: TreeNode | null): boolean => {
        if (!t1 && !t2) return true; // Both nodes are null
        if (!t1 || !t2) return false; // One node is null, the other isn't

        // Both nodes are non-null, compare values and recursive check
        return (t1.val === t2.val) &&
            isMirror(t1.left, t2.right) &&
            isMirror(t1.right, t2.left);
    };

    return isMirror(root.left, root.right);
}
```

### Time and Space Complexity:
- **Time Complexity**: O(n), where n is the total number of nodes in the binary tree. Every node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree, corresponding to the call stack space in the recursion.

---

## Iterative Approach Using Queue

This approach leverages a queue to simulate the stack frames of the recursive approach. By handling nodes pairwise, we simulate the mirror checking iteratively.

### Intuition:

1. Use a queue to hold pairs of nodes to be compared for symmetry.
2. Start by enqueueing the root's left and right child.
3. While the queue is not empty:
   - Dequeue a pair of nodes.
   - If both nodes are null, continue.
   - If one of the nodes is null or their values don't match, the tree isn't symmetric.
   - Enqueue the children of the dequeued nodes in mirrored positions:
     - Left child of the first node with the right child of the second node.
     - Right child of the first node with the left child of the second node.

### Typescript Solution:

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

function isSymmetric(root: TreeNode | null): boolean {
    if (!root) return true;

    const queue: (TreeNode | null)[] = [root.left, root.right];

    while (queue.length > 0) {
        const t1 = queue.shift();
        const t2 = queue.shift();

        if (!t1 && !t2) continue;
        if (!t1 || !t2) return false;
        if (t1.val !== t2.val) return false;

        // Enqueue children in mirror order
        queue.push(t1.left);
        queue.push(t2.right);
        queue.push(t1.right);
        queue.push(t2.left);
    }
    
    return true;
}
```

### Time and Space Complexity:
- **Time Complexity**: O(n), where n is the total number of nodes in the binary tree.
- **Space Complexity**: O(n), because in the worst case, we need to store all the nodes in a level of the tree in the queue.

