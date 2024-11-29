# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
csharp
```csharp
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<TreeNode> GenerateTrees(int n) {
        if (n == 0) return new List<TreeNode>();
        return GenerateTreesHelper(1, n);
    }

    private IList<TreeNode> GenerateTreesHelper(int start, int end) {
        IList<TreeNode> allTrees = new List<TreeNode>();
        if (start > end) {
            allTrees.Add(null);
            return allTrees;
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; i++) {
            // Generate all left and right subtrees
            IList<TreeNode> leftTrees = GenerateTreesHelper(start, i - 1);
            IList<TreeNode> rightTrees = GenerateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            foreach (var left in leftTrees) {
                foreach (var right in rightTrees) {
                    TreeNode currentTree = new TreeNode(i);
                    currentTree.left = left;
                    currentTree.right = right;
                    allTrees.Add(currentTree);
                }
            }
        }
        return allTrees;
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode() {}

    public TreeNode(int val) { this.val = val; }

    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
csharp
```csharp
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table
using System.Collections.Generic;

public class Solution {
    private Dictionary<string, IList<TreeNode>> memo;

    public IList<TreeNode> GenerateTrees(int n) {
        if (n == 0) return new List<TreeNode>();
        memo = new Dictionary<string, IList<TreeNode>>();
        return GenerateTreesHelper(1, n);
    }

    private IList<TreeNode> GenerateTreesHelper(int start, int end) {
        IList<TreeNode> allTrees = new List<TreeNode>();
        if (start > end) {
            allTrees.Add(null);
            return allTrees;
        }

        string memoKey = start + "-" + end;
        if (memo.ContainsKey(memoKey)) {
            return memo[memoKey];
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; i++) {
            // Generate all left and right subtrees
            IList<TreeNode> leftTrees = GenerateTreesHelper(start, i - 1);
            IList<TreeNode> rightTrees = GenerateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            foreach (var left in leftTrees) {
                foreach (var right in rightTrees) {
                    TreeNode currentTree = new TreeNode(i);
                    currentTree.left = left;
                    currentTree.right = right;
                    allTrees.Add(currentTree);
                }
            }
        }

        memo[memoKey] = allTrees;
        return allTrees;
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode() {}

    public TreeNode(int val) { this.val = val; }

    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

