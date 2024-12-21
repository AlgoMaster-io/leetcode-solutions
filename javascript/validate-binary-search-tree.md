# [Leetcode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approaches:
- [Approach 1: Recursive In-Order Traversal](#approach-1-recursive-in-order-traversal)
- [Approach 2: Iterative In-Order Traversal](#approach-2-iterative-in-order-traversal)
- [Approach 3: Recursive Range Validation](#approach-3-recursive-range-validation)

### Approach 1: Recursive In-Order Traversal

**Intuition**: 
A Binary Search Tree (BST) is such that an in-order traversal visits nodes in strictly increasing order. Therefore, by conducting an in-order traversal and maintaining the last seen node, we can verify the strict ordering of the values.

```javascript
function isValidBST(root) {
    let prev = null;

    function inOrder(node) {
        if (!node) return true;

        // Recursively go to the left subtree
        if (!inOrder(node.left)) return false;

        // Check if the current node value is greater than the previous node value
        if (prev !== null && node.val <= prev) return false;
        prev = node.val;

        // Recursively go to the right subtree
        return inOrder(node.right);
    }

    return inOrder(root);
}
```

**Time Complexity**: O(n), where n is the number of nodes, since each node is visited once.

**Space Complexity**: O(n), where n is the height of the tree due to recursion stack space.

### Approach 2: Iterative In-Order Traversal

**Intuition**: 
Instead of relying on recursive traversal, we can simulate an in-order traversal using a stack. This approach also allows us to check the order of traversal iteratively.

```javascript
function isValidBST(root) {
    let stack = [];
    let prev = null;
    let current = root;

    while (stack.length > 0 || current !== null) {
        // Visit left subtree
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }

        // Visit the node
        current = stack.pop();
        if (prev !== null && current.val <= prev.val) return false;
        prev = current;

        // Visit right subtree
        current = current.right;
    }

    return true;
}
```

**Time Complexity**: O(n), where n is the number of nodes, as each node is pushed and popped from the stack once.

**Space Complexity**: O(n), the stack stores nodes up to the height of the tree.

### Approach 3: Recursive Range Validation

**Intuition**: 
Each node in a BST must satisfy the range constraints. Starting from the root, for each node, the node's value must be greater than all ancestor nodes on the left and less than all ancestor nodes on the right. We can maintain these constraints using upper and lower bounds as we traverse the tree.

```javascript
function isValidBST(root, lower = -Infinity, upper = Infinity) {
    if (root === null) return true;

    // The current root value must satisfy the constraint (lower, upper)
    if (root.val <= lower || root.val >= upper) return false;

    // Validate the left subtree with updated upper constraint
    // Validate the right subtree with updated lower constraint
    return isValidBST(root.left, lower, root.val) && isValidBST(root.right, root.val, upper);
}
```

**Time Complexity**: O(n), since each node is visited once.

**Space Complexity**: O(n), accounting for recursion stack space in the worst-case scenario of a skewed tree.

