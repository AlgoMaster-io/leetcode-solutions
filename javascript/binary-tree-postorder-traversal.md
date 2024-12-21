# [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approaches:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Using Two Stacks](#approach-2-iterative-using-two-stacks)
- [Approach 3: Iterative Using One Stack (Most Optimal)](#approach-3-iterative-using-one-stack-most-optimal)

## Approach 1: Recursive Approach

**Intuition**:

The postorder traversal involves visiting the left subtree, then the right subtree, and finally the root of the subtree. In a recursive approach, we mimic this process by recursively calling the same function for the left child, then the right child, and finally processing the root node. This approach takes advantage of recursion's natural stack mechanism.

### Code:
```javascript
var postorderTraversal = function(root) {
    let result = [];
    
    function traverse(node) {
        // Base case: if the current node is null, return
        if (!node) return;
        
        // Recur on the left subtree
        traverse(node.left);
        
        // Recur on the right subtree
        traverse(node.right);
        
        // Visit the root node
        result.push(node.val);
    }
    
    traverse(root); // Start traversal from root
    return result;  // Return the collected result
};
```

### Time and Space Complexity:

- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity**: O(h), where h is the height of the binary tree, corresponding to the depth of the recursion stack.

## Approach 2: Iterative Using Two Stacks

**Intuition**:

By using two stacks, we can simulate the postorder traversal. The first stack is used to process nodes similar to preorder (root, right, left). The second stack reverses this order when nodes are popped: left, right, root, which aligns with postorder.

### Code:
```javascript
var postorderTraversal = function(root) {
    if (!root) return [];

    let stack1 = [root], stack2 = [], result = [];
    
    // Iterate while stack1 is not empty
    while (stack1.length > 0) {
        // Pop the element from stack1
        let node = stack1.pop();
        // Push it onto stack2
        stack2.push(node);
        
        // Push left and right children of node onto stack1
        if (node.left) stack1.push(node.left);
        if (node.right) stack1.push(node.right);
    }
    
    // Pop all elements from stack2 and add to result
    while (stack2.length > 0) {
        result.push(stack2.pop().val);
    }
    
    return result;
};
```

### Time and Space Complexity:

- **Time Complexity**: O(n), where n is the number of nodes. Every node is processed and pushed/popped from stacks.
- **Space Complexity**: O(n), used for storing nodes in the stacks.

## Approach 3: Iterative Using One Stack (Most Optimal)

**Intuition**:

We utilize a single stack to traverse the tree. This approach simulates the recursion mechanism using a stack by tracking the last visited node, which helps to determine whether to visit the current node or continue traversing the tree nodes.

### Code:
```javascript
var postorderTraversal = function(root) {
    let result = [];
    let stack = [];
    let current = root;
    let lastVisited = null;
    
    while (current || stack.length > 0) {
        // Reach the leftmost Node of the current Node
        while (current) {
            stack.push(current);
            current = current.left;
        }
        
        // Check the top node in the stack
        current = stack[stack.length - 1];
        
        // If the right Tree has not been visited, process that next
        if (!current.right || current.right === lastVisited) {
            result.push(current.val);
            stack.pop();  // Pop the node from the stack
            
            // Set last visited to current and process the stack
            lastVisited = current;
            current = null;
        } else {
            // Explore the right subtree
            current = current.right;
        }
    }
    
    return result;
};
```

### Time and Space Complexity:

- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity**: O(n), equivalent to the stack storage during traversal.

Each approach has its benefits, but the iterative one-stack solution is often favored for its efficient space utilization.

