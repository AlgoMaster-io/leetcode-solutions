# [Leetcode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approaches:
1. [Level Order Serialization and Deserialization](#level-order-serialization-and-deserialization)
2. [PreOrder Traversal with Delimiters](#preorder-traversal-with-delimiters)

---

### Level Order Serialization and Deserialization

**Intuition:**

The idea behind using level order (BFS) traversal for serialization and deserialization is to reflect the natural breadth-first structure of a tree. This approach considers nodes level-by-level and uses 'null' to denote non-existent or leaf children, which makes deserialization straightforward.

**Serialize:**

- We will use a queue to perform a BFS traversal of the tree.
- Append each node's value to a list if it exists, or 'null' if it does not.
- Join the list into a string with commas to return the serialized string.

**Deserialize:**

- Use a queue to manage nodes during reconstruction.
- Begin with the root node.
- For each node, attach its left and right children based on the following items in the list/queue.

**Code:**

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    
    constructor(val: number, left: TreeNode | null = null, right: TreeNode | null = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Codec {
    serialize(root: TreeNode | null): string {
        if (!root) return 'null';
        
        const queue: (TreeNode | null)[] = [root];
        const result: string[] = [];
        
        while (queue.length) {
            const node = queue.shift();
            if (node) {
                result.push(node.val.toString());
                queue.push(node.left);
                queue.push(node.right);
            } else {
                result.push('null');
            }
        }
        
        return result.join(',');
    }

    deserialize(data: string): TreeNode | null {
        if (data === 'null') return null;
        
        const values = data.split(',');
        const root = new TreeNode(parseInt(values[0]));
        const queue: TreeNode[] = [root];
        let index = 1;
        
        while (queue.length) {
            const node = queue.shift();
            
            if (values[index] !== 'null') {
                node.left = new TreeNode(parseInt(values[index]));
                queue.push(node.left);
            }
            index++;
            
            if (values[index] !== 'null') {
                node.right = new TreeNode(parseInt(values[index]));
                queue.push(node.right);
            }
            index++;
        }
        
        return root;
    }
}
```

**Time Complexity:**

- Serialization: O(N), where N is the number of nodes in the tree, because each node is processed once.
- Deserialization: O(N), each node is processed once.

**Space Complexity:**

- Serialization: O(N), due to the space required for the result list and queue.
- Deserialization: O(N), for storing the nodes in the queue and the result tree.

---

### PreOrder Traversal with Delimiters 

**Intuition:**

Using preorder traversal ensures that the root node is processed before its child nodes, making it feasible for serialization. Adding delimiters (like commas) allows us to reconstruct the exact tree structure during deserialization.

**Serialize:**

- Perform a recursive preorder traversal of the tree.
- Store each node value and use 'null' to denote non-existent nodes, separated by commas.

**Deserialize:**

- Utilize an iterator to reconstruct the tree structure recursively.
- Follow the preorder pattern: recreate the root and then recursively call on left and right subtrees.

**Code:**

```typescript
class CodecPreorder {
    serialize(root: TreeNode | null): string {
        const result: string[] = [];
        
        function preorder(node: TreeNode | null) {
            if (!node) {
                result.push('null');
                return;
            }
            result.push(node.val.toString());
            preorder(node.left);
            preorder(node.right);
        }
        
        preorder(root);
        return result.join(',');
    }

    deserialize(data: string): TreeNode | null {
        const values = data.split(',');
        let index = 0;
        
        function buildTree(): TreeNode | null {
            if (index >= values.length || values[index] === 'null') {
                index++;
                return null;
            }
            
            const node = new TreeNode(parseInt(values[index]));
            index++;
            node.left = buildTree();
            node.right = buildTree();
            
            return node;
        }
        
        return buildTree();
    }
}
```

**Time Complexity:**

- Serialization: O(N), where N is the number of nodes, due to recursive traversal.
- Deserialization: O(N), since each node and 'null' placeholder is processed once.

**Space Complexity:**

- Serialization: O(N), for the recursive call stack and result list.
- Deserialization: O(N), for the recursive call stack during tree reconstruction.

