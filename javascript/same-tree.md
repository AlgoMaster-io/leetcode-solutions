# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Approaches:
- [Recursive Approach](#recursive-approach)
- [Iterative Approach](#iterative-approach)

### Recursive Approach

#### Intuition:
The problem involves checking if two binary trees are structurally identical and all their nodes have the same value. This can be easily approached using recursion, where for each pair of nodes `(p, q)`, we check:
- Whether both nodes are null.
- Whether one is null and the other is not (trees are not the same).
- Whether both node values are the same and then recursively check their left and right children.

#### Implementation:

```javascript
function isSameTree(p, q) {
    // Helper function that checks individual nodes
    function check(node1, node2) {
        // If both nodes are null, they are considered the same
        if (!node1 && !node2) return true;
        // If one node is null and the other isn't, they are not the same
        if (!node1 || !node2) return false;
        // If both nodes have different values, they are not the same
        return node1.val === node2.val;
    }
    
    // Recursive function to check if trees are the same
    function dfs(n1, n2) {
        // Check the current pair of nodes
        if (!check(n1, n2)) return false;
        // If both nodes are null, we're at the end of the tree and it's the same upto this point
        if (!n1) return true;

        // Recursively check left subtree and right subtree
        return dfs(n1.left, n2.left) && dfs(n1.right, n2.right);
    }

    return dfs(p, q);
}
```

#### Time Complexity:
- **O(N)**, where `N` is the number of nodes in the smaller of the two trees. We visit each node once.

#### Space Complexity:
- **O(H)**, where `H` is the height of the tree, due to the recursion stack in a balanced tree.

### Iterative Approach

#### Intuition:
Instead of using recursion, we can simulate the process using a stack. We iteratively check pairs of nodes starting from the root. This approach is based on a depth-first traversal using a stack to systematically check every node pair for equality in value and structure.

#### Implementation:

```javascript
function isSameTree(p, q) {
    // Stack to simulate recursion
    const stack = [[p, q]];

    while(stack.length > 0) {
        const [node1, node2] = stack.pop();
        
        // If both nodes are null, continue checking other nodes
        if (!node1 && !node2) continue;
        // If only one node is null or values not equal, trees not the same
        if (!node1 || !node2 || node1.val !== node2.val) return false;
        
        // Push right children to stack
        stack.push([node1.right, node2.right]);
        // Push left children to stack
        stack.push([node1.left, node2.left]);
    }
    
    // If all nodes matched, the trees are the same
    return true;
}
```

#### Time Complexity:
- **O(N)**, where `N` is the number of nodes in the smaller of the two trees. Similar to the recursive approach, we visit each node once.

#### Space Complexity:
- **O(H)**, where `H` is the height of the tree, since the maximum number of nodes on the stack will be equal to the height in a balanced tree.

