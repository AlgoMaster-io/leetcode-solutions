[Leetcode Problem 95: Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approaches
- [Approach 1: Recursive Tree Construction](#approach-1-recursive-tree-construction)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)

### Approach 1: Recursive Tree Construction

**Intuition:**

The problem requires us to generate all structurally unique BSTs that store values from 1 to n. A recursive solution is suitable here because a tree can be constructed by picking any number `i` as the root, all numbers smaller than `i` form the left subtree, and all numbers larger form the right subtree. This recursive property facilitates constructing all possible trees.

1. For each number from 1 to n, pick it as a root.
2. Recursively generate all left subtrees using numbers less than the current root.
3. Recursively generate all right subtrees using numbers greater than the current root.
4. Combine each left subtree with each right subtree, adding the current root node.

This approach generates all possible BSTs by considering each number as the root.

**C++ Code:**

```cpp
#include <vector>
using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        return buildBST(1, n);
    }
    
    vector<TreeNode*> buildBST(int start, int end) {
        vector<TreeNode*> allTrees;
        
        // Base case
        if (start > end) {
            allTrees.push_back(nullptr);
            return allTrees;
        }
        
        // Iterate over all values as root
        for (int i = start; i <= end; i++) {
            // Generate all possible left subtrees
            vector<TreeNode*> leftTrees = buildBST(start, i - 1);
            
            // Generate all possible right subtrees
            vector<TreeNode*> rightTrees = buildBST(i + 1, end);
            
            // Connect each left and right subtree to the current root 'i'
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

**Time Complexity:** \(O(C_n \cdot n)\), where \(C_n\) is the nth Catalan number (possible BSTs), derived from G(n) = ∑ G(i-1)*G(n-i), and n is time spent copying tree nodes.

**Space Complexity:** \(O(C_n \cdot n)\) to store all trees.

### Approach 2: Dynamic Programming with Memoization

**Intuition:**

The recursive solution revisits the same subproblems multiple times, causing unnecessary computations. With memoization, repeated calculations can be stored and reused, thus optimizing the solution. This approach can potentially reduce the time complexity due to efficient use of previously computed results.

1. Create a memoization map to store solutions for each range.
2. Use a similar recursive structure but store already computed results for each range in the map.
3. Reuse results from the map to save computation time.

**C++ Code:**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
private:
    unordered_map<string, vector<TreeNode*>> memo;
    
    string getKey(int start, int end) {
        return to_string(start) + "-" + to_string(end);
    }
    
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        return buildBST(1, n);
    }
    
    vector<TreeNode*> buildBST(int start, int end) {
        vector<TreeNode*> allTrees;
        
        // Base case
        if (start > end) {
            allTrees.push_back(nullptr);
            return allTrees;
        }
        
        // Memoization lookup
        string key = getKey(start, end);
        if (memo.find(key) != memo.end()) {
            return memo[key];
        }
        
        // Iterate over all values as root
        for (int i = start; i <= end; i++) {
            // Generate all possible left subtrees
            vector<TreeNode*> leftTrees = buildBST(start, i - 1);
            
            // Generate all possible right subtrees
            vector<TreeNode*> rightTrees = buildBST(i + 1, end);
            
            // Connect each left and right subtree to the current root 'i'
            for (TreeNode* left : leftTrees) {
                for (TreeNode* right : rightTrees) {
                    TreeNode* currentTree = new TreeNode(i);
                    currentTree->left = left;
                    currentTree->right = right;
                    allTrees.push_back(currentTree);
                }
            }
        }
        
        // Save the result in memoization map
        memo[key] = allTrees;
        
        return allTrees;
    }
};
```

**Time Complexity:** \(O(C_n \cdot n)\), similar to the previous approach as Catalan numbers still define the subproblem split.

**Space Complexity:** \(O(C_n \cdot n \cdot m)\), with m being the memory overhead for the memoization store.

