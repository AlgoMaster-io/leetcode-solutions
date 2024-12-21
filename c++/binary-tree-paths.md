# [Leetcode 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approaches
* [Approach 1: Depth-First Search (DFS) with Recursion - Naive](#approach-1-depth-first-search-dfs-with-recursion-naive)
* [Approach 2: Depth-First Search (DFS) with Recursion - String Optimization](#approach-2-depth-first-search-dfs-with-recursion-string-optimization)
* [Approach 3: Depth-First Search (DFS) with Iteration](#approach-3-depth-first-search-dfs-with-iteration)

## Approach 1: Depth-First Search (DFS) with Recursion - Naive

### Intuition
The simplest approach to the problem of finding all root-to-leaf paths in a binary tree is to use a depth-first search (DFS) using recursion. The idea is to explore each path from the root to a leaf, keeping track of the current path as we go, and storing the path in a list once a leaf node is reached.

### Steps
1. Check if the root is NULL, if so, return an empty list as there are no paths.
2. Create a helper function that takes the current node, a string representing the current path, and a list to store all paths.
3. If the current node is a leaf node, append the current path plus the current node's value to the list.
4. If not a leaf, recursively call the function for the left and right child nodes, appending the current node's value and "->" to the current path.

### Code
```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> paths;
        if (root == nullptr) return paths; // Return empty if tree is empty
        traverseTree(root, "", paths); // Start traversal
        return paths; // Return all collected paths
    }

private:
    void traverseTree(TreeNode* node, string path, vector<string>& paths) {
        if (!node) return; // Base case: if node is NULL, return
        
        path += to_string(node->val); // Add the current node value to the path
        
        if (!node->left && !node->right) { // If it is a leaf node
            paths.push_back(path); // Store the complete path
        } else {
            path += "->"; // Add arrow to indicate path
            traverseTree(node->left, path, paths); // Recurse left
            traverseTree(node->right, path, paths); // Recurse right
        }
    }
};
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the tree because each node is visited once.
- **Space Complexity:** O(H), where H is the height of the tree due to the recursive stack.

## Approach 2: Depth-First Search (DFS) with Recursion - String Optimization

### Intuition
In the naive approach, strings are concatenated during each recursion which can be inefficient as it involves creating new strings in each recursive call. By handling the paths more efficiently, using a vector of integers which only gets converted to a string at a leaf, we can optimize space usage.

### Steps
1. Similar to Approach 1, but use a vector of integers to store the nodes in the current path.
2. On reaching a leaf node, convert the path to a string path.
3. Recur for left and right children.

### Code
```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> paths;
        vector<int> currentPath;
        if (root == nullptr) return paths; // Return empty if tree is empty
        traverseTree(root, currentPath, paths); // Start traversal
        return paths; // Return all collected paths
    }

private:
    void traverseTree(TreeNode* node, vector<int>& currentPath, vector<string>& paths) {
        if (!node) return; // Base case: if node is NULL, return
        currentPath.push_back(node->val); // Add current node to the path
        
        if (!node->left && !node->right) { // If it is a leaf node
            string path = convertPathToString(currentPath);
            paths.push_back(path); // Store the complete path
        } 
        else {
            traverseTree(node->left, currentPath, paths); // Recurse left
            traverseTree(node->right, currentPath, paths); // Recurse right
        }

        currentPath.pop_back(); // Backtracking: remove the current node from path
    }

    string convertPathToString(const vector<int>& path) {
        stringstream ss;
        for (int i = 0; i < path.size(); ++i) {
            if (i > 0) ss << "->"; // Add arrow for all nodes except the first
            ss << path[i];
        }
        return ss.str();
    }
};
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes in the tree because each node is visited once.
- **Space Complexity:** O(N), for both the recursive stack and to store the current path at any point.

## Approach 3: Depth-First Search (DFS) with Iteration

### Intuition
Instead of recursion, an iterative DFS can be conducted using a stack to hold nodes along with the path leading to them. This avoids the call stack overhead and provides a clear approach to handling the DFS iteratively.

### Steps
1. Utilize a stack to carry tuples of nodes and current path.
2. While the stack is not empty, pop an element.
3. If the current node is a leaf, add the path to the list.
4. If not, push its children onto the stack with the updated path.

### Code
```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> paths;
        if (root == nullptr) return paths; // Return empty if tree is empty
        stack<pair<TreeNode*, string>> s;
        s.push({root, ""});

        while (!s.empty()) {
            auto [node, path] = s.top();
            s.pop();

            if (!node) continue;

            path += to_string(node->val); // Add current node value to the path

            if (!node->left && !node->right) { // If it is a leaf node
                paths.push_back(path); // Add path to results
            } else {
                path += "->"; // Add separator for the path
                if (node->right) s.push({node->right, path});
                if (node->left) s.push({node->left, path});
            }
        }

        return paths; // Return all collected paths
    }
};
```

### Complexity
- **Time Complexity:** O(N), where N is the number of nodes since each node is processed once.
- **Space Complexity:** O(N), for the stack used to store nodes during DFS traversal.

These approaches cover various ways to solve the problem of finding all root-to-leaf paths in a binary tree, catering to both recursion and iteration preferences.

