# [Leetcode 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Optimized Recursive Approach with Hash Map](#optimized-recursive-approach-with-hash-map)

### Recursive Approach

#### Intuition:
The problem requires constructing a binary tree given the preorder and inorder traversal of the tree. In preorder traversal, the first element is always the root. In inorder traversal, the elements to the left of the root form the left subtree, and to the right form the right subtree. Using these properties, we can recursively construct the tree.

1. Take the first element from the preorder list as the root.
2. Find this root in the inorder list, which splits the list into left and right subtrees.
3. Recursively apply this process for the left and right subtrees.

#### Code:
```javascript
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

function buildTree(preorder, inorder) {
    // Helper function to construct the tree
    function arrayToTree(preorder, inorder) {
        if (inorder.length === 0) return null;
        
        // The first element in preorder is always the root
        const rootVal = preorder.shift();
        const root = new TreeNode(rootVal);

        // Find the index of the root in inorder traversal
        const index = inorder.indexOf(rootVal);

        // Build left and right subtree recursively
        root.left = arrayToTree(preorder, inorder.slice(0, index));
        root.right = arrayToTree(preorder, inorder.slice(index + 1));

        return root;
    }
    
    return arrayToTree(preorder, inorder);
}
```

#### Time Complexity:
- The `indexOf` operation makes this approach O(n^2) in the worst case.

#### Space Complexity:
- The space complexity is O(n) due to the recursion stack and space required to store the nodes.

### Optimized Recursive Approach with Hash Map

#### Intuition:
The basic approach can be optimized by using a hash map to store indices of inorder elements. This ensures that index lookups take O(1) time, thus reducing the time complexity of the algorithm.

1. Pre-construct a hashmap that maps values to their indices in the inorder list.
2. Use the map to quickly split the inorder list without iterating over it.
3. Recursively build the tree using indices.

#### Code:
```javascript
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

function buildTree(preorder, inorder) {
    // Create a map to store the index of each value in the inorder list for O(1) access
    const inorderMap = {};
    inorder.forEach((value, index) => {
        inorderMap[value] = index;
    });

    let preIndex = 0;

    function arrayToTree(left, right) {
        if (left > right) return null;

        // Select preIndex element as the root and increment it
        const rootVal = preorder[preIndex++];
        const root = new TreeNode(rootVal);

        // Build left and right subtrees
        const index = inorderMap[rootVal];
        
        root.left = arrayToTree(left, index - 1);
        root.right = arrayToTree(index + 1, right);

        return root;
    }
    
    return arrayToTree(0, inorder.length - 1);
}
```

#### Time Complexity:
- This approach is O(n) because each element in the preorder and inorder lists is processed exactly once.

#### Space Complexity:
- The space complexity remains O(n) due to the recursion stack and the hashmap storing all nodes.

