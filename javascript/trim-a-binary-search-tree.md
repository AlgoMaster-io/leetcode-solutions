## [Leetcode 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

### Table of Contents
- [Approach 1: Recursive Traversal](#approach-1-recursive-traversal)
- [Approach 2: Iterative Traversal using Stack](#approach-2-iterative-traversal-using-stack)

## Approach 1: Recursive Traversal

### Intuition
The problem requires us to trim a binary search tree so that all its elements lie between `low` and `high`, inclusive. The property of a binary search tree (BST) allows us to make efficient decisions. Specifically, if the root node's value is less than `low`, then we don't need it or its left subtree. Similarly, if the root node's value is greater than `high`, then we don't need it or its right subtree. 

Thus, we can use a recursive approach:
1. Compare the root's value with `low` and `high`.
2. Recursively adjust the left and right children based on boundaries.
3. If the root's value is within `low` and `high`, keep it and recursively check its children.

### Code
```javascript
function trimBST(root, low, high) {
    if (!root) return null;

    // If the current node's value is less than low, recursively trim the right subtree.
    if (root.val < low) {
        return trimBST(root.right, low, high);
    }
    
    // If the current node's value is greater than high, recursively trim the left subtree.
    if (root.val > high) {
        return trimBST(root.left, low, high);
    }
    
    // If the current node's value is within the range, trim both subtrees.
    root.left = trimBST(root.left, low, high);
    root.right = trimBST(root.right, low, high);
    
    return root;
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the total number of nodes in the tree. In the worst case, we might have to check each node once.
- **Space Complexity**: O(N) in the worst case due to the space used by the recursive call stack.

## Approach 2: Iterative Traversal using Stack

### Intuition
The recursive solution can be transformed into an iterative one using a stack. This approach is useful if we want to avoid the call stack space consumption of recursion. We'll use a stack to perform an in-order traversal, trimming nodes as needed.

### Code
```javascript
function trimBST(root, low, high) {
    if (!root) return null;

    // Define a helper function to trim nodes based on bounds
    function trim(node) {
        while (node && (node.val < low || node.val > high)) {
            if (node.val < low) {
                node = node.right;
            } else if (node.val > high) {
                node = node.left;
            }
        }
        return node;
    }

    // Initialize stack and result structure
    let dummy = new TreeNode(0);
    dummy.right = root;
    let parent = dummy;
    let stack = [parent];
    let current = root;
    
    // Iteratively process the tree
    while (stack.length > 0) {
        while (current) {
            stack.push(current);
            current = trim(current.left);
        }

        current = stack.pop();

        // Trim the right subtree
        current.right = trim(current.right);

        // Move to the right
        current = current.right;
    }

    return dummy.right;
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the total number of nodes in the tree. We may need to visit each node once.
- **Space Complexity**: O(N), where N is the number of nodes in the tree. This accounts for the maximum number of nodes that the stack may hold at once.

