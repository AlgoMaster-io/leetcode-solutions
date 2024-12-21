[Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Iterative Breadth-First Search (BFS)](#approach-2-iterative-breadth-first-search-bfs)

### Approach 1: Recursive Depth-First Search (DFS)

**Intuition:**

The basic idea is to recursively compare the two trees' nodes. If both trees' nodes have the same value, then we recursively check their left children and their right children. If all corresponding nodes in the two trees are equal, then the trees are the same.

1. Base Cases:
   - If both nodes we're comparing are null, they are identical at this subtree level.
   - If only one of them is null, they're not identical.
   - If both nodes are not null but have different values, they're not identical.

2. Recursive Case:
   - Compare the left subtree of both nodes.
   - Compare the right subtree of both nodes.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // If both nodes are null, the subtrees are identical
        if (p == null && q == null) {
            return true;
        }
        // If one is null and the other is not, the subtrees are not identical
        if (p == null || q == null) {
            return false;
        }
        // If the values differ, the subtrees are not identical
        if (p.val != q.val) {
            return false;
        }
        // Recursively check left and right subtrees
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

**Time Complexity:** O(N), where N is the total number of nodes in both trees. Each node is visited once.

**Space Complexity:** O(H), where H is the height of the tree. This is due to the recursion stack space. In the best case (balanced tree), H = log N, and in the worst case (skewed tree), H = N.

### Approach 2: Iterative Breadth-First Search (BFS)

**Intuition:**

Instead of doing it recursively, we can use an iterative method to traverse both trees level by level using a queue. At each node, we check if both nodes are null or if one is null. If both nodes are non-null, we compare their values and enqueue their children.

1. Use a queue to keep track of nodes from both trees.
2. Start by adding the root nodes of both trees to the queue.
3. While the queue is not empty, process each pair of nodes:
   - If both nodes are null, continue.
   - If one node is null, the trees are not identical.
   - If values of the nodes are different, the trees are not identical.
   - Otherwise, enqueue left and right children of both nodes.
   
```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Use a queue to perform level order traversal
        Queue<TreeNode> queue = new LinkedList<>();
        // Start with the root nodes
        queue.offer(p);
        queue.offer(q);

        while (!queue.isEmpty()) {
            // Get the next pair of nodes to compare
            TreeNode node1 = queue.poll();
            TreeNode node2 = queue.poll();

            // If both nodes are null, continue to the next pair of nodes
            if (node1 == null && node2 == null) {
                continue;
            }
            // If one node is null, the tree structures are different
            if (node1 == null || node2 == null) {
                return false;
            }
            // If values are not the same, the trees are not identical
            if (node1.val != node2.val) {
                return false;
            }

            // Enqueue the children of both nodes
            queue.offer(node1.left);
            queue.offer(node2.left);
            queue.offer(node1.right);
            queue.offer(node2.right);
        }

        // If all nodes have been processed without mismatch
        return true;
    }
}
```

**Time Complexity:** O(N), where N is the total number of nodes in both trees. We traverse each node once.

**Space Complexity:** O(N), where N is the maximum number of nodes at any level in the queue. In the worst case (full binary tree), there can be up to N/2 nodes at the last level.

