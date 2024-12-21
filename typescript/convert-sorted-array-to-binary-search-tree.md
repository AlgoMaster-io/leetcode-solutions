# [Leetcode 108: Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

In this problem, we are given a sorted array in ascending order, and we need to convert it into a height-balanced binary search tree (BST). A height-balanced BST is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

## Approaches
- [Recursive Approach](#recursive-approach)
- [Optimal Recursive In-place Approach](#optimal-recursive-in-place-approach)

### Recursive Approach

In this approach, we use divide and conquer to build the tree. We select the middle element of the sorted array as the root node so that it balances the left and right subtrees. Recursively, we repeat the process for the left and right subarrays.

#### Intuition:
- Take the middle element of the current segment of the array as the root, because a balanced BST has its root in the center.
- Recursively select the middle of the left subarray as the left child.
- Recursively select the middle of the right subarray as the right child.
- The base case is when the current segment's starting index is greater than the end index.

#### Code:
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null = null;
    right: TreeNode | null = null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function sortedArrayToBST(nums: number[]): TreeNode | null {
    if (nums.length === 0) return null;

    // Helper function to construct BST
    const buildBST = (start: number, end: number): TreeNode | null => {
        if (start > end) return null;

        const mid = Math.floor((start + end) / 2);

        // Create the node for the middle element
        const node = new TreeNode(nums[mid]);

        // Recursively build the left and right subtrees
        node.left = buildBST(start, mid - 1);
        node.right = buildBST(mid + 1, end);

        return node;
    };

    return buildBST(0, nums.length - 1);
}
```

#### Time Complexity: 
- O(n) because each element in the input array is visited once.

#### Space Complexity: 
- O(log n) in terms of recursive stack space due to the height of the tree.

### Optimal Recursive In-place Approach

We can optimize the above approach by considering that we don’t necessarily need to store any extra arrays since we are just computing indexes when choosing midpoints for the recursion.

#### Intuition:
- This method is nearly identical to the first approach.
- We utilize the same algorithm but drop the step of using additional space for segment arrays computations.

#### Code:
```typescript
function sortedArrayToBST(nums: number[]): TreeNode | null {
    const buildTree = (left: number, right: number): TreeNode | null => {
        if (left > right) return null;
        
        const mid = Math.floor((left + right) / 2);
        const node = new TreeNode(nums[mid]);
        
        node.left = buildTree(left, mid - 1);
        node.right = buildTree(mid + 1, right);

        return node;
    };

    return buildTree(0, nums.length - 1);
}
```

#### Time Complexity: 
- O(n) for the same reasons as prior, since each element in the input array is used exactly once.

#### Space Complexity: 
- O(log n) in terms of recursive stack space, which corresponds to the average height of the tree and the depth of the recursion.

Overall, both solutions maintain the guarantees of constructing a height-balanced BST from the given sorted array without unnecessary complexity.

