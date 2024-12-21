## LeetCode Problem 106: Construct Binary Tree from Inorder and Postorder Traversal

Link to the problem: [LeetCode 106](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### Approaches:

- [Recursive Approach](#recursive-approach)

---

### Recursive Approach

The problem involves constructing a binary tree given two sequences, inorder and postorder traversal of the tree.

#### Intuition

1. **Understanding Traversals**:
   - **Inorder Traversal**: Follows Left, Root, Right. Therefore, given the inorder sequence, the left subtree elements are found to the left of the root in sequence, and the right subtree elements are to the right of the root.
   - **Postorder Traversal**: Follows Left, Right, Root. Thus, in the postorder sequence, the last element is always the root of the subtree.

2. **Constructing the Tree**:
   - We can identify the root as the last element of the postorder array.
   - Locate this root in the inorder array. This divides the tree into left and right subtrees.
   - Recursively apply this process to the left and right subtrees.

Below is the step-by-step implementation:

```javascript
var buildTree = function(inorder, postorder) {
    // Map to hold the index of elements in the inorder traversal for quick lookup
    const inorderIndexMap = new Map();
    
    // Fill the map with corresponding inorder indices for quick access
    inorder.forEach((value, index) => {
        inorderIndexMap.set(value, index);
    });

    // Helper function responsible for building the tree recursively
    function helper(inStart, inEnd, postStart, postEnd) {
        // Base case: if there are no elements to construct the tree
        if (inStart > inEnd || postStart > postEnd) return null;

        // Root is the last element in the current postorder segment
        const rootVal = postorder[postEnd];
        const root = new TreeNode(rootVal);

        // Get the index of the root value in inorder to find the boundary
        const inRootIndex = inorderIndexMap.get(rootVal);
        const leftSubtreeSize = inRootIndex - inStart;

        // Recursively build the left subtree
        root.left = helper(inStart, inRootIndex - 1, postStart, postStart + leftSubtreeSize - 1);
        // Recursively build the right subtree
        root.right = helper(inRootIndex + 1, inEnd, postStart + leftSubtreeSize, postEnd - 1);

        return root;
    }

    // Initiate the recursion with the full range of inorder and postorder arrays
    return helper(0, inorder.length - 1, 0, postorder.length - 1);
};
```

#### Detailed Comments:

- We first create a hashmap `inorderIndexMap` to store the indexes of values in inorder traversal for O(1) access time.
- The `helper` function is a recursive function that constructs the tree:
  - For the base condition, it checks if there are no elements to form a subtree.
  - It identifies the `rootVal` from the end of the current `postorder` section.
  - It finds `rootVal` in the `inorder` array to divide the array into left and right subtrees using the index found.
  - It calculates the size of the left subtree to accurately break the current sections of inorder and postorder arrays for recursive calls for left and right subtrees.
  - Constructs left and right children by calling `helper` recursively.

#### Time and Space Complexities:

- **Time Complexity**: O(n), where n is the number of nodes. Each node is processed once.
- **Space Complexity**: O(n), for the hashmap storage and recursive stack space.


