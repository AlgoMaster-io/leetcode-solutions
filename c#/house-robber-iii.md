# [Leetcode 337: House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approaches
- [Brute Force Recursion](#brute-force-recursion)
- [Memoization](#memoization)
- [Tree DP (Dynamic Programming)](#tree-dp)

---

### Brute Force Recursion

#### Intuition
The problem requires maximizing the amount of money you can rob from a binary tree where each node contains a value. The constraint is that you cannot rob two directly connected nodes, which leads to a classic "House Robber" problem that spreads out into a tree structure. The brute force solution would involve exploring every possibility by recursively deciding for each node whether to rob it or not.

#### Approach
1. **Recursive Function**: Define a recursive function `Rob(root)` that returns the maximum amount of money that can be robbed from the subtree rooted at `root`.
2. **Robbery Decision**:
   - If we decide to rob the `root` node, we cannot rob its children but can rob its grandchildren.
   - If we decide not to rob the `root` node, we are free to rob its children.
3. **Calculate** the maximum value from robbing the root vs. not robbing the root.
4. **Base Case**: If the node is `null`, return `0` because there's nothing to rob.

#### Code
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Solution {
    public int Rob(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int robRoot = root.val;
        if (root.left != null) {
            robRoot += Rob(root.left.left) + Rob(root.left.right);
        }
        if (root.right != null) {
            robRoot += Rob(root.right.left) + Rob(root.right.right);
        }

        int notRobRoot = Rob(root.left) + Rob(root.right);

        return Math.Max(robRoot, notRobRoot);
    }
}
```

#### Complexity
- **Time Complexity**: O(2^n), where n is the number of nodes. This is because each node gives two choices: to rob or not.
- **Space Complexity**: O(h), where h is the height of the tree, due to the recursion stack.

### Memoization

#### Intuition
To optimize the recursive approach, we can store the results of subproblems (overlapping subproblems) to avoid redundant calculations. This involves using a cache (dictionary) to store the maximum value robable from each node, which saves computations by retrieving stored results.

#### Approach
1. Use a dictionary to memorize results for each node.
2. Follow the same recursive decision making as above but store and retrieve results before recalculating.

#### Code
```csharp
public class Solution {
    private Dictionary<TreeNode, int> memo = new Dictionary<TreeNode, int>();
    
    public int Rob(TreeNode root) {
        if (root == null) return 0;
        if (memo.ContainsKey(root)) return memo[root];

        int robRoot = root.val;
        if (root.left != null) {
            robRoot += Rob(root.left.left) + Rob(root.left.right);
        }
        if (root.right != null) {
            robRoot += Rob(root.right.left) + Rob(root.right.right);
        }

        int notRobRoot = Rob(root.left) + Rob(root.right);

        int result = Math.Max(robRoot, notRobRoot);
        memo[root] = result;
        return result;
    }
}
```

#### Complexity
- **Time Complexity**: O(n), each node is processed once and results are stored.
- **Space Complexity**: O(n), due to the dictionary storing each node's result.

### Tree DP

#### Intuition
Instead of top-down recursion, adopt a bottom-up approach using dynamic programming directly over the tree. For each node, calculate two possibilities: maximum value when robbing this node (`rob`) and when not robbing this node (`notRob`).

#### Approach
1. For each node, compute and return two values, `rob` and `notRob`, in a helper function.
2. If a node is robbed, then children cannot be robbed.
3. If a node is not robbed, then children might be robbed to maximize value.
4. Aggregating choices allows to capture the core of subproblem dependencies.

#### Code
```csharp
public class Solution {
    public int Rob(TreeNode root) {
        var result = RobHouse(root);
        return Math.Max(result.rob, result.notRob);
    }
    
    private (int rob, int notRob) RobHouse(TreeNode node) {
        if (node == null) return (0, 0);
        
        var left = RobHouse(node.left);
        var right = RobHouse(node.right);
        
        int rob = node.val + left.notRob + right.notRob;
        int notRob = Math.Max(left.rob, left.notRob) + Math.Max(right.rob, right.notRob);
        
        return (rob, notRob);
    }
}
```

#### Complexity
- **Time Complexity**: O(n), each node is processed once.
- **Space Complexity**: O(h), where h is the height of the tree due to recursion stack.

In this solution, we leverage tuples for clear distinction between two cases at each node, facilitating efficient propagation of decisions upwards in the tree.

