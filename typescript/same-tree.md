# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Approaches
1. [Recursive Solution](#recursive-solution)
2. [Iterative Solution](#iterative-solution)

## Recursive Solution

### Intuition:
The problem asks us to determine if two binary trees are the same, meaning they have the same structure and node values. A straightforward approach is to use recursion. We compare the root nodes of both trees and recursively compare their left and right subtrees. If at any point, the nodes are different, we return `false`.

### Steps:
1. If both trees are empty (i.e., their nodes are `null`), they are the same; return `true`.
2. If one tree is empty and the other is not, they are not the same; return `false`.
3. If the values of the current nodes are different, return `false`.
4. Recursively check the left and right subtree of both trees.
5. Return `true` only if both subtrees are the same.

### Implementation:

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function isSameTree(p: TreeNode | null, q: TreeNode | null): boolean {
    // Base case: both nodes are null, trees are the same at this point
    if (!p && !q) return true;
    // If only one of the nodes is null, trees are not the same
    if (!p || !q) return false;
    // Both nodes have values, compare values and recursively check left and right subtrees
    return (p.val === q.val) && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

### Time Complexity:
- O(N), where N is the minimum number of nodes in the two trees. We visit each node exactly once until we find a discrepancy or confirm that they are the same.

### Space Complexity:
- O(H), where H is the height of the tree, representing the recursion stack space. For a balanced tree, this would be O(log N), and in the worst case (skewed tree), it would be O(N).

## Iterative Solution

### Intuition:
Similarly to the recursive method, we can use an iterative approach using two stacks. These stacks will help us simulate the recursion by manually traversing both trees. We insert nodes from both trees into the stacks, and at each step, compare the top elements.

### Steps:
1. Use two stacks to store the nodes of the two trees to be compared.
2. Start by pushing the root nodes of both trees to their respective stacks.
3. While stacks are not empty:
   - Pop the top node from each stack.
   - If both nodes are null, continue to the next iteration (this means both subtrees are identical thus far).
   - If only one is null, return `false`.
   - If values are different, return `false`.
   - Push children of both nodes from each tree to their respective stack, ensuring we maintain stack order by pushing the right child first.
4. If the loop completes without returning `false`, the trees are identical; return `true`.

### Implementation:

```typescript
function isSameTreeIterative(p: TreeNode | null, q: TreeNode | null): boolean {
    const stackP: Array<TreeNode | null> = [p];
    const stackQ: Array<TreeNode | null> = [q];

    // Iterate as long as there are elements in stack
    while (stackP.length > 0 && stackQ.length > 0) {
        const nodeP = stackP.pop();
        const nodeQ = stackQ.pop();

        // If both nodes are null, continue to next iteration
        if (!nodeP && !nodeQ) continue;
        // If one is null, trees are not same
        if (!nodeP || !nodeQ) return false;
        // If node values differ
        if (nodeP.val !== nodeQ.val) return false;

        // Push right and left children for both nodes
        stackP.push(nodeP.right);
        stackP.push(nodeP.left);
        stackQ.push(nodeQ.right);
        stackQ.push(nodeQ.left);
    }
    return stackP.length === stackQ.length;
}
```

### Time Complexity:
- O(N), similar reasoning as the recursive solution.

### Space Complexity:
- O(H), where H is the height of the tree, which could be as much as O(N) in the worst case. However, for each node, you only push children onto the stack, unlike recursion where stack frames are created.  

You now have both a recursive and an iterative approach to determine if two binary trees are the same, each with clear trade-offs in terms of space complexity due to recursion and stack usage.

