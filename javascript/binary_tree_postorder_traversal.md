# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

class TreeNode {
    constructor(val, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

function postorderTraversal(root) {
    const result = [];
    postorder(root, result);
    return result;
}

function postorder(node, result) {
    if (node === null) {
        return; // Base case: if the node is null, return
    }

    postorder(node.left, result); // Recurse for the left subtree
    postorder(node.right, result); // Recurse for the right subtree
    result.push(node.val); // Visit the root
}
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks

class TreeNode {
    constructor(val, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

function postorderTraversal(root) {
    const result = [];
    if (root === null) {
        return result;
    }

    const stack1 = [];
    const stack2 = [];
    stack1.push(root);

    while (stack1.length !== 0) {
        const current = stack1.pop();
        stack2.push(current);

        if (current.left !== null) {
            stack1.push(current.left); // Push left child to stack1
        }
        if (current.right !== null) {
            stack1.push(current.right); // Push right child to stack1
        }
    }

    while (stack2.length !== 0) {
        result.push(stack2.pop().val); // Add nodes in postorder from stack2
    }

    return result;
}
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

class TreeNode {
    constructor(val, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

function postorderTraversal(root) {
    const result = [];
    if (root === null) {
        return result;
    }

    const stack = [];
    let current = root;
    let lastVisited = null;
    
    while (current !== null || stack.length !== 0) {
        if (current !== null) {
            stack.push(current); // Push nodes to the stack
            current = current.left; // Move to the left subtree
        } else {
            const peekNode = stack[stack.length - 1];
            if (peekNode.right !== null && lastVisited !== peekNode.right) {
                current = peekNode.right; // Move to the right subtree
            } else {
                result.push(peekNode.val); // Visit the node
                lastVisited = stack.pop();
            }
        }
    }

    return result;
}
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

class TreeNode {
    constructor(val, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

function postorderTraversal(root) {
    const result = [];
    const dummy = new TreeNode(0);
    dummy.left = root;
    let current = dummy, predecessor = null;

    while (current !== null) {
        if (current.left === null) {
            current = current.right;
        } else {
            predecessor = current.left;
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            if (predecessor.right === null) {
                predecessor.right = current; // Create a temporary thread
                current = current.left;
            } else {
                reverseAddPath(current.left, predecessor, result);
                predecessor.right = null; // Remove the temporary thread
                current = current.right;
            }
        }
    }

    return result;
}

function reverseAddPath(start, end, result) {
    const temp = [];
    while (start !== end) {
        temp.push(start.val);
        start = start.right;
    }
    temp.push(end.val);
    for (let i = temp.length - 1; i >= 0; i--) {
        result.push(temp[i]); // Add nodes in reverse order
    }
}
```

