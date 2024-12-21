# [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approaches

1. [Breadth-First Search with Queue](#approach-1-bfs-with-queue)
2. [Depth-First Search with Preorder Traversal & StringBuilder](#approach-2-dfs-with-preorder-traversal-stringbuilder)

---

## Approach 1: BFS with Queue

### Intuition

This approach uses a queue to perform a level-order traversal (BFS) to serialize and deserialize a binary tree. The BFS approach processes nodes level by level, which suits the serialization of nodes where it can record the tree's structure, including null children, by utilizing a delimiter.

### Serialize

1. **Start**: Initialize a queue and add the root to it.
2. **Traverse**: While the queue is not empty:
   - Dequeue an element. If it is not null, add its value to the result string and enqueue its children. If it is null, add a special marker (e.g., "null") to the result string.
3. **Output**: Join all results with a comma.

### Deserialize

1. **Start**: Split the serialized string on commas and initialize a queue.
2. **Initialize**: Create the root node and add it to the queue.
3. **Reconstruct**: For each node, check its children from the serialized data:
   - If it is "null", move to the next node.
   - Otherwise, create a new node, link it as a child, and add it to the queue.
   
### Code

```java
// Definition for a binary tree node.
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "null";
        
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                sb.append("null,");
            } else {
                sb.append(node.val).append(",");
                queue.add(node.left);
                queue.add(node.right);
            }
        }
        
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.equals("null")) return null;
        
        String[] nodes = data.split(",");
        Queue<TreeNode> queue = new LinkedList<>();
        TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));
        queue.add(root);
        
        int i = 1;
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (!nodes[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(node.left);
            }
            i++;
            if (!nodes[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(node.right);
            }
            i++;
        }
        
        return root;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the tree, as each node is processed once during serialization and deserialization.
- **Space Complexity**: O(N), due to the storage of nodes in the queue and the string building process for serialization.

---

## Approach 2: DFS with Preorder Traversal & StringBuilder

### Intuition

In this approach, the idea is to use a depth-first search (DFS) technique with a recursive method that uses preorder traversal. This involves capturing each node followed by its left and right children. By marking null references explicitly, reconstruction is facilitated.

### Serialize

1. Use a recursive function to append node values to a `StringBuilder` in preorder (root → left → right).
2. Append a special marker ("null") for null nodes to denote absent children.
3. Join all elements to generate the final serialized string.

### Deserialize

1. Split the serialized data using a delimiter.
2. Use recursive functions to construct tree nodes in preorder, consuming values sequentially:
   - Return null when a "null" string is found.
   - Create a tree node using the current value and recursively handle its left and right subtrees.

### Code

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append("null,");
            return;
        }
        sb.append(root.val).append(",");
        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);
    }
    
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        int[] index = {0};  // Mutable integer to track position in nodes array
        return deserializeHelper(nodes, index);
    }
    
    private TreeNode deserializeHelper(String[] nodes, int[] index) {
        if (nodes[index[0]].equals("null")) {
            index[0]++;
            return null;
        }
        TreeNode node = new TreeNode(Integer.parseInt(nodes[index[0]++]));
        node.left = deserializeHelper(nodes, index);
        node.right = deserializeHelper(nodes, index);
        return node;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(N), due to the recursion stack in the worst case (when the tree is skewed) and the storage for serialization strings.

