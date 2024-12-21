## [LeetCode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

### Approaches
- [Approach 1: Breadth-First Search (BFS) using Queue](#bfs-queue)
- [Approach 2: Depth-First Search (DFS) using Preorder Traversal](#dfs-preorder)

---

### Approach 1: Breadth-First Search (BFS) using Queue

#### Intuition:
The idea is to perform a level-order traversal of the binary tree. During serialization, we will add the value of each node to the result list, and for null nodes, we will specifically add "N" to indicate the absence of a node. During deserialization, we split the serialized string and reconstruct the binary tree level by level.

#### Code:
```csharp
public class Codec {

    // Encodes a tree to a single string.
    public string serialize(TreeNode root) {
        if (root == null) return "";
        
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        
        while (queue.Count > 0) {
            TreeNode node = queue.Dequeue();
            if (node == null) {
                sb.Append("N ");
                continue;
            }
            
            sb.Append(node.val + " ");
            queue.Enqueue(node.left);
            queue.Enqueue(node.right);
        }
        
        return sb.ToString().TrimEnd();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(string data) {
        if (data == "") return null;
        
        string[] values = data.Split(' ');
        TreeNode root = new TreeNode(int.Parse(values[0]));
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        int i = 1;
        
        while (queue.Count > 0) {
            TreeNode node = queue.Dequeue();
            
            if (values[i] != "N") {
                TreeNode leftNode = new TreeNode(int.Parse(values[i]));
                node.left = leftNode;
                queue.Enqueue(leftNode);
            }
            i++;
            
            if (values[i] != "N") {
                TreeNode rightNode = new TreeNode(int.Parse(values[i]));
                node.right = rightNode;
                queue.Enqueue(rightNode);
            }
            i++;
        }
        
        return root;
    }
}
```

#### Time and Space Complexities:
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. We visit each node exactly once during both serialization and deserialization.
- **Space Complexity**: O(N), we store N nodes in the queue at maximum.

---

### Approach 2: Depth-First Search (DFS) using Preorder Traversal

#### Intuition:
In this approach, we use preorder traversal for both serialization and deserialization. During serialization, we traverse the tree in a preorder fashion while recording values. For null nodes, we add a special marker. During deserialization, we recreate the tree using a recursive approach utilizing the preorder values.

#### Code:
```csharp
public class Codec {

    // Encodes a tree to a single string.
    public string serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.ToString().TrimEnd();
    }
    
    private void serializeHelper(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.Append("N ");
            return;
        }
        
        sb.Append(node.val + " ");
        serializeHelper(node.left, sb);
        serializeHelper(node.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(string data) {
        if (data == "") return null;
        
        Queue<string> values = new Queue<string>(data.Split(' '));
        return deserializeHelper(values);
    }
    
    private TreeNode deserializeHelper(Queue<string> values) {
        string value = values.Dequeue();
        
        if (value == "N") return null;
        
        TreeNode node = new TreeNode(int.Parse(value));
        node.left = deserializeHelper(values);
        node.right = deserializeHelper(values);
        return node;
    }
}
```

#### Time and Space Complexities:
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. We process each node exactly once.
- **Space Complexity**: O(N), for storing the preorder traversal in the queue and recursion stack space.

