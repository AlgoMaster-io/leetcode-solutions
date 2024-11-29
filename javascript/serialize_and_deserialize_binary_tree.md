# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue

class Codec {
    
    // Encodes a tree to a single string
    serialize(root) {
        if (root === null) {
            return "null"; // Return "null" for an empty tree
        }
        
        let serialized = [];
        let queue = [root];
        
        while (queue.length > 0) {
            let current = queue.shift();
            if (current === null) {
                serialized.push("null");
            } else {
                serialized.push(current.val);
                queue.push(current.left);
                queue.push(current.right);
            }
        }
        
        return serialized.join(",");
    }
    
    // Decodes your encoded data to tree
    deserialize(data) {
        if (data === "null") {
            return null; // Return null for an empty tree
        }
        
        let nodes = data.split(",");
        let root = new TreeNode(parseInt(nodes[0], 10));
        let queue = [root];
        
        let i = 1;
        while (queue.length > 0 && i < nodes.length) {
            let current = queue.shift();
            
            if (nodes[i] !== "null") {
                current.left = new TreeNode(parseInt(nodes[i], 10));
                queue.push(current.left);
            }
            i++;
            
            if (i < nodes.length && nodes[i] !== "null") {
                current.right = new TreeNode(parseInt(nodes[i], 10));
                queue.push(current.right);
            }
            i++;
        }
        
        return root;
    }
}
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string

class Codec {
    
    // Encodes a tree to a single string
    serialize(root) {
        if (root === null) {
            return "null"; // Return "null" for an empty tree
        }
        
        let serialized = [];
        this.serializeHelper(root, serialized);
        return serialized.join(",");
    }
    
    serializeHelper(node, serialized) {
        if (node === null) {
            serialized.push("null");
            return;
        }
        
        serialized.push(node.val);
        this.serializeHelper(node.left, serialized);
        this.serializeHelper(node.right, serialized);
    }
    
    // Decodes your encoded data to tree
    deserialize(data) {
        let nodes = data.split(",");
        let queue = nodes.slice(); // Create a copy of the array
        
        return this.deserializeHelper(queue);
    }
    
    deserializeHelper(queue) {
        let current = queue.shift();
        if (current === "null") {
            return null; // Return null for "null" nodes
        }
        
        let node = new TreeNode(parseInt(current, 10));
        node.left = this.deserializeHelper(queue);
        node.right = this.deserializeHelper(queue);
        
        return node;
    }
}
```


