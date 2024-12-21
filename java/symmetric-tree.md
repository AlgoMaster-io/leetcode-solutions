# [Symmetric Tree - LeetCode](https://leetcode.com/problems/symmetric-tree/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Queue](#iterative-approach-using-queue)

---

### Recursive Approach

The symmetric tree problem can be solved by recursively checking if the left subtree is a mirror of the right subtree. More formally, two trees are a mirror of each other if:
1. Their two roots have the same value.
2. The right subtree of each tree is a mirror of the left subtree of the other tree.

#### Intuition
- A tree is symmetric if the left subtree is a mirror reflection of the right subtree.
- For each node, compare the left and right subtrees for mirror-like properties.
- This can be implemented using a recursive function that checks these properties at every node.

#### Steps:
1. If both nodes are null, they are symmetric.
2. If only one of the nodes is null, they are not symmetric.
3. Check if the current nodes have the same value.
4. Recursively check the left and right children.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // An empty tree is symmetric
        if (root == null) return true;
        // Use a helper function to compare left and right subtree
        return isMirror(root.left, root.right);
    }

    private boolean isMirror(TreeNode t1, TreeNode t2) {
        // If both nodes are null, they are mirrors
        if (t1 == null && t2 == null) return true;
        // If one node is null, not mirrors
        if (t1 == null || t2 == null) return false;
        // Check values and recurse on children, swapping roles
        return (t1.val == t2.val) && 
               isMirror(t1.right, t2.left) &&
               isMirror(t1.left, t2.right);
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes in the tree, as each node is visited once.  
**Space Complexity:** O(h), where h is the height of the tree, due to the recursion stack.

---

### Iterative Approach using Queue

Another way to solve the symmetric tree problem is by using an iterative approach with a queue. The idea is similar to the recursive approach, but we employ a queue to imitate the function call stack.

#### Intuition
- Instead of recursion, maintain a queue to process nodes in layers.
- Enqueue pairs of nodes to be compared.
- If at any point, the nodes are not symmetric, return false.

#### Steps:
1. Use a queue to store nodes for comparison.
2. Initially, enqueue the left and right child of the root.
3. While the queue is not empty, extract a pair of nodes.
4. If both are null, continue the loop.
5. If one is null, return false.
6. If their values differ, return false.
7. Enqueue the children of the nodes in the order to maintain the symmetry.

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        
        Queue<TreeNode> queue = new LinkedList<>();
        // Start with the left and right children of the root
        queue.add(root.left);
        queue.add(root.right);

        while (!queue.isEmpty()) {
            TreeNode t1 = queue.poll();
            TreeNode t2 = queue.poll();

            // If both null, continue to the next pair in queue
            if (t1 == null && t2 == null) continue;
            // If one of them is null or values do not match
            if (t1 == null || t2 == null || t1.val != t2.val) return false;

            // Add the children in a mirrored order
            queue.add(t1.left);
            queue.add(t2.right);
            queue.add(t1.right);
            queue.add(t2.left);
        }
        return true;
    }
}
```

**Time Complexity:** O(n), as every node is processed once.  
**Space Complexity:** O(n), where n is the number of nodes in the queue at any level (widest point of the tree). For a perfectly balanced tree, this would be approximately h/2 nodes, or n/2/3 in the worst case.

