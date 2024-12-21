# [LeetCode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approaches:
- [Approach 1: In-Order Traversal + List](#approach-1-in-order-traversal--list)
- [Approach 2: Controlled Recursion using a Stack](#approach-2-controlled-recursion-using-a-stack)

### Approach 1: In-Order Traversal + List

**Intuition:**

A straightforward approach involves performing an in-order traversal of the BST to obtain all elements in sorted order. We can store these elements in a list and then simulate the iterator by accessing elements in this list one by one.

**Solution:**

```javascript
class BSTIterator {
    constructor(root) {
        this.nodes = [];
        this.index = -1;  // pointer to track current node
        this.inOrderTraversal(root);  // perform in-order traversal
    }
    
    inOrderTraversal(node) {
        if (!node) return;
        this.inOrderTraversal(node.left);  // visit left subtree
        this.nodes.push(node.val);  // add node's value to list
        this.inOrderTraversal(node.right);  // visit right subtree
    }
    
    next() {
        // Increment index to access the next element
        return this.nodes[++this.index];
    }
    
    hasNext() {
        // Check if there are remaining elements in the list
        return this.index + 1 < this.nodes.length;
    }
}
```

**Time Complexity:**
- Preprocessing (constructor call): O(N), where N is the number of nodes in the BST since we are performing a complete in-order traversal.
- `next()`: O(1) since we're simply accessing the next element.
- `hasNext()`: O(1) since it's just checking if more elements exist.

**Space Complexity:**
- O(N) due to storing all node values in an array.

### Approach 2: Controlled Recursion using a Stack

**Intuition:**

To improve upon space complexity, we can utilize a controlled recursion technique using a stack. During in-order traversal, we maintain the path to the current node with a stack, ensuring we only store part of the tree in memory at any time.

**Solution:**

```javascript
class BSTIterator {
    constructor(root) {
        this.stack = [];
        this._leftmostInorder(root);  // Initialize the stack with leftmost path
    }
    
    _leftmostInorder(node) {
        // Add nodes of leftmost path to stack
        while (node) {
            this.stack.push(node);
            node = node.left;
        }
    }
    
    next() {
        // Node at the top of the stack is the next smallest element
        const topNode = this.stack.pop();
        
        // If the node has a right child, we process its leftmost path
        if (topNode.right) {
            this._leftmostInorder(topNode.right);
        }
        
        return topNode.val;
    }
    
    hasNext() {
        // Check if stack is non-empty
        return this.stack.length > 0;
    }
}
```

**Time Complexity:**
- Amortized O(1) for `next()`: Each node is pushed and popped from the stack once.
- O(1) for `hasNext()`.

**Space Complexity:**
- O(h) where h is the height of the tree, which is the maximum depth of the call stack. In the worst case, this is O(N) for a skewed tree, but averaged out it's log(N) for a balanced tree.

