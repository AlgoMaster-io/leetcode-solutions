# [Leetcode 236: Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approaches
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)
- [Approach 2: Iterative DFS with Parent Pointers](#approach-2-iterative-dfs-with-parent-pointers)

## Approach 1: Recursive DFS

### Intuition
The problem can be solved using a recursive approach that explores each node of the tree to find the nodes `p` and `q`. As we find these nodes, we backtrack. The node returning both `p` and `q` in subtrees will be the LCA. This works because the LCA is the deepest node that has both `p` and `q` as descendants. So if we have found `p` and `q` in different halves of a node's subtrees, this node is the LCA.

### Algorithm
1. Traverse the binary tree using Depth-First Search (DFS) recursion.
2. If you encounter either `p` or `q`, return that node.
3. Check the left subtree and the right subtree for `p` or `q`.
4. If both left and right subtree calls return non-null values, current node is the LCA.
5. If only one of them is non-null, return the non-null node, as it might be the LCA or part of the path to LCA.
6. If neither subtree has `p` or `q`, return null.

### Code
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function lowestCommonAncestor(root: TreeNode | null, p: TreeNode, q: TreeNode): TreeNode | null {
    // Base case
    if (root === null || root === p || root === q) return root;

    // Recursively search left subtree
    let left = lowestCommonAncestor(root.left, p, q);
    // Recursively search right subtree
    let right = lowestCommonAncestor(root.right, p, q);

    // If both left and right are non-null, this node is the LCA
    if (left !== null && right !== null) return root;

    // Otherwise return the non-null child
    return left !== null ? left : right;
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. We need to traverse all nodes.
- **Space Complexity**: O(N), due to the recursion stack.

---

## Approach 2: Iterative DFS with Parent Pointers

### Intuition
This approach is a more iterative method to solve the problem by using a parent pointer and a set. Traverse the tree to establish a parent mapping for each node and then use it to determine the path from `p` and `q` to the root. The first common node in both paths is their LCA.

### Algorithm
1. Use a stack to traverse the tree and a map to keep track of each node's parent.
2. Use a set to store ancestors of `p`.
3. Mark `p`'s ancestors by climbing up to the root.
4. Climb up from `q`, the first ancestor that appears in `p`'s ancestor set is the LCA.

### Code
```typescript
function lowestCommonAncestor(root: TreeNode | null, p: TreeNode, q: TreeNode): TreeNode | null {
    if (root === null) return null;

    // A map to store parent pointers
    const parentMap = new Map<TreeNode, TreeNode | null>();

    // Set to store all ancestors of p
    const ancestors = new Set<TreeNode>();

    // Stack for DFS
    const stack: TreeNode[] = [root];
    parentMap.set(root, null);

    // Traverse the tree until both p and q are found
    while (!parentMap.has(p) || !parentMap.has(q)) {
        const node = stack.pop()!;
        if (node.left) {
            parentMap.set(node.left, node);
            stack.push(node.left);
        }
        if (node.right) {
            parentMap.set(node.right, node);
            stack.push(node.right);
        }
    }

    // Add all of p's ancestors to the set
    while (p) {
        ancestors.add(p);
        p = parentMap.get(p)!;
    }

    // The first ancestor of q which appears in p's ancestor set is the LCA
    while (!ancestors.has(q)) {
        q = parentMap.get(q)!;
    }

    return q;
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree, because each node is processed once.
- **Space Complexity**: O(N), needed for the parent map and the ancestor set.

In conclusion, the recursive method offers a natural and straightforward approach with linear space complexity due to the recursion stack, while the iterative approach with parent pointers provides a more explicit method to find the LCA with additional space for the parent map and ancestor set.

