# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue
#include <string>
#include <queue>
#include <sstream>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Codec {
public:

    // Encodes a tree to a single string
    std::string serialize(TreeNode* root) {
        if (root == nullptr) {
            return "null"; // Return "null" for an empty tree
        }

        std::ostringstream serialized;
        std::queue<TreeNode*> queue;
        queue.push(root);

        while (!queue.empty()) {
            TreeNode* current = queue.front();
            queue.pop();
            if (current == nullptr) {
                serialized << "null,";
            } else {
                serialized << current->val << ",";
                queue.push(current->left);
                queue.push(current->right);
            }
        }

        return serialized.str();
    }

    // Decodes your encoded data to tree
    TreeNode* deserialize(std::string data) {
        if (data == "null") {
            return nullptr; // Return null for an empty tree
        }

        std::stringstream ss(data);
        std::string nodeValue;
        std::getline(ss, nodeValue, ',');
        
        TreeNode* root = new TreeNode(std::stoi(nodeValue));
        std::queue<TreeNode*> queue;
        queue.push(root);

        while (!queue.empty()) {
            TreeNode* current = queue.front();
            queue.pop();

            if (std::getline(ss, nodeValue, ',') && nodeValue != "null") {
                current->left = new TreeNode(std::stoi(nodeValue));
                queue.push(current->left);
            }
            
            if (std::getline(ss, nodeValue, ',') && nodeValue != "null") {
                current->right = new TreeNode(std::stoi(nodeValue));
                queue.push(current->right);
            }
        }

        return root;
    }
};
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string
#include <string>
#include <queue>
#include <sstream>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Codec {
public:

    // Encodes a tree to a single string
    std::string serialize(TreeNode* root) {
        if (root == nullptr) {
            return "null"; // Return "null" for an empty tree
        }

        std::ostringstream serialized;
        serializeHelper(root, serialized);
        return serialized.str();
    }

private:
    void serializeHelper(TreeNode* node, std::ostringstream& serialized) {
        if (node == nullptr) {
            serialized << "null,";
            return;
        }

        serialized << node->val << ",";
        serializeHelper(node->left, serialized);
        serializeHelper(node->right, serialized);
    }

public:
    // Decodes your encoded data to tree
    TreeNode* deserialize(std::string data) {
        std::queue<std::string> q;
        std::stringstream ss(data);
        std::string nodeValue;

        while (std::getline(ss, nodeValue, ',')) {
            q.push(nodeValue);
        }
        
        return deserializeHelper(q);
    }

private:
    TreeNode* deserializeHelper(std::queue<std::string>& queue) {
        std::string current = queue.front();
        queue.pop();
        
        if (current == "null") {
            return nullptr; // Return null for "null" nodes
        }

        TreeNode* node = new TreeNode(std::stoi(current));
        node->left = deserializeHelper(queue);
        node->right = deserializeHelper(queue);

        return node;
    }
};
```

