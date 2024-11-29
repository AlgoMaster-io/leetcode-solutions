# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

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

function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    function postorder(node: TreeNode | null) {
        if (node === null) {
            return; // Base case: if the node is null, return
        }

        postorder(node.left); // Recurse for the left subtree
        postorder(node.right); // Recurse for the right subtree
        result.push(node.val); // Visit the root
    }
    postorder(root);
    return result;
}
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks

function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (root === null) {
        return result;
    }

    const stack1: TreeNode[] = [];
    const stack2: TreeNode[] = [];
    stack1.push(root);

    while (stack1.length > 0) {
        const current = stack1.pop()!;
        stack2.push(current);

        if (current.left !== null) {
            stack1.push(current.left); // Push left child to stack1
        }
        if (current.right !== null) {
            stack1.push(current.right); // Push right child to stack1
        }
    }

    while (stack2.length > 0) {
        result.push(stack2.pop()!.val); // Add nodes in postorder from stack2
    }

    return result;
}
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (root === null) {
        return result;
    }

    const stack: TreeNode[] = [];
    let current = root;
    let lastVisited: TreeNode | null = null;

    while (current !== null || stack.length > 0) {
        if (current !== null) {
            stack.push(current); // Push nodes to the stack
            current = current.left; // Move to the left subtree
        } else {
            const peekNode = stack[stack.length - 1];
            if (peekNode.right !== null && lastVisited !== peekNode.right) {
                current = peekNode.right; // Move to the right subtree
            } else {
                result.push(peekNode.val); // Visit the node
                lastVisited = stack.pop()!;
            }
        }
    }

    return result;
}
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    const dummy: TreeNode = new TreeNode(0);
    dummy.left = root;
    let current: TreeNode | null = dummy, predecessor: TreeNode | null = null;

    while (current !== null) {
        if (current.left === null) {
            current = current.right;
        } else {
            predecessor = current.left;
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            if (predecessor.right === null) {
                predecessor.right = current; // Create a temporary thread
                current = current.left;
            } else {
                reverseAddPath(current.left, predecessor, result);
                predecessor.right = null; // Remove the temporary thread
                current = current.right;
            }
        }
    }
    return result;
}

function reverseAddPath(start: TreeNode | null, end: TreeNode, result: number[]): void {
    const temp: number[] = [];
    while (start !== end) {
        temp.push(start!.val);
        start = start!.right;
    }
    temp.push(end.val);
    for (let i = temp.length - 1; i >= 0; i--) {
        result.push(temp[i]); // Add nodes in reverse order
    }
}
```

