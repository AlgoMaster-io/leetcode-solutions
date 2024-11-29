# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
typescript
```typescript
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack

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

class BSTIterator {
    private stack: TreeNode[];

    constructor(root: TreeNode | null) {
        this.stack = [];
        this.pushLeft(root);
    }

    // Push all left children of the current node to the stack
    private pushLeft(node: TreeNode | null): void {
        while (node !== null) {
            this.stack.push(node);
            node = node.left;
        }
    }

    /** @return the next smallest number */
    next(): number {
        const node = this.stack.pop()!;
        if (node.right !== null) {
            this.pushLeft(node.right);
        }
        return node.val;
    }

    /** @return whether we have a next smallest number */
    hasNext(): boolean {
        return this.stack.length > 0;
    }
}
```

## Approach 2: Precomputed Inorder Traversal

### Solution
typescript
```typescript
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - next(): O(1)
//   - hasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree

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

class BSTIterator {
    private inorder: number[];
    private index: number;

    constructor(root: TreeNode | null) {
        this.inorder = [];
        this.index = 0;
        this.inorderTraversal(root);
    }

    // Perform an inorder traversal to store the elements
    private inorderTraversal(node: TreeNode | null): void {
        if (node === null) {
            return;
        }
        this.inorderTraversal(node.left);
        this.inorder.push(node.val);
        this.inorderTraversal(node.right);
    }

    /** @return the next smallest number */
    next(): number {
        return this.inorder[this.index++];
    }

    /** @return whether we have a next smallest number */
    hasNext(): boolean {
        return this.index < this.inorder.length;
    }
}
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
typescript
```typescript
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(1), no additional storage

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

class BSTIterator {
    private current: TreeNode | null;
    private predecessor: TreeNode | null;

    constructor(root: TreeNode | null) {
        this.current = root;
    }

    /** @return the next smallest number */
    next(): number {
        let value = -1;

        while (this.current !== null) {
            if (this.current.left === null) {
                value = this.current.val;
                this.current = this.current.right;
                break;
            } else {
                this.predecessor = this.current.left;

                while (this.predecessor.right !== null && this.predecessor.right !== this.current) {
                    this.predecessor = this.predecessor.right;
                }

                if (this.predecessor.right === null) {
                    this.predecessor.right = this.current;
                    this.current = this.current.left;
                } else {
                    this.predecessor.right = null;
                    value = this.current.val;
                    this.current = this.current.right;
                    break;
                }
            }
        }

        return value;
    }

    /** @return whether we have a next smallest number */
    hasNext(): boolean {
        return this.current !== null;
    }
}
```

