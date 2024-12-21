# [Leetcode 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach Using Stack](#iterative-approach-using-stack)

### Recursive Approach

**Intuition:**

In a Binary Search Tree (BST), each node follows the constraint where the left child is smaller and the right child is larger than the node itself. To trim a BST between the given range `[L, R]`, we need to ensure all nodes fall within this range.

1. **Base Case**: If the current node is `null`, simply return `null` as there's nothing to trim.
2. **Node Value Comparison**:
    - If the node's value is less than `L`, discard the left subtree but recurse on the right subtree (since all left nodes are smaller).
    - If the node's value is greater than `R`, discard the right subtree but recurse on the left subtree (since all right nodes are larger).
    - If the node's value is within `[L, R]`, recursively process both subtrees.

**Code:**

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

function trimBST(root: TreeNode | null, L: number, R: number): TreeNode | null {
    if (root === null) return null;

    // If the current node value is less than L, it means all nodes in the left subtree
    // will also be less. So, we just trim the right subtree.
    if (root.val < L) {
        return trimBST(root.right, L, R);
    }

    // If the current node value is greater than R, it means all nodes in the right subtree
    // will also be greater. So, we just trim the left subtree.
    if (root.val > R) {
        return trimBST(root.left, L, R);
    }

    // If the current node value is within the range, we recursively process both subtrees
    root.left = trimBST(root.left, L, R);
    root.right = trimBST(root.right, L, R);

    return root;
}
```

**Complexity Analysis:**

- **Time Complexity**: O(N), where N is the number of nodes. Each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree. This is due to the recursive stack space.

### Iterative Approach Using Stack

**Intuition:**

The iterative approach can be achieved using a stack. We perform a depth-first traversal while modifying the tree structure based on the node's value comparison with `L` and `R`.

1. Use a stack to perform an iterative depth-first traversal of the tree.
2. Traverse down the tree and remove nodes not within the `[L, R]` range:
    - Relink nodes as necessary to skip over subtrees that are outside the range.
    - Continue processing while utilizing a stack to track nodes yet to be trimmed.

**Code:**

```typescript
function trimBSTIterative(root: TreeNode | null, L: number, R: number): TreeNode | null {
    if (!root) return null;

    // Initialize the stack with the root
    const stack: Array<[TreeNode, TreeNode | null, 'left' | 'right' | null]> = [[root, null, null]];
    let newRoot: TreeNode | null = null;

    while (stack.length > 0) {
        const [node, parent, direction] = stack.pop()!;

        if (node.val < L) {
            // If node value is smaller than L, skip its left subtree
            if (node.right) stack.push([node.right, parent, direction]);
        } else if (node.val > R) {
            // If node value is greater than R, skip its right subtree
            if (node.left) stack.push([node.left, parent, direction]);
        } else {
            // If node is within range, consider it part of the final tree
            if (!newRoot) newRoot = node;
            if (direction === 'left' && parent) parent.left = node;
            if (direction === 'right' && parent) parent.right = node;
            
            // Add left and right children to stack
            if (node.left) stack.push([node.left, node, 'left']);
            if (node.right) stack.push([node.right, node, 'right']);
        }
    }
    return newRoot;
}
```

**Complexity Analysis:**

- **Time Complexity**: O(N), where N is the number of nodes. We process each node once.
- **Space Complexity**: O(H), due to the stack usage, where H is the height of the tree.

