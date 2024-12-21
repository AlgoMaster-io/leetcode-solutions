# [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approaches:
- [Recursive Approach](#recursive-approach)
- [Iterative Approach Using Stack](#iterative-approach-using-stack)
- [Morris Traversal](#morris-traversal)

### Recursive Approach

The recursive approach is the most straightforward method to complete an inorder traversal of a binary tree. In a binary tree, an inorder traversal involves:

1. Traversing the left subtree.
2. Visiting the root node.
3. Traversing the right subtree.

#### Intuition:

- A binary tree can be traversed recursively by calling the inorder function on the left child, then processing the current node, and finally calling the inorder function on the right child.

#### Time Complexity:

- O(n), where n is the number of nodes in the tree, since we visit each node once.

#### Space Complexity:

- O(n) in the worst case if the tree is unbalanced because it depends on the height of the tree. For a balanced tree, the space complexity is O(log n).

```javascript
function inorderTraversal(root) {
    // Initialize an array to store the inorder traversal
    const result = [];

    // Recursive function to perform inorder traversal
    function traverse(node) {
        if (node === null) return; // Base case: if node is null, return

        traverse(node.left);       // Recurse on left child
        result.push(node.val);     // Visit current node
        traverse(node.right);      // Recurse on right child
    }
    
    traverse(root); // Kick-off the recursion
    return result;
}
```

### Iterative Approach Using Stack

An iterative approach can be used to simulate the recursion using an explicit stack. This way, we avoid the function-call overhead of recursion and manage node traversal manually.

#### Intuition:

- Use a stack to keep track of nodes as we traverse down the left children. When no left child is more present, visit a node and then explore the right subtree.

#### Time Complexity:

- O(n), as every node is processed exactly once.

#### Space Complexity:

- O(n) for the stack in the worst case when the tree is completely unbalanced.

```javascript
function inorderTraversal(root) {
    const result = [];
    const stack = [];
    let current = root;

    // Iterate while there are unvisited nodes
    while (current !== null || stack.length > 0) {
        // Travel to each node's left child, and push the node to the stack on the way
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }
        
        // Process the node
        current = stack.pop();
        result.push(current.val);

        // Now explore the right side
        current = current.right;
    }

    return result;
}
```

### Morris Traversal

Morris Traversal is an advanced technique used to achieve inorder traversal with O(1) space complexity by re-threading the tree.

#### Intuition:

- Instead of using a stack to remember the nodes we should return to after finishing the current node, we use the tree to hold this information by making its rightmost node point back to the current node.

#### Time Complexity:

- O(n), although this method modifies the tree structure temporarily, it still has linear time complexity since each edge is traversed at most twice.

#### Space Complexity:

- O(1), since no stack or recursion is used; the tree structure itself is temporarily modified.

```javascript
function inorderTraversal(root) {
    const result = [];
    let current = root;

    while (current !== null) {
        if (current.left === null) {
            // If there is no left child, visit this node and go to the right
            result.push(current.val);
            current = current.right;
        } else {
            // Find the rightmost node in left subtree or node itself if right is null
            let predecessor = current.left;
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            // If the right node points back to the current, it means we've finished that subtree
            if (predecessor.right === null) {
                // Creating a temporary link to predecessor to point to current
                predecessor.right = current;
                current = current.left;
            } else {
                // We finished processing left subtree, remove the link
                predecessor.right = null;
                result.push(current.val);
                current = current.right; // Move to the right subtree of the node
            }
        }
    }

    return result;
}
```

In the Morris traversal, remember to restore the tree to its original state if needed (like in interview settings), since the structure of the tree is temporarily modified.

