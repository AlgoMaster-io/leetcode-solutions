# 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Approach 1: Recursive Generation

### Solution
```java
// Time Complexity: Catalan number time complexity (approximately O(4^n / n^(3/2)))
// Space Complexity: Catalan space complexity due to recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) return new ArrayList<>();
        return generateTreesHelper(1, n);
    }

    private List<TreeNode> generateTreesHelper(int start, int end) {
        List<TreeNode> allTrees = new ArrayList<>();
        if (start > end) {
            allTrees.add(null);
            return allTrees;
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; i++) {
            // Generate all left and right subtrees
            List<TreeNode> leftTrees = generateTreesHelper(start, i - 1);
            List<TreeNode> rightTrees = generateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode currentTree = new TreeNode(i);
                    currentTree.left = left;
                    currentTree.right = right;
                    allTrees.add(currentTree);
                }
            }
        }
        return allTrees;
    }
}

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {}

    TreeNode(int val) { this.val = val; }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## Approach 2: Dynamic Programming (Memoization) - Optimized Recursive

### Solution
```java
// Time Complexity: Catalan number time complexity
// Space Complexity: O(n^2) for the memoization table
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;
import java.util.Map;

public class Solution {
    private Map<String, List<TreeNode>> memo;

    public List<TreeNode> generateTrees(int n) {
        if (n == 0) return new ArrayList<>();
        memo = new HashMap<>();
        return generateTreesHelper(1, n);
    }

    private List<TreeNode> generateTreesHelper(int start, int end) {
        List<TreeNode> allTrees = new ArrayList<>();
        if (start > end) {
            allTrees.add(null);
            return allTrees;
        }

        String memoKey = start + "-" + end;
        if (memo.containsKey(memoKey)) {
            return memo.get(memoKey);
        }

        // Iterate through each number as the root
        for (int i = start; i <= end; i++) {
            // Generate all left and right subtrees
            List<TreeNode> leftTrees = generateTreesHelper(start, i - 1);
            List<TreeNode> rightTrees = generateTreesHelper(i + 1, end);

            // Combine left and right subtrees with the root i
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode currentTree = new TreeNode(i);
                    currentTree.left = left;
                    currentTree.right = right;
                    allTrees.add(currentTree);
                }
            }
        }

        memo.put(memoKey, allTrees);
        return allTrees;
    }
}

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {}

    TreeNode(int val) { this.val = val; }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

The recursive approach builds trees by choosing different nodes as roots and generating all possible left and right subtrees. The memoization approach improves efficiency by storing already calculated results to avoid redundant calculations.

