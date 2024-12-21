## [Leetcode 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

### Table of Contents
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Approach Using Stack](#approach-2-iterative-approach-using-stack)
- [Approach 3: Optimized Iterative Approach Using Two Stacks](#approach-3-optimized-iterative-approach-using-two-stacks)

### Approach 1: Recursive Approach

This basic approach utilizes recursion to traverse the tree in postorder. The postorder traversal visits the left subtree, then the right subtree, and finally the root node. 

#### Intuition:
Recursive traversal calls itself for the left and right children before processing the current node. It naturally follows the postorder mechanism of Left -> Right -> Node.

#### Implementation:
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

function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    
    function traverse(node: TreeNode | null): void {
        if (!node) return;

        // Recurse on the left child
        traverse(node.left);
        // Recurse on the right child
        traverse(node.right);
        // Process the current node
        result.push(node.val);
    }
    
    traverse(root);
    return result;
}
```

#### Time Complexity:
- O(n) - where n is the number of nodes, as we visit each node once.

#### Space Complexity:
- O(n) - due to the stack space used by recursion.

### Approach 2: Iterative Approach Using Stack

Instead of using recursion, we can manage the traversal using an explicit stack to track nodes, and a previous node to manage the sequence of visits.

#### Intuition:
The stack helps simulate the recursive nature of tree traversal. We use a `prev` pointer to check whether to process or revisit the nodes.

#### Implementation:
```typescript
function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (!root) return result;

    const stack: TreeNode[] = [];
    let current: TreeNode | null = root;
    let prev: TreeNode | null = null;

    while (stack.length > 0 || current !== null) {
        // Reach the left most Node of the current Node
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }
        
        // current must be null at this point
        current = stack[stack.length - 1];
        
        // If the right subtree is not processed yet, process it
        if (current.right === null || current.right === prev) {
            result.push(current.val);
            stack.pop();
            prev = current;
            current = null;
        } else {
            current = current.right;
        }
    }
    
    return result;
}
```

#### Time Complexity:
- O(n) - we visit each node exactly once.

#### Space Complexity:
- O(n) - the stack may hold all nodes in the worst case.

### Approach 3: Optimized Iterative Approach Using Two Stacks

This utilizes two stacks to make traversal easier by managing node order explicitly. Here we process nodes similar to reverse preorder and reverse the results.

#### Intuition:
We can utilize two stacks: one to perform a simulated preorder traversal (root-right-left) and another to reverse the order to achieve postorder by taking elements top to bottom.

#### Implementation:
```typescript
function postorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (!root) return result;

    const stack1: TreeNode[] = [];
    const stack2: TreeNode[] = [];

    stack1.push(root);

    while (stack1.length > 0) {
        const node = stack1.pop();
        stack2.push(node!);

        // Push left and then right to pop right first
        if (node!.left) stack1.push(node!.left);
        if (node!.right) stack1.push(node!.right);
    }

    // Second stack gives us postorder when popped
    while (stack2.length > 0) {
        result.push(stack2.pop()!.val);
    }

    return result;
}
```

#### Time Complexity:
- O(n) - we visit each node twice by using two stacks.

#### Space Complexity:
- O(n) - each stack may hold all nodes in the worst case.

