# [Leetcode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approaches:
1. [Basic Approach: Inorder Traversal with Extra Space](#basic-approach)
2. [Optimal Approach: Controlled Inorder Traversal](#optimal-approach)

### Basic Approach: Inorder Traversal with Extra Space

#### Intuition:
The simplest approach to implement an iterator for a BST is to first perform an inorder traversal of the tree and store all the nodes in a list. The inorder traversal of a BST gives nodes in non-decreasing order. Once we have this list, the operations `next()` and `hasNext()` can be easily implemented by iterating over the list.

#### Implementation:
```typescript
class BinarySearchTreeIterator {
    private nodes: number[];
    private index: number;

    constructor(root: TreeNode | null) {
        this.nodes = [];
        this.index = -1;
        this.inorderTraversal(root); // Fill the nodes list
    }
    
    private inorderTraversal(root: TreeNode | null): void {
        if (!root) return;
        this.inorderTraversal(root.left);
        this.nodes.push(root.val); // Visiting the node
        this.inorderTraversal(root.right);
    }
    
    next(): number {
        this.index += 1;
        return this.nodes[this.index]; // Return current index node value
    }
    
    hasNext(): boolean {
        return this.index + 1 < this.nodes.length; // Check if there is a next node
    }
}
```

#### Time Complexity:
- `next()` and `hasNext()` take O(1) time.
- The constructor takes O(N) time where N is the number of nodes in the BST.

#### Space Complexity:
- O(N) space is used to store the nodes' values.

---

### Optimal Approach: Controlled Inorder Traversal

#### Intuition:
Storing all nodes in a list might not be memory efficient, especially for large trees. Instead, we can simulate the inorder traversal using a stack. The stack will help us track where we are in the traversal at any point, which will allow us to get the next smallest node whenever required.

#### Implementation:
```typescript
class BSTIterator {
    private stack: TreeNode[];

    constructor(root: TreeNode | null) {
        this.stack = [];
        this._pushLeftPath(root); // Initialize by pushing the left path
    }

    private _pushLeftPath(node: TreeNode | null): void {
        while (node !== null) {
            this.stack.push(node); // Push current node to stack
            node = node.left; // Move to left child
        }
    }
    
    next(): number {
        const node = this.stack.pop(); // Node to be returned
        if (node.right !== null) {
            this._pushLeftPath(node.right); // Traverse the right subtree
        }
        return node.val;
    }
    
    hasNext(): boolean {
        return this.stack.length > 0; // Check if stack is not empty
    }
}
```

#### Time Complexity:
- `next()` and `hasNext()` operations both run in average O(1) time in amortized analysis.

#### Space Complexity:
- O(h) space, where h is the height of the BST, for the stack.

The optimal approach efficiently uses a stack to maintain the state of the traversal, allowing us to use less memory while still fulfilling the problem's constraints.

