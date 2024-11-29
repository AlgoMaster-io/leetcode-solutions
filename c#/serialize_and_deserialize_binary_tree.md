# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue
using System;
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Codec {

    // Encodes a tree to a single string
    public string Serialize(TreeNode root) {
        if (root == null) {
            return "null"; // Return "null" for an empty tree
        }

        var serialized = new System.Text.StringBuilder();
        var queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while (queue.Count > 0) {
            TreeNode current = queue.Dequeue();
            if (current == null) {
                serialized.Append("null,");
            } else {
                serialized.Append(current.val).Append(",");
                queue.Enqueue(current.left);
                queue.Enqueue(current.right);
            }
        }

        return serialized.ToString();
    }

    // Decodes your encoded data to tree
    public TreeNode Deserialize(string data) {
        if (data == "null") {
            return null; // Return null for an empty tree
        }

        string[] nodes = data.Split(',');
        TreeNode root = new TreeNode(int.Parse(nodes[0]));
        var queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        int i = 1;
        while (queue.Count > 0 && i < nodes.Length) {
            TreeNode current = queue.Dequeue();

            if (nodes[i] != "null") {
                current.left = new TreeNode(int.Parse(nodes[i]));
                queue.Enqueue(current.left);
            }
            i++;

            if (i < nodes.Length && nodes[i] != "null") {
                current.right = new TreeNode(int.Parse(nodes[i]));
                queue.Enqueue(current.right);
            }
            i++;
        }

        return root;
    }
}
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string
using System;
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Codec {

    // Encodes a tree to a single string
    public string Serialize(TreeNode root) {
        if (root == null) {
            return "null"; // Return "null" for an empty tree
        }

        var serialized = new System.Text.StringBuilder();
        SerializeHelper(root, serialized);
        return serialized.ToString();
    }

    private void SerializeHelper(TreeNode node, System.Text.StringBuilder serialized) {
        if (node == null) {
            serialized.Append("null,");
            return;
        }

        serialized.Append(node.val).Append(",");
        SerializeHelper(node.left, serialized);
        SerializeHelper(node.right, serialized);
    }

    // Decodes your encoded data to tree
    public TreeNode Deserialize(string data) {
        string[] nodes = data.Split(',');
        var queue = new Queue<string>(nodes);
        return DeserializeHelper(queue);
    }

    private TreeNode DeserializeHelper(Queue<string> queue) {
        string current = queue.Dequeue();
        if (current == "null") {
            return null; // Return null for "null" nodes
        }

        TreeNode node = new TreeNode(int.Parse(current));
        node.left = DeserializeHelper(queue);
        node.right = DeserializeHelper(queue);

        return node;
    }
}
```

