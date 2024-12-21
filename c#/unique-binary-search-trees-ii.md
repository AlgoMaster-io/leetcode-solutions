## Problem: [Leetcode 95: Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

### Approaches:
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

#### Intuition
To solve the problem of generating all unique BSTs, we utilize a recursive approach that constructs trees by considering each number from 1 to `n` as the root. For each selected root, we recursively generate all possible left subtrees and right subtrees that can be formed using the remaining numbers. The key is to combine each left subtree with each right subtree; this will give all unique BSTs for that specific root.

#### Algorithm
1. Define a recursive function `GenerateTrees(start, end)` that will generate all unique BSTs for numbers between `start` and `end`.
2. For each possible root from `start` to `end`:
   - Recursively generate all left subtrees using numbers less than the root.
   - Recursively generate all right subtrees using numbers greater than the root.
   - Combine each left subtree with each right subtree for the given root and store the resulting trees.
3. Return the list of all constructed trees.
4. Base case: If `start` > `end`, return a list containing `null` (empty tree).

#### Time and Space Complexity
- **Time Complexity:** `O((2^n) * n)`. This is the Catalan number because we are generating all structurally unique BSTs.
- **Space Complexity:** `O(2^n)`. Due to the recursion stack and the storage of trees.

```csharp
public class Solution {
    public IList<TreeNode> GenerateTrees(int n) {
        // Helper function to generate trees from start to end
        IList<TreeNode> GenerateTrees(int start, int end) {
            var allTrees = new List<TreeNode>();
            if (start > end) {
                allTrees.Add(null);
                return allTrees;
            }
            
            // Pick each number i as the root
            for (int i = start; i <= end; i++) {
                // Generate all left and right subtrees
                var leftTrees = GenerateTrees(start, i - 1);
                var rightTrees = GenerateTrees(i + 1, end);
                
                // Combine left and right subtrees with root i
                foreach (var l in leftTrees) {
                    foreach (var r in rightTrees) {
                        var currentTree = new TreeNode(i);
                        currentTree.left = l;
                        currentTree.right = r;
                        allTrees.Add(currentTree);
                    }
                }
            }
            
            return allTrees;
        }
        
        return n == 0 ? new List<TreeNode>() : GenerateTrees(1, n);
    }
}
```

### Dynamic Programming Approach

#### Intuition
We can use dynamic programming to optimize the recursive approach slightly by memoizing results. Each unique tree structure from a specific start to end can be calculated once and stored for reuse, reducing redundant calculations. However, since the trees themselves need to be constructed, this approach does not drastically change time complexity but can improve runtime slightly.

#### Algorithm
1. Use a map (or dictionary) to store already computed results for the given range (start, end).
2. Proceed similarly as the recursive method but check if the result is already computed for the range and reuse it.
3. This approach mainly helps avoid redundant calculations in large inputs by caching results.

#### Time and Space Complexity
- **Time Complexity:** `O((2^n) * n)`. Same as recursive due to the need to construct each tree.
- **Space Complexity:** `O(2^n)`. Storage for all unique subtrees generated, with additional space used for memoization.

```csharp
public class Solution {
    private Dictionary<(int, int), IList<TreeNode>> memo;
    
    public IList<TreeNode> GenerateTrees(int n) {
        if (n == 0) return new List<TreeNode>();
        memo = new Dictionary<(int, int), IList<TreeNode>>();
        return GenerateTrees(1, n);
    }
    
    private IList<TreeNode> GenerateTrees(int start, int end) {
        if (start > end) return new List<TreeNode> { null };
        
        if (memo.ContainsKey((start, end))) return memo[(start, end)];
        
        var allTrees = new List<TreeNode>();
        
        for (int i = start; i <= end; i++) {
            var leftTrees = GenerateTrees(start, i - 1);
            var rightTrees = GenerateTrees(i + 1, end);
            
            foreach (var left in leftTrees) {
                foreach (var right in rightTrees) {
                    var root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    allTrees.Add(root);
                }
            }
        }
        
        memo[(start, end)] = allTrees;
        return allTrees;
    }
}
```

The dynamic programming approach with memoization helps in reducing function calls that produce the same results, thus improving the execution time slightly. However, due to the nature of the problem, the extensive number of unique trees to be constructed keeps the overall time complexity significant.

