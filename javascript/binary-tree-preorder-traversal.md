# [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches
- [Approach 1: Recursive Preorder Traversal](#approach-1-recursive-preorder-traversal)
- [Approach 2: Iterative Preorder Traversal using Stack](#approach-2-iterative-preorder-traversal-using-stack)
- [Approach 3: Morris Preorder Traversal](#approach-3-morris-preorder-traversal)

---

### Approach 1: Recursive Preorder Traversal

#### Intuition
Preorder traversal of a binary tree is a depth-first traversal method which visits root first, then recursively visits the left subtree, and finally the right subtree. This recursive strategy can naturally utilize the call stack to keep track of the nodes, making the code simple and elegant.

#### Steps
1. Define a helper function to carry out the recursive traversal.
2. Start the recursion from the root node.
3. For each node, append its value to the result array.
4. Recursively call the left child and then the right child.
5. Return the result array when all nodes have been visited.

```javascript
function preorderTraversal(root) {
    const result = [];

    function traverse(node) {
        // Base case: if the node is null, return
        if (!node) return;

        // Process the current node (Preorder: Node -> Left -> Right)
        result.push(node.val);

        // Recurse to the left subtree
        traverse(node.left);

        // Recurse to the right subtree
        traverse(node.right);
    }

    // Initial call to the helper with the root node
    traverse(root);

    return result;
}
```

**Time Complexity:** O(n) - Each node is visited exactly once.  
**Space Complexity:** O(n) - Space for recursion stack in case of an imbalanced tree.

---

### Approach 2: Iterative Preorder Traversal using Stack

#### Intuition
An iterative solution avoids the function call overhead and potential stack overflow by using an explicit stack. The stack mimics the call stack of a recursive solution, storing nodes that need to be processed. This approach can be more efficient in the case of very deep trees.

#### Steps
1. Initialize a stack with the root node.
2. While the stack is not empty:
   - Pop the node from the top of the stack.
   - Visit the node by adding its value to the result array.
   - Push the right child (if exists) to the stack.
   - Push the left child (if exists) to the stack.
3. Right is pushed before left to ensure left subtree is processed first (stack is LIFO).

```javascript
function preorderTraversal(root) {
    const result = [];
    const stack = [];

    if (root) stack.push(root);

    while (stack.length) {
        const node = stack.pop();

        // Process the current node
        result.push(node.val);

        // Push right then left
        if (node.right) stack.push(node.right);
        if (node.left) stack.push(node.left);
    }

    return result;
}
```

**Time Complexity:** O(n) - Each node is pushed and popped once.  
**Space Complexity:** O(n) - In the worst case, the stack will hold all nodes of a skewed tree.

---

### Approach 3: Morris Preorder Traversal

#### Intuition
Morris Traversal aims to reduce the space complexity to O(1) by using the "threading" technique. It creates temporary links in the tree structure to allow us to traverse back to the parents without using extra space. Each node's predecessor in its left subtree is linked to the node itself, allowing the traversal to return to the node after finishing its left subtree.

#### Steps
1. Start with the root node.
2. For each node:
   - If the node has no left child, process it and move to the right child.
   - If the node has a left child, find the rightmost node in the left subtree (predecessor).
   - If the predecessor’s right is null, link it to the current node, then move to the left.
   - If the predecessor’s right is the current node, unlink it, process the current node and move to the right.
3. Continue until all nodes are processed.

```javascript
function preorderTraversal(root) {
    const result = [];
    let current = root;

    while (current) {
        if (!current.left) {
            // No left child, visit this node
            result.push(current.val);
            // Move to the right child
            current = current.right;
        } else {
            // Find the predecessor
            let predecessor = current.left;
            while (predecessor.right && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            if (!predecessor.right) {
                // Establish a temporary link to the current node
                result.push(current.val);  // Process the current before left children
                predecessor.right = current;
                current = current.left;
            } else {
                // Predecessor's right is already linked to current
                predecessor.right = null; // Break the link
                current = current.right; // Move to the right
            }
        }
    }

    return result;
}
```

**Time Complexity:** O(n) - Each node is visited twice (once to establish links, once to break them).  
**Space Complexity:** O(1) - No extra space used; the tree is temporarily modified.

