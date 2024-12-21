# [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### Approaches:
- [Recursive Approach](#recursive-approach)
- [Iterative Approach using Stack](#iterative-approach-using-stack)
- [Morris Traversal](#morris-traversal)

## Recursive Approach

### Intuition:
The recursion is a natural way to explore a binary tree because it inherently follows the tree's hierarchical and branching structure. For inorder traversal:  
1. Recursively visit the left subtree.  
2. Visit the root node.  
3. Recursively visit the right subtree.

This approach quite directly translates to a simple recursive function that mirrors the definition of inorder traversal.

### Time Complexity: 
- O(n), where n is the number of nodes in the tree because we visit each node once.

### Space Complexity: 
- O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (unbalanced tree), it could be O(n).

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

function inorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    
    // Helper function to perform the recursive inorder traversal
    function inorder(node: TreeNode | null): void {
        if (node === null) return;
        
        inorder(node.left); // Traverse the left subtree
        result.push(node.val); // Visit the root node
        inorder(node.right); // Traverse the right subtree
    }
    
    inorder(root); // Start with the root node
    return result;
}
```

## Iterative Approach using Stack

### Intuition:
The stack data structure can replicate the system calls used in recursion. We can use a stack to mimic the recursive traversal process. By iterating through the nodes:
1. Push all left children to the stack.
2. Pop off the stack, visit the node.
3. Traverse its right subtree.

This helps us avoid using the call stack, controlling the process explicitly.

### Time Complexity: 
- O(n), as we visit each node once.

### Space Complexity: 
- O(h), where h is the height of the tree, due to the stack's usage.

```typescript
function inorderTraversalStack(root: TreeNode | null): number[] {
    const result: number[] = [];
    const stack: TreeNode[] = [];
    let current: TreeNode | null = root;

    while (current !== null || stack.length !== 0) {
        // Traverse the left subtree
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }
        
        // Visit the node
        current = stack.pop()!;
        result.push(current.val);
        
        // Traverse the right subtree
        current = current.right;
    }

    return result;
}
```

## Morris Traversal

### Intuition:
Morris Traversal leverages a clever use of the tree itself to gain O(1) space complexity. The core idea is to temporarily modify the tree structure by establishing links back to the inorder successor.
1. If a node has no left child, visit it and move to right child.
2. If there is a left child, find the rightmost node in the left subtree (predecessor).
3. If predecessor has no right child, make current the right child and move to left child.
4. If predecessor has a right child, reset it to NULL, visit current and move to right child.

### Time Complexity:
- O(n), each link is only created and removed once per node.

### Space Complexity:
- O(1), since we're not using any extra space except for a few pointers.

```typescript
function inorderTraversalMorris(root: TreeNode | null): number[] {
    const result: number[] = [];
    let current: TreeNode | null = root;

    while (current !== null) {
        // If no left subtree exists, visit the node and move to the right subtree
        if (current.left === null) {
            result.push(current.val);
            current = current.right;
        } else {
            // Find the inorder predecessor of current
            let predecessor: TreeNode = current.left;
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            // Establish temporary link back to the current node
            if (predecessor.right === null) {
                predecessor.right = current;
                current = current.left;
            }
            // Break the link, visit the node, move to right subtree
            else {
                predecessor.right = null;
                result.push(current.val);
                current = current.right;
            }
        }
    }

    return result;
}
```

Each solution progressively enhances understanding of both recursion and iterative tree traversal techniques. The Morris method showcases an intriguing approach to tree traversal with constant space.

