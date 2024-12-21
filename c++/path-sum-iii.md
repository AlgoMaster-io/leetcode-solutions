[Leetcode Problem 437: Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach using Prefix Sum](#optimized-approach-using-prefix-sum)

---

## Brute Force Approach

### Intuition
The brute force solution involves iterating through each node in the binary tree and, for each node, calculating the number of paths with the given sum that start from that node. We achieve this by traversing downwards and exploring all possible paths originated from each node.

### Steps:
1. Define a helper function to calculate the number of paths with a given sum starting from the current node.
2. For each node, recursively calculate the paths starting from itself.
3. Include paths starting from the left child and right child as well.
4. The helper function will deal with situations where a path does not necessarily have to end at a leaf, but must go downwards.

### Detailed Implementation:
```cpp
class Solution {
public:
    // Helper function to count paths starting with specific node
    int countPathsFromNode(TreeNode* node, int targetSum) {
        if (node == nullptr) return 0;
        
        int paths = 0;
        if (node->val == targetSum) // Check if the node itself is a valid path
            paths++;
        
        // Explore the left and right subtrees
        paths += countPathsFromNode(node->left, targetSum - node->val);
        paths += countPathsFromNode(node->right, targetSum - node->val);
        
        return paths;
    }

    int pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) return 0;
        
        // Calculate the paths from the root node
        int paths = countPathsFromNode(root, targetSum);

        // Include paths from the left and right subnodes
        paths += pathSum(root->left, targetSum);
        paths += pathSum(root->right, targetSum);
        
        return paths;
    }
};
```
### Time Complexity
- O(N^2) in the worst case for a balanced tree, since for each node we may potentially consider all sub-nodes.
- O(N*log(N)) on average for a balanced tree.

### Space Complexity
- O(H) where H is the height of the tree, due to recursion stack space.

---

## Optimized Approach using Prefix Sum

### Intuition
The optimization involves using a prefix sum combined with a hashmap to get the path sum directly. This method reduces the need for repeated calculations by keeping track of the cumulative sum at each node and using previously computed sums to find the number of valid paths efficiently.

### Steps:
1. Use a recursive helper function with a prefix sum hashmap.
2. As we traverse the tree, calculate the current prefix sum.
3. Check how many times `currentSum - targetSum` has appeared in the prefix sum using the hashmap.
4. This count gives the number of valid paths with the target sum ending at the current node.

### Detailed Implementation:
```cpp
class Solution {
public:
    int pathSumHelper(TreeNode* node, int currentSum, int targetSum, unordered_map<int, int>& prefixSumCount) {
        if (node == nullptr) return 0;
        
        currentSum += node->val;
        int numPathsToCurr = prefixSumCount[currentSum - targetSum];
        
        // Save the current prefix sum in map
        prefixSumCount[currentSum]++;
        
        // Paths include left and right subtrees
        numPathsToCurr += pathSumHelper(node->left, currentSum, targetSum, prefixSumCount);
        numPathsToCurr += pathSumHelper(node->right, currentSum, targetSum, prefixSumCount);
        
        // Backtrack: remove current prefix sum since we're moving back to parent
        prefixSumCount[currentSum]--;
        
        return numPathsToCurr;
    }
    
    int pathSum(TreeNode* root, int targetSum) {
        unordered_map<int, int> prefixSumCount;
        prefixSumCount[0] = 1; // Base case for when path's sum equals the target sum itself
        return pathSumHelper(root, 0, targetSum, prefixSumCount);
    }
};
```

### Time Complexity
- O(N) where N is the number of nodes in the tree, since each node is visited once.

### Space Complexity
- O(H) where H is the height of the tree, due to the recursion stack space.
- Additional space for the hashmap, which in the worst case can take O(N) space.

