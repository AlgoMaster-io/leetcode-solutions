# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
```cpp
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, TreeNode* right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return vector<TreeNode*>{};
        return generateTreesHelper(1, n);
    }

private:
    vector<TreeNode*> generateTreesHelper(int start, int end) {
        vector<TreeNode*> allTrees;
        if (start > end) {
            allTrees.push_back(nullptr);
            return allTrees;
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; ++i) {
            // Generate all left and right subtrees
            vector<TreeNode*> leftTrees = generateTreesHelper(start, i - 1);
            vector<TreeNode*> rightTrees = generateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            for (TreeNode* left : leftTrees) {
                for (TreeNode* right : rightTrees) {
                    TreeNode* currentTree = new TreeNode(i);
                    currentTree->left = left;
                    currentTree->right = right;
                    allTrees.push_back(currentTree);
                }
            }
        }
        return allTrees;
    }
};
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
```cpp
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table
#include <unordered_map>
#include <string>
#include <vector>

using namespace std;

class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return vector<TreeNode*>{};
        memo.clear();
        return generateTreesHelper(1, n);
    }

private:
    unordered_map<string, vector<TreeNode*>> memo;

    vector<TreeNode*> generateTreesHelper(int start, int end) {
        vector<TreeNode*> allTrees;
        if (start > end) {
            allTrees.push_back(nullptr);
            return allTrees;
        }

        string memoKey = to_string(start) + "-" + to_string(end);
        if (memo.find(memoKey) != memo.end()) {
            return memo[memoKey];
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; ++i) {
            // Generate all left and right subtrees
            vector<TreeNode*> leftTrees = generateTreesHelper(start, i - 1);
            vector<TreeNode*> rightTrees = generateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            for (TreeNode* left : leftTrees) {
                for (TreeNode* right : rightTrees) {
                    TreeNode* currentTree = new TreeNode(i);
                    currentTree->left = left;
                    currentTree->right = right;
                    allTrees.push_back(currentTree);
                }
            }
        }

        memo[memoKey] = allTrees;
        return allTrees;
    }
};
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

