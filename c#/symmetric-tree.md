[LeetCode 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

### Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Queue](#iterative-approach-using-queue)

---

## Recursive Approach

The basic idea of checking if a tree is symmetric is essentially checking if two subtrees are mirror images of each other. This can be tackled recursively by comparing left and right children.

### Intuition:

1. A tree is symmetric if the left subtree is a mirror reflection of the right subtree.
2. Recursively check if two trees are mirror:
   - Both trees are empty, they are mirrors: return `true`.
   - Only one of them is empty, they are not mirrors: return `false`.
   - Their root nodes have different values, they are not mirrors: return `false`.
   - Otherwise, check the mirror condition of:
     - Left's left subtree with right's right subtree.
     - Left's right subtree with right's left subtree.

### C# Code:

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public bool IsSymmetric(TreeNode root) {
        if (root == null) return true; // If tree is empty, it is symmetric
        return IsMirror(root.left, root.right);
    }

    private bool IsMirror(TreeNode left, TreeNode right) {
        // Both nodes are null, they are symmetric (leaf nodes)
        if (left == null && right == null) return true;
        // One of the nodes is null, they are not symmetric
        if (left == null || right == null) return false;
        // Values of nodes are different, they are not symmetric
        if (left.val != right.val) return false;

        // Check: left's left and right's right, left's right and right's left
        return IsMirror(left.left, right.right) && IsMirror(left.right, right.left);
    }
}
```

### Time and Space Complexity:

- **Time Complexity**: O(n), since we visit each node once.
- **Space Complexity**: O(n), due to the recursive stack in the worst case (height of the tree).

---

## Iterative Approach using Queue

To avoid recursion, use a queue to iteratively perform a breadth-first traversal.

### Intuition:

1. Use a queue to keep track of nodes to be compared.
2. Initially, put the left and right children of the root into the queue.
3. While the queue is not empty:
   - Extract two nodes at a time and check them for symmetry.
   - If they are symmetric, enqueue their children in appropriate order for further checks.

### C# Code:

```csharp
public class Solution {
    public bool IsSymmetric(TreeNode root) {
        if (root == null) return true; // If tree is empty, it is symmetric

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root.left);
        queue.Enqueue(root.right);

        while (queue.Count > 0) {
            TreeNode left = queue.Dequeue();
            TreeNode right = queue.Dequeue();

            // Both are null, continue with next pair if exists
            if (left == null && right == null) continue;
            // Only one is null, asymmetric
            if (left == null || right == null) return false;
            // Values are not the same, asymmetric
            if (left.val != right.val) return false;

            // Enqueue children in opposite order for symmetry check
            queue.Enqueue(left.left);
            queue.Enqueue(right.right);
            queue.Enqueue(left.right);
            queue.Enqueue(right.left);
        }

        return true;
    }
}
```

### Time and Space Complexity:

- **Time Complexity**: O(n), as we visit each node once.
- **Space Complexity**: O(n), because in the worst case, the queue will hold all nodes at a particular level of tree.


