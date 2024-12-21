# [Leetcode 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approaches:
- [Approach 1: Recursive Approach with Detailed Steps](#approach-1-recursive-approach-with-detailed-steps)
- [Approach 2: Optimized Recursive Approach using HashMap](#approach-2-optimized-recursive-approach-using-hashmap)

## Approach 1: Recursive Approach with Detailed Steps

### Intuition:
This problem can be tackled effectively using a recursive approach. The preorder traversal provides the root node of the binary tree at each recursive step, and the inorder traversal helps in understanding the boundaries for left and right subtrees.

1. **Base Case**: If there are no elements in the inorder list between the given indices, it implies there is no subtree, hence return `null`.
2. **Root Identification**: As per the preorder traversal, the first element is always the root node.
3. **Split inorder list**: Find the index of the root node in the inorder list, which separates left subtree nodes (left of the index) from the right subtree nodes (right of the index).
4. **Recursive Construction**: Construct left and right subtrees using segments of preorder and inorder lists.

### Implementation in TypeScript:

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val ?? 0;
        this.left = left ?? null;
        this.right = right ?? null;
    }
}

function buildTree(preorder: number[], inorder: number[]): TreeNode | null {
    const build = (preStart: number, inStart: number, inEnd: number): TreeNode | null => {
        if (inStart > inEnd) return null;

        // Select preStart element as a root
        const rootVal = preorder[preStart];
        const root = new TreeNode(rootVal);

        // Find root value in the inorder segment
        const inIndex = inorder.indexOf(rootVal);

        // Elements left to inIndex are part of the left subtree
        root.left = build(preStart + 1, inStart, inIndex - 1);
        // Elements right to inIndex are part of the right subtree
        root.right = build(preStart + (inIndex - inStart) + 1, inIndex + 1, inEnd);

        return root;
    };

    return build(0, 0, inorder.length - 1);
}
```

### Time Complexity:
- **Time**: `O(n^2)` - Where `n` is the number of nodes. This is due to finding the root index in the inorder array which can take `O(n)` time for each call.
- **Space**: `O(n)` - Due to recursion stack usage.

## Approach 2: Optimized Recursive Approach using HashMap

### Intuition:
To optimize the approach, a hash map can be utilized to store indices of inorder elements ahead of time, allowing for O(1) access during tree construction.

### Implementation in TypeScript:

```typescript
function buildTreeOptimized(preorder: number[], inorder: number[]): TreeNode | null {
    const inOrderMap = new Map<number, number>();
      
    // Populate the inOrderMap for quick index lookup
    for (let i = 0; i < inorder.length; i++) {
        inOrderMap.set(inorder[i], i);
    }

    const build = (preStart: number, inStart: number, inEnd: number): TreeNode | null => {
        if (inStart > inEnd) return null;

        // Pick the preStart element as a root
        const rootVal = preorder[preStart];
        const root = new TreeNode(rootVal);

        // Find the position in the inorder sequence (in constant time)
        const inIndex = inOrderMap.get(rootVal)!;

        // Construct the subtrees by splitting inorder around inIndex
        root.left = build(preStart + 1, inStart, inIndex - 1);
        root.right = build(preStart + (inIndex - inStart) + 1, inIndex + 1, inEnd);

        return root;
    };

    return build(0, 0, inorder.length - 1);
}
```

### Time Complexity:
- **Time**: `O(n)` - Building the hash map takes O(n), and each subtree division is done in O(1) due to the hash map.
- **Space**: `O(n)` - Due to the hash map and recursion stack usage.

