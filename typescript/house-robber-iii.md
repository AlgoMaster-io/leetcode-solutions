### [Leetcode Problem 337: House Robber III](https://leetcode.com/problems/house-robber-iii/)

---

**Approaches**

1. [Recursive Approach](#1-recursive-approach)
2. [Memoization (Top-Down Dynamic Programming)](#2-memoization-top-down-dynamic-programming)
3. [Optimized DFS with Pair](#3-optimized-dfs-with-pair)

---

### 1. Recursive Approach

#### Intuition:
The problem can be approached by recursively deciding whether to rob the current house or not. If you rob this house, you cannot rob its children. If you do not rob this house, you have the option to rob its children. The goal is to calculate the maximum amount you can rob from this root node and its children recursively.

#### Code:
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val === undefined ? 0 : val;
        this.left = left === undefined ? null : left;
        this.right = right === undefined ? null : right;
    }
}

function rob(root: TreeNode | null): number {
    // Base case: if the current node is null, return 0 as there's nothing to rob
    if (!root) return 0;

    // Include the current node's value and proceed recursively with the grandchildren
    let robCurrent = root.val;
    
    if (root.left) {
        robCurrent += rob(root.left.left) + rob(root.left.right);
    }
    if (root.right) {
        robCurrent += rob(root.right.left) + rob(root.right.right);
    }

    // Exclude the current node and proceed recursively with the children
    const robNext = rob(root.left) + rob(root.right);

    // Return the maximum amount obtained by either robbing the current node or skipping it
    return Math.max(robCurrent, robNext);
}

// Time Complexity: O(2^n) as each node has two choices which are considered independently.
// Space Complexity: O(h) where h is the height of the tree due to recursion stacking.
```

---

### 2. Memoization (Top-Down Dynamic Programming)

#### Intuition:
We can optimize the recursive approach by using a memoization table to store results of subproblems (i.e., results of computations for each node). This avoids recomputation and significantly speeds up the process.

#### Code:
```typescript
function robWithMemo(root: TreeNode | null): number {
    const memo = new Map<TreeNode | null, number>();  // Use a Map to store the result of sub-trees

    function robHelper(node: TreeNode | null): number {
        // Base case: if the current node is null, return 0
        if (!node) return 0;

        // If the result is already computed, return it
        if (memo.has(node)) return memo.get(node)!;

        // Calculate the value when robbing this node
        let robCurrent = node.val;
        if (node.left) {
            robCurrent += robHelper(node.left.left) + robHelper(node.left.right);
        }
        if (node.right) {
            robCurrent += robHelper(node.right.left) + robHelper(node.right.right);
        }

        // Calculate the value when not robbing this node
        let robNext = robHelper(node.left) + robHelper(node.right);

        // Store the result in the memoization table
        const result = Math.max(robCurrent, robNext);
        memo.set(node, result);
        
        // Return the maximum robbed value
        return result;
    }

    return robHelper(root);
}

// Time Complexity: O(n) where n is the number of nodes since each node is processed once.
// Space Complexity: O(n) due to the memoization map and recursion stack.
```

---

### 3. Optimized DFS with Pair

#### Intuition:
Instead of storing a single maximum value for each subtree, we store two values: 
- If we rob this node and hence cannot rob its direct children.
- If we do not rob this node, hence are free to rob its children.

By using this pair approach, we traverse the tree once and make a decision for each node whether to rob it or not based not just on its immediate values but also on potential values of future states. This approach is more efficient than using a map, as it utilizes two potential outcomes per node during traversal.

#### Code:
```typescript
function robOptimized(root: TreeNode | null): number {
    function dfs(node: TreeNode | null): [number, number] {
        // If the node is null, return (0, 0) as its rob and not-rob value
        if (!node) return [0, 0];

        // Traverse left and right subtrees
        const left = dfs(node.left);
        const right = dfs(node.right);

        // If robbing this node, can't rob its children
        const robNode = node.val + left[1] + right[1];

        // If not robbing this node, can rob the children
        const notRobNode = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

        // Return a tuple [robNode, notRobNode]
        return [robNode, notRobNode];
    }

    const result = dfs(root);
    return Math.max(result[0], result[1]);
}

// Time Complexity: O(n) where n is the number of nodes in the tree.
// Space Complexity: O(h) where h is the height of the tree due to the recursion stack.
```

This DFS with Pair solution is the most efficient, leveraging a single traversal and considering two states per node, thus keeping the space and time complexity optimal.

