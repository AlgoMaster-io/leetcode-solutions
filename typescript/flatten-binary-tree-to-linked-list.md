# [LeetCode Problem 114: Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Table of Contents
1. [Naive Approach](#naive-approach)
2. [Iterative Approach](#iterative-approach)
3. [Most Optimal Approach - Morris Traversal](#most-optimal-approach---morris-traversal)

---

## Naive Approach

**Intuition**: The naive approach involves performing a preorder traversal of the binary tree and collecting nodes. Then, link nodes linearly based on the collected traversal order. 

### Steps:

1. Perform a preorder traversal of the binary tree and store nodes in a list.
2. Iterate over the list, and adjust the right pointer of each node to point to the next node in the list, essentially flattening the tree in place.
3. Set the left pointer of all nodes to `null` as required by the problem.

### Time Complexity:
- **Time**: O(n) for the traversal
- **Space**: O(n) to store the nodes in a list

### Code:

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

function flatten(root: TreeNode | null): void {
    if (!root) return;
    
    const nodes: TreeNode[] = [];
    
    // Helper function for preorder traversal
    function preorder(node: TreeNode | null) {
        if (node == null) return;
        nodes.push(node);
        preorder(node.left);
        preorder(node.right);
    }
    
    // Fill the nodes list with preorder traversal
    preorder(root);
    
    // Link nodes in list format
    for (let i = 0; i < nodes.length - 1; i++) {
        nodes[i].left = null;
        nodes[i].right = nodes[i + 1];
    }
}
```

---

## Iterative Approach

**Intuition**: We can use an explicit stack to flatten the tree, similar to an iterative preorder traversal but adjusting the pointers during traversal. 

### Steps:

1. Utilize a stack to help perform a preorder traversal.
2. Adjust pointers as nodes are popped off the stack.
3. Ensure the left pointer of each node is set to `null`.

### Time Complexity:
- **Time**: O(n) for traversing each node once
- **Space**: O(n) for the stack in the worst case

### Code:

```typescript
function flatten(root: TreeNode | null): void {
    if (!root) return;
    
    const stack: TreeNode[] = [];
    stack.push(root);
    
    while (stack.length > 0) {
        const current = stack.pop()!;
        
        // If right child exists, push it to stack.
        if (current.right) stack.push(current.right);
        // If left child exists, push it to stack.
        if (current.left) stack.push(current.left);
        
        // Adjust the pointers.
        if (stack.length > 0) {
            current.right = stack[stack.length - 1];
        }
        current.left = null;
    }
}
```

---

## Most Optimal Approach - Morris Traversal

**Intuition**: An optimal way to achieve in constant space is using the Morris traversal technique. The main idea is to connect nodes without using stack or recursion by making use of right pointers.

### Steps:

1. Traverse the tree nodes using a loop.
2. For each node, find its rightmost child in the left subtree.
3. Connect that rightmost child to the current's right subtree.
4. Set current's right to its left subtree.
5. Set current’s left to `null`.

### Time Complexity:
- **Time**: O(n) since each node will be visited at most twice
- **Space**: O(1) as no additional space is used

### Code:

```typescript
function flatten(root: TreeNode | null): void {
    let current = root;
    
    while (current) {
        if (current.left) {
            // Find the rightmost node in the left subtree
            let rightmost = current.left;
            while (rightmost.right) {
                rightmost = rightmost.right;
            }
            
            // Connect it to the right subtree
            rightmost.right = current.right;
            
            // Current's right is now its left
            current.right = current.left;
            current.left = null;
        }
        
        // Move to the next right node
        current = current.right;
    }
}
```

In each approach, the binary tree is transformed into a linked list in-place, but each has differing time and space complexities. The Morris Traversal approach offers the most efficient solution with O(1) space usage.

