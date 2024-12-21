## [Leetcode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

### Approaches
1. [Naive Recursive Approach - Preorder Traversal](#naive-recursive-approach---preorder-traversal)
2. [Iterative Level Order Approach](#iterative-level-order-approach)

---

### Naive Recursive Approach - Preorder Traversal

**Intuition**

To serialize a binary tree, we can use a preorder traversal to visit each node and form a string representation. During the traversal, if a node exists, we add its value to the string. If a node is null, we add a special character (e.g., `'null'`) to signify the absence of a node.

For deserialization, we can reverse this process by reading the string and reconstruct the binary tree. We use a helper function to consume values sequentially and build the tree recursively.

**Code**

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */

var serialize = function(root) {
    const result = [];
    
    function helper(node) {
        if (node === null) {
            result.push('null');
            return;
        }
        
        result.push(node.val);
        helper(node.left);
        helper(node.right);
    }
    
    helper(root);
    return result.join(',');
};

var deserialize = function(data) {
    const nodes = data.split(',');
    let index = 0;
    
    function helper() {
        if (index >= nodes.length || nodes[index] === 'null') {
            index++;
            return null;
        }
        
        const node = new TreeNode(parseInt(nodes[index]));
        index++;
        node.left = helper();
        node.right = helper();
        return node;
    }
    
    return helper();
};
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree. This accounts for touching each node once in both serialization and deserialization.

**Space Complexity**: O(N) for the call stack during recursive serialization and deserialization and also for storing the serialized result.

---

### Iterative Level Order Approach

**Intuition**

Level Order traversal (BFS) can also be used to serialize and deserialize a binary tree. This approach uses a queue to traverse each level of the tree, adding node values to a list for serialization. For null nodes, a placeholder is added to signify missing nodes.

During deserialization, the queue is again used to construct each level of the tree from the serialized data.

**Code**

```javascript
var serialize = function(root) {
    if (root === null) return 'null';
    
    const result = [];
    const queue = [root];
    
    while (queue.length) {
        const node = queue.shift();
        
        if (node === null) {
            result.push('null');
        } else {
            result.push(node.val);
            queue.push(node.left);
            queue.push(node.right);
        }
    }
    
    return result.join(',');
};

var deserialize = function(data) {
    if (data === 'null') return null;
    
    const nodes = data.split(',');
    const root = new TreeNode(parseInt(nodes[0]));
    const queue = [root];
    let index = 1;
    
    while (queue.length) {
        const node = queue.shift();
        
        if (nodes[index] !== 'null') {
            node.left = new TreeNode(parseInt(nodes[index]));
            queue.push(node.left);
        }
        index++;
        
        if (nodes[index] !== 'null') {
            node.right = new TreeNode(parseInt(nodes[index]));
            queue.push(node.right);
        }
        index++;
    }
    
    return root;
};
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree. This approach also visits each node once for serialization and deserialization.

**Space Complexity**: O(N), primarily due to the queue used for the level order traversal which, in the worst case, holds all nodes of the last level of the tree.

This method efficiently handles trees with varying depth and helps maintain the order across different levels easily without recursion.

