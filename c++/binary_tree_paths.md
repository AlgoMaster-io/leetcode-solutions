# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
#include <string>
#include <iostream>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        if (root != nullptr) {
            dfs(root, "", result);
        }
        return result;
    }

private:
    void dfs(TreeNode* node, string path, vector<string>& result) {
        // Append the current node's value to the path
        path += to_string(node->val);

        // If the current node is a leaf, add the path to the result
        if (node->left == nullptr && node->right == nullptr) {
            result.push_back(path);
            return;
        }

        // If not a leaf, continue the path with "->" and recurse
        if (node->left != nullptr) {
            dfs(node->left, path + "->", result);
        }
        if (node->right != nullptr) {
            dfs(node->right, path + "->", result);
        }
    }
};
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage
#include <vector>
#include <string>
#include <stack>
#include <iostream>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        if (root == nullptr) {
            return result; // Return empty result if the tree is empty
        }

        stack<TreeNode*> nodeStack;
        stack<string> pathStack;
        nodeStack.push(root);
        pathStack.push(to_string(root->val));

        while (!nodeStack.empty()) {
            TreeNode* currentNode = nodeStack.top();
            nodeStack.pop();
            string currentPath = pathStack.top();
            pathStack.pop();

            // If the current node is a leaf, add the path to the result
            if (currentNode->left == nullptr && currentNode->right == nullptr) {
                result.push_back(currentPath);
            }

            // If not a leaf, push children to the stack with updated paths
            if (currentNode->right != nullptr) {
                nodeStack.push(currentNode->right);
                pathStack.push(currentPath + "->" + to_string(currentNode->right->val));
            }
            if (currentNode->left != nullptr) {
                nodeStack.push(currentNode->left);
                pathStack.push(currentPath + "->" + to_string(currentNode->left->val));
            }
        }

        return result;
    }
};
```

## Approach 3: Backtracking

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
#include <string>
#include <iostream>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        if (root == nullptr) {
            return result; // Return empty result if the tree is empty
        }
        backtrack(root, "", result);
        return result;
    }

private:
    void backtrack(TreeNode* node, string path, vector<string>& result) {
        size_t len = path.length();
        path += to_string(node->val);

        // If the current node is a leaf, add the path to the result
        if (node->left == nullptr && node->right == nullptr) {
            result.push_back(path);
        } else {
            // If not a leaf, continue the path
            path += "->";
            if (node->left != nullptr) {
                backtrack(node->left, path, result);
            }
            if (node->right != nullptr) {
                backtrack(node->right, path, result);
            }
        }

        // Backtrack to remove the current node's value and "->"
        path.resize(len);
    }
};
```

