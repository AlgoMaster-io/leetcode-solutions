# [Leetcode 236: Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approaches:

- [Approach 1: Brute Force - Node Paths Comparison](#approach-1-brute-force---node-paths-comparison)
- [Approach 2: Optimized Depth-First Search (DFS) - Recursive](#approach-2-optimized-depth-first-search-dfs---recursive)

## Approach 1: Brute Force - Node Paths Comparison

### Intuition:

In a brute force approach, we can find the paths from the root to the two nodes `p` and `q`. The lowest common ancestor would be the last common node in these two paths.

### Steps:

1. First, traverse the tree to find the path from the root to node `p` and also to find the path from the root to node `q`.
2. Once you have both paths, compare the nodes from the start (root) to the end of both paths.
3. The last common node in both paths is the lowest common ancestor.

### Code:

```javascript
function findPath(root, path, k) {
  if (root === null) return false;

  path.push(root);

  if (root.val === k) return true;

  if ((root.left && findPath(root.left, path, k)) || 
      (root.right && findPath(root.right, path, k)))
      return true;

  path.pop();
  return false;
}

function lowestCommonAncestor(root, p, q) {
  const path1 = [];
  const path2 = [];

  if (!findPath(root, path1, p.val) || !findPath(root, path2, q.val)) return null;

  let i;
  for (i = 0; i < path1.length && i < path2.length; i++) {
    if (path1[i] !== path2[i]) break;
  }

  return path1[i - 1]; // The LCA is the last shared ancestor in the paths
}
```

### Complexity:

- **Time Complexity:** `O(n)`, where `n` is the number of nodes in the tree as both searches for paths may traverse the entire tree.
- **Space Complexity:** `O(n)`, due to the space needed to store the path in the worst case for a skewed tree.

## Approach 2: Optimized Depth-First Search (DFS) - Recursive

### Intuition:

A more efficient way to solve this problem is to use a depth-first search (DFS). While traversing the tree:

1. If the current node is either `p` or `q`, return this node.
2. Recursively call the function on the left and right subtrees.
3. If both left and right subtrees return a non-null value, it means we found both nodes in different branches - so the current node is their lowest common ancestor.
4. If only one subtree returns a non-null, return the non-null node upwards. This means both nodes are located in that subtree.

### Code:

```javascript
function lowestCommonAncestor(root, p, q) {
  if (root === null || root === p || root === q) {
    return root; // Found target node or reached end of path
  }
  
  const left = lowestCommonAncestor(root.left, p, q); // Search left subtree
  const right = lowestCommonAncestor(root.right, p, q); // Search right subtree

  if (left !== null && right !== null) {
    return root; // p found in one subtree and q in another
  }

  return left !== null ? left : right; // Either one of the nodes is found, or none
}
```

### Complexity:

- **Time Complexity:** `O(n)`, as each node is visited at most once.
- **Space Complexity:** `O(h)`, where `h` is the height of the tree, due to the recursion stack.

