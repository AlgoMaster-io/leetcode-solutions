# [Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

## Table of Contents
- [Approach 1: Serialize Subtrees using String Concatenation](#approach-1-serialize-subtrees-using-string-concatenation)
- [Approach 2: Serialize Subtrees using Unique Identifiers](#approach-2-serialize-subtrees-using-unique-identifiers)

### Approach 1: Serialize Subtrees using String Concatenation

**Intuition:**
The basic idea is to serialize each subtree into a string format. By using a hashmap, we can count how many times we've seen the same serialization. If a serialization appears more than once, it means there are duplicate subtrees.

**Algorithm:**
1. For each node, construct a string representation of the subtree rooted at that node.
2. Use a map to record the frequency of each subtree serialization.
3. If any subtree serialization has a frequency greater than 1, add the root of that subtree to the result.

**Implementation:**

```cpp
#include <unordered_map>
#include <vector>
#include <string>
#include <utility> 

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    std::vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        std::unordered_map<std::string, int> subtreeCount;
        std::vector<TreeNode*> duplicates;
        serializeTree(root, subtreeCount, duplicates);
        return duplicates;
    }

private:
    std::string serializeTree(TreeNode* node, std::unordered_map<std::string, int>& subtreeCount, std::vector<TreeNode*>& duplicates) {
        if (!node) return "#"; // Null node representation.

        // Recursively serialize left and right subtrees.
        std::string leftSubtree = serializeTree(node->left, subtreeCount, duplicates);
        std::string rightSubtree = serializeTree(node->right, subtreeCount, duplicates);

        // Form the current subtree's unique string representation.
        std::string serialized = std::to_string(node->val) + "," + leftSubtree + "," + rightSubtree;

        // Record the serialized subtree and check for duplicates.
        if (++subtreeCount[serialized] == 2) {
            duplicates.push_back(node);
        }

        return serialized;
    }
};
```

**Time Complexity:** O(n^2) in the worst case due to the string concatenation and comparison operations, where n is the number of nodes.

**Space Complexity:** O(n) for the storage of the serializations and nodes.

### Approach 2: Serialize Subtrees using Unique Identifiers

**Intuition:**
Instead of using strings, we use unique identifiers for each subtree. This can help reduce the time complexity by avoiding costly string operations. We use a hashmap to store mappings of subtree structures to unique identifiers.

**Algorithm:**
1. Traverse the tree and generate unique IDs using the 3-element tuple: (node value, left child ID, right child ID).
2. Use a map to count the occurrences of these tuples.
3. If any tuple has a frequency greater than 1, add the corresponding subtree root to the result.

**Implementation:**

```cpp
#include <unordered_map>
#include <vector>
#include <tuple>
#include <utility> 

class Solution {
public:
    std::vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        std::unordered_map<std::string, int> idsMap;
        std::unordered_map<int, int> countMap;
        std::vector<TreeNode*> duplicates;
        issueId = 0;
        collect(root, idsMap, countMap, duplicates);
        return duplicates;
    }

private:
    int collect(TreeNode* node, std::unordered_map<std::string, int>& idsMap, std::unordered_map<int, int>& countMap, std::vector<TreeNode*>& duplicates) {
        if (!node) return 0;
        
        // Create a unique identifier for the current subtree
        std::string serialized = std::to_string(node->val) + "," + std::to_string(collect(node->left, idsMap, countMap, duplicates)) + "," + std::to_string(collect(node->right, idsMap, countMap, duplicates));
        
        // Use the id for storing serialized subtree
        int subtreeId = 0;
        if (idsMap.find(serialized) == idsMap.end()) {
            subtreeId = ++issueId;
            idsMap[serialized] = subtreeId;
        } else {
            subtreeId = idsMap[serialized];
        }

        // If a duplicate subtree is detected, record its root in duplicates
        if (++countMap[subtreeId] == 2) {
            duplicates.push_back(node);
        }

        return subtreeId;
    }

    int issueId;
};
```

**Time Complexity:** O(n) where n is the number of nodes, as we are simply visiting each node once with efficient hashing operations.

**Space Complexity:** O(n) due to the storage of IDs and nodes in the map.

