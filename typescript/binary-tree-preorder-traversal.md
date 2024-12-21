# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches:
- [Approach 1: Recursive Preorder Traversal](#approach-1-recursive-preorder-traversal)
- [Approach 2: Iterative Preorder Traversal using Stack](#approach-2-iterative-preorder-traversal-using-stack)

### Approach 1: Recursive Preorder Traversal

**Intuition**:
Preorder traversal involves processing the root, then the left subtree, followed by the right subtree. The recursive solution is the most straightforward approach as it mirrors the natural definition of a preorder traversal.

#### Steps:
1. Initialize an empty result array to store the nodes' values in preorder.
2. Define a recursive helper function that accepts a node as a parameter.
3. In the helper function:
   - If the node is null, return.
   - Add the node's value to the result array.
   - Recursively call the helper function for the left subtree.
   - Recursively call the helper function for the right subtree.
4. Call the helper function starting from the root.
5. Return the result array.

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val === undefined ? 0 : val;
        this.left = left === undefined ? null : left;
        this.right = right === undefined ? null : right;
    }
}

function preorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    
    function traverse(node: TreeNode | null): void {
        if (!node) return;               // Base case: if node is null, do nothing
        result.push(node.val);           // Process the current node
        traverse(node.left);             // Traverse the left subtree
        traverse(node.right);            // Traverse the right subtree
    }
    
    traverse(root);                       // Start traversal from root
    return result;
}
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree, because we visit each node once.

**Space Complexity**: O(N) in the worst case, due to the recursion stack, for a completely unbalanced tree. O(log N) in the best case for a balanced tree.

### Approach 2: Iterative Preorder Traversal using Stack

**Intuition**:
We can simulate the recursive call stack using an explicit stack data structure. This iterative solution ensures traversal without the inherent recursion overhead.

#### Steps:
1. If the root is null, return an empty array.
2. Initialize an empty result array to store the nodes' values in preorder.
3. Initialize a stack and push the root node onto it.
4. While the stack is not empty:
   - Pop the top node from the stack and add its value to the result.
   - If the popped node has a right child, push it onto the stack. 
   - If the popped node has a left child, push it onto the stack.
5. Return the result array.

```typescript
function preorderTraversalIterative(root: TreeNode | null): number[] {
    if (root === null) return [];       // If the tree is empty, return empty array

    const result: number[] = [];
    const stack: TreeNode[] = [root];   // Stack to maintain the nodes to process

    while (stack.length > 0) {
        const node = stack.pop()!;      // Pop the node from the stack
        result.push(node.val);          // Process the current node
        
        if (node.right !== null) {      // Push the right child first
            stack.push(node.right);     // So that left is processed first
        }
        if (node.left !== null) {       // Push the left child to the stack
            stack.push(node.left);
        }
    }

    return result;
}
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree, since we visit each node once.

**Space Complexity**: O(N), where N is the number of nodes in the binary tree; in the worst case, the stack will hold all the nodes in the tree.

