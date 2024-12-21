# [Leetcode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

### Approaches:
- [Approach 1: Recursive Pre-order Traversal](#approach-1:-recursive-pre-order-traversal)
- [Approach 2: Iterative BFS using Queue](#approach-2:-iterative-bfs-using-queue)
- [Approach 3: Iterative DFS using Stack](#approach-3:-iterative-dfs-using-stack)

---

## Approach 1: Recursive Pre-order Traversal

### Intuition:
Serialization can be simplified by performing a recursive pre-order traversal of the tree, converting each node value to a string, and using a delimiter (e.g., comma) to separate values. In deserialization, we parse the string representation back into a tree using recursion with an index to track the position of the next value to process.

### Solution:

```cpp
class Codec {
public:

    // Serializes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "#";
        return to_string(root->val) + "," + serialize(root->left) + "," + serialize(root->right);
    }

    // Deserializes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> nodes;
        stringstream ss(data);
        string node;
        while (getline(ss, node, ',')) {
            nodes.push(node);
        }
        return deserializeHelper(nodes);
    }

private:

    TreeNode* deserializeHelper(queue<string>& nodes) {
        string value = nodes.front();
        nodes.pop();
        if (value == "#") return nullptr;
        TreeNode* root = new TreeNode(stoi(value));
        root->left = deserializeHelper(nodes);
        root->right = deserializeHelper(nodes);
        return root;
    }
};
```

### Time Complexity:
- **Serialization**: O(n), where n is the number of nodes in the tree, since we visit each node exactly once.
- **Deserialization**: O(n), as we process each node string exactly once from the queue.

### Space Complexity:
- O(n), due to the space needed to store the result string in serialization, and the space to store the queue in deserialization.


---

## Approach 2: Iterative BFS using Queue

### Intuition:
Instead of recursion, we can use a queue to perform BFS on the tree during both serialization and deserialization. We maintain the order of nodes the same way by enqueuing children left to right.

### Solution:

```cpp
class Codec {
public:

    // Serializes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "#";
        string result;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            if (node) {
                result += to_string(node->val) + ",";
                q.push(node->left);
                q.push(node->right);
            } else {
                result += "#,";
            }
        }
        return result;
    }

    // Deserializes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "#") return nullptr;
        stringstream ss(data);
        string value;
        getline(ss, value, ',');
        TreeNode* root = new TreeNode(stoi(value));
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            if (getline(ss, value, ',')) {
                if (value != "#") {
                    node->left = new TreeNode(stoi(value));
                    q.push(node->left);
                }
            }
            if (getline(ss, value, ',')) {
                if (value != "#") {
                    node->right = new TreeNode(stoi(value));
                    q.push(node->right);
                }
            }
        }
        return root;
    }
};
```

### Time Complexity:
- **Serialization**: O(n), where n is the number of nodes since we visit each node once.
- **Deserialization**: O(n), as we process each node similarly.

### Space Complexity:
- O(n), for the queue used during serialization and deserialization to store nodes.

---

## Approach 3: Iterative DFS using Stack

### Intuition:
A DFS approach using a stack can also be utilized for serialization/deserialization. This method explicitly manages memory stack vs recursion stack, which makes it iterative and potentially more suitable for very deep trees.

### Solution:

```cpp
class Codec {
public:

    // Serializes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "#";
        stack<TreeNode*> stack;
        stack.push(root);
        string result;
        while (!stack.empty()) {
            TreeNode* node = stack.top();
            stack.pop();
            if (node) {
                result += to_string(node->val) + ",";
                stack.push(node->right);
                stack.push(node->left);
            } else {
                result += "#,";
            }
        }
        return result;
    }

    // Deserializes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "#") return nullptr;
        istringstream iss(data);
        string value;
        getline(iss, value, ',');
        TreeNode* root = new TreeNode(stoi(value));
        stack<TreeNode*> stack;
        stack.push(root);
        TreeNode* current = root;
        while (getline(iss, value, ',')) {
            if (value != "#") {
                TreeNode* node = new TreeNode(stoi(value));
                if (current->left) {
                    current->right = node;
                    stack.pop();
                } else {
                    current->left = node;
                }
                stack.push(node);
                current = node;
            } else {
                if (!stack.empty()) {
                    current = stack.top();
                }
            }
        }
        return root;
    }
};
```

### Time Complexity:
- **Serialization**: O(n), for visiting all nodes.
- **Deserialization**: O(n), for reconstructing all nodes.

### Space Complexity:
- O(n), due to the storage required for the stack to maintain nodes during DFS.

