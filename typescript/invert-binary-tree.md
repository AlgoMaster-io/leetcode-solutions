# [Leetcode 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Table of Contents

1. [Approach 1: Recursive Solution](#approach-1:-recursive-solution)
2. [Approach 2: Iterative Solution using BFS](#approach-2:-iterative-solution-using-bfs)
3. [Approach 3: Iterative Solution using DFS](#approach-3:-iterative-solution-using-dfs)

### Approach 1: Recursive Solution

#### Intuition

The recursive approach to invert a binary tree is a straightforward method. The key insight is that the inversion of a binary tree can be thought of as recursively swapping the left and right subtrees of each node. This can be achieved by performing the following actions:
- Swap the left and right children of the current node.
- Recursively call the invert function on the left and right children.

#### Code

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

function invertTree(root: TreeNode | null): TreeNode | null {
    // Base case: If the current node is null, return null
    if (root === null) return null;

    // Recursively invert the left and right subtrees
    const left = invertTree(root.left);
    const right = invertTree(root.right);

    // Swap the left and right children
    root.left = right;
    root.right = left;

    // Return the root node which now points to an inverted subtree
    return root;
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the binary tree. We have to visit each node exactly once.
- **Space Complexity:** O(h), where h is the height of the tree. This space is used by the recursion stack. In the worst case (skewed tree), h could be equal to n.

### Approach 2: Iterative Solution using BFS

#### Intuition

For the iterative solution, we can use a queue data structure to perform a level-order traversal (Breadth-First Search). We process each node by swapping its left and right children. This method allows us to achieve an iterative solution without the need for recursion.

#### Code

```typescript
function invertTree(root: TreeNode | null): TreeNode | null {
    if (root === null) return null;

    // Initialize a queue and enqueue the root node to start the BFS
    const queue: TreeNode[] = [root];

    while (queue.length > 0) {
        // Dequeue a node from the front
        const current = queue.shift()!;
        
        // Swap its left and right children
        const temp = current.left;
        current.left = current.right;
        current.right = temp;

        // Enqueue left and right children if they are not null
        if (current.left !== null) queue.push(current.left);
        if (current.right !== null) queue.push(current.right);
    }

    return root;
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity:** O(n), where n is the number of nodes in the binary tree. In the worst case, the queue could contain half of the nodes in the tree.

### Approach 3: Iterative Solution using DFS

#### Intuition

Alternatively, we can use a stack to perform a Depth-First Search to achieve an iterative inversion of the binary tree. This is essentially a depth-first traversal where nodes are processed by swapping their left and right children.

#### Code

```typescript
function invertTree(root: TreeNode | null): TreeNode | null {
    if (root === null) return null;

    // Initialize a stack and push the root node
    const stack: TreeNode[] = [root];

    while (stack.length > 0) {
        // Pop a node from the top of the stack
        const current = stack.pop()!;
        
        // Swap its left and right children
        const temp = current.left;
        current.left = current.right;
        current.right = temp;

        // Push non-null children to the stack for later processing
        if (current.left !== null) stack.push(current.left);
        if (current.right !== null) stack.push(current.right);
    }

    return root;
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity:** O(n), where n is the number of nodes in the binary tree. The stack can contain up to the height of the tree nodes, which in the worst case (skewed tree) could be n.

