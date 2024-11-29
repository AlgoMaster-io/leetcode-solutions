# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue
import java.util.LinkedList;
import java.util.Queue;

public class Codec {

    // Encodes a tree to a single string
    public String serialize(TreeNode root) {
        if (root == null) {
            return "null"; // Return "null" for an empty tree
        }

        StringBuilder serialized = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            if (current == null) {
                serialized.append("null,");
            } else {
                serialized.append(current.val).append(",");
                queue.add(current.left);
                queue.add(current.right);
            }
        }

        return serialized.toString();
    }

    // Decodes your encoded data to tree
    public TreeNode deserialize(String data) {
        if (data.equals("null")) {
            return null; // Return null for an empty tree
        }

        String[] nodes = data.split(",");
        TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        int i = 1;
        while (!queue.isEmpty() && i < nodes.length) {
            TreeNode current = queue.poll();

            if (!nodes[i].equals("null")) {
                current.left = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(current.left);
            }
            i++;

            if (i < nodes.length && !nodes[i].equals("null")) {
                current.right = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(current.right);
            }
            i++;
        }

        return root;
    }
}
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string
import java.util.LinkedList;
import java.util.Queue;

public class Codec {

    // Encodes a tree to a single string
    public String serialize(TreeNode root) {
        if (root == null) {
            return "null"; // Return "null" for an empty tree
        }

        StringBuilder serialized = new StringBuilder();
        serializeHelper(root, serialized);
        return serialized.toString();
    }

    private void serializeHelper(TreeNode node, StringBuilder serialized) {
        if (node == null) {
            serialized.append("null,");
            return;
        }

        serialized.append(node.val).append(",");
        serializeHelper(node.left, serialized);
        serializeHelper(node.right, serialized);
    }

    // Decodes your encoded data to tree
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        Queue<String> queue = new LinkedList<>();
        for (String node : nodes) {
            queue.add(node);
        }
        return deserializeHelper(queue);
    }

    private TreeNode deserializeHelper(Queue<String> queue) {
        String current = queue.poll();
        if (current.equals("null")) {
            return null; // Return null for "null" nodes
        }

        TreeNode node = new TreeNode(Integer.parseInt(current));
        node.left = deserializeHelper(queue);
        node.right = deserializeHelper(queue);

        return node;
    }
}
```