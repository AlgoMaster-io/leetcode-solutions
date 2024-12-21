# [Leetcode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approaches
- [Approach 1: In-order Traversal](#approach-1-in-order-traversal)
- [Approach 2: Recursive In-order Traversal with Validation](#approach-2-recursive-in-order-traversal-with-validation)
- [Approach 3: Recursive Range Checking](#approach-3-recursive-range-checking)

## Approach 1: In-order Traversal

### Intuition
A binary search tree (BST) yields its elements in sorted order when traversed in an in-order manner (left-root-right). We can use this property to determine if a tree is a valid BST. By maintaining an array to collect the values and then checking if this array is sorted, we can verify the tree's validity.

### Code
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function isValidBST(root: TreeNode | null): boolean {
    const inorderTraversal = (node: TreeNode | null, arr: number[]) => {
        if (!node) return;
        inorderTraversal(node.left, arr);
        // Collect the node's value
        arr.push(node.val);
        inorderTraversal(node.right, arr);
    };

    const values: number[] = [];
    inorderTraversal(root, values);

    // Check if the collected values are sorted
    for (let i = 1; i < values.length; i++) {
        if (values[i] <= values[i - 1]) {
            return false;
        }
    }
    return true;
}
```

### Complexity
- **Time Complexity**: O(n) as we visit each node once.
- **Space Complexity**: O(n) due to the use of the array storing the traversal.

## Approach 2: Recursive In-order Traversal with Validation

### Intuition
Instead of using an additional array to store in-order traversal, we can use a variable to track the last visited node value. During traversal, if we encounter a node that's less than or equal to the last visited node, we know the tree is not a BST.

### Code
```typescript
function isValidBST(root: TreeNode | null): boolean {
    let lastVisited: number | null = null;

    const inorderCheck = (node: TreeNode | null): boolean => {
        if (!node) return true;

        // Recursively check the left subtree
        if (!inorderCheck(node.left)) return false;

        // Validate the current node
        if (lastVisited !== null && node.val <= lastVisited) {
            return false;
        }
        lastVisited = node.val;

        // Recursively check the right subtree
        return inorderCheck(node.right);
    };
    
    return inorderCheck(root);
}
```

### Complexity
- **Time Complexity**: O(n)
- **Space Complexity**: O(h), where h is the height of the tree due to the recursion stack.

## Approach 3: Recursive Range Checking

### Intuition
A node in a BST must satisfy the condition that it is greater than all nodes in its left subtree and less than all nodes in its right subtree. This can be ensured by setting range limits for each node as we recursively navigate the tree.

### Code
```typescript
function isValidBST(root: TreeNode | null): boolean {
    const validate = (node: TreeNode | null, lower: number | null, upper: number | null): boolean => {
        if (!node) return true;

        // Validate current node
        if ((lower !== null && node.val <= lower) || (upper !== null && node.val >= upper)) {
            return false;
        }

        // Validate left subtree with an updated upper bound
        if (!validate(node.left, lower, node.val)) return false;

        // Validate right subtree with an updated lower bound
        if (!validate(node.right, node.val, upper)) return false;

        return true;
    };

    return validate(root, null, null);
}
```

### Complexity
- **Time Complexity**: O(n)
- **Space Complexity**: O(h), where h is the height of the tree due to the recursion stack.

Each of these approaches validates the BST properties in unique ways, using either traversal comparison or range verification.

