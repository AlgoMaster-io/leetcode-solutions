# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

class Codec {

    // Encodes a tree to a single string
    serialize(root: TreeNode | null): string {
        if (root === null) {
            return "null"; // Return "null" for an empty tree
        }

        let serialized: string[] = [];
        let queue: (TreeNode | null)[] = [root];

        while (queue.length > 0) {
            let current = queue.shift();
            if (current === null) {
                serialized.push("null");
            } else {
                serialized.push(current.val.toString());
                queue.push(current.left);
                queue.push(current.right);
            }
        }

        return serialized.join(",");
    }

    // Decodes your encoded data to tree
    deserialize(data: string): TreeNode | null {
        if (data === "null") {
            return null; // Return null for an empty tree
        }

        let nodes = data.split(",");
        let root = new TreeNode(parseInt(nodes[0]));
        let queue: TreeNode[] = [root];

        let i = 1;
        while (queue.length > 0 && i < nodes.length) {
            let current = queue.shift()!;

            if (nodes[i] !== "null") {
                current.left = new TreeNode(parseInt(nodes[i]));
                queue.push(current.left);
            }
            i++;

            if (i < nodes.length && nodes[i] !== "null") {
                current.right = new TreeNode(parseInt(nodes[i]));
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
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;

    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

class Codec {

    // Encodes a tree to a single string
    serialize(root: TreeNode | null): string {
        let serialized: string[] = [];
        this.serializeHelper(root, serialized);
        return serialized.join(",");
    }

    private serializeHelper(node: TreeNode | null, serialized: string[]) {
        if (node === null) {
            serialized.push("null");
            return;
        }

        serialized.push(node.val.toString());
        this.serializeHelper(node.left, serialized);
        this.serializeHelper(node.right, serialized);
    }

    // Decodes your encoded data to tree
    deserialize(data: string): TreeNode | null {
        let nodes = data.split(",");
        let queue = nodes.reduce((acc, node) => {
            acc.push(node);
            return acc;
        }, [] as string[]);
        return this.deserializeHelper(queue);
    }

    private deserializeHelper(queue: string[]): TreeNode | null {
        let current = queue.shift();
        if (current === "null") {
            return null; // Return null for "null" nodes
        }

        let node = new TreeNode(parseInt(current));
        node.left = this.deserializeHelper(queue);
        node.right = this.deserializeHelper(queue);

        return node;
    }
}
```

