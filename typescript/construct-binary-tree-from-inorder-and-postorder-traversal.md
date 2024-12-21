# [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Approaches:
- [Recursive Approach](#recursive-approach)
  
---

## Recursive Approach

### Intuition:
The problem requires us to reconstruct a binary tree from its inorder and postorder traversal arrays. The key observation here is that the **last** element of the postorder traversal is the **root** of the tree or subtree, and an inorder traversal allows us to separate the left and right subtrees.

Given:
- `inorder` traversal: Left, Root, Right
- `postorder` traversal: Left, Right, Root

#### Steps:
1. The last element of the postorder list gives the root value.
2. Find this root value in the inorder list. This split divides the inorder list into left and right subtrees.
3. Recursively assign the left and right children by repeating this process:
   - Elements before the root index in the inorder array form the left subtree.
   - Elements after the root index in the inorder array form the right subtree.

### Time Complexity:
- O(n^2) in the worst case due to searching for the root in the inorder list.

### Space Complexity:
- O(n) for the recursion stack in the worst case of a skewed tree.

### Implementation:

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

function buildTree(inorder: number[], postorder: number[]): TreeNode | null {
    if (!inorder.length || !postorder.length) return null;
    
    // The last element in postorder is the root.
    const rootVal = postorder.pop()!;
    const root = new TreeNode(rootVal);
    
    // Find the index of the root in inorder to split left/right subtrees.
    const inorderIndex = inorder.indexOf(rootVal);
    
    // Recursively build the right subtree
    root.right = buildTree(inorder.slice(inorderIndex + 1), postorder);
    // Recursively build the left subtree
    root.left = buildTree(inorder.slice(0, inorderIndex), postorder);
    
    return root;
}
```

This solution is straightforward and builds the tree using recursion by slicing the inorder array. Further optimization can be done using hashmaps to improve the index search, making it more efficient.

