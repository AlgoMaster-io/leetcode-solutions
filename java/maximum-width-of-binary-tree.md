# [Leetcode 662: Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

## Approaches:
1. [Breadth-First Search (BFS) with Index Tracking](#approach-1-bfs-with-index-tracking)
2. [Level Order Traversal with Queue](#approach-2-level-order-traversal-with-queue)

## Approach 1: BFS with Index Tracking

### Intuition
To determine the maximum width of the binary tree, one intuitive method is to perform a level-wise traversal while keeping track of the indices of nodes if they were part of a "complete" binary tree. The idea here is that if two nodes are on the same level, the difference between their indices can give us the width at that level. The maximum width found across all levels is the answer.

### Steps:
1. Use a breadth-first search (BFS) approach to visit all nodes level by level, using a queue to store each node along with its index as if it were in a complete binary tree.
2. For each level:
   - Compute the width as the difference between the index of the first and last nodes at that level plus one.
   - Keep track of the maximum width encountered.
3. Continue until all levels have been processed.

### Java Code:
```java
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        if (root == null) return 0;

        int maxWidth = 0;
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair<>(root, 0)); // Start with the root at index 0

        while (!queue.isEmpty()) {
            int size = queue.size();
            int minIndex = queue.peek().getValue(); // Minimum index at the current level
            int firstIndex = 0, lastIndex = 0;

            for (int i = 0; i < size; i++) {
                Pair<TreeNode, Integer> pair = queue.poll();
                TreeNode node = pair.getKey();
                int currentIndex = pair.getValue() - minIndex; // Normalize index to avoid overflow

                // Update the first and last index at this level
                if (i == 0) firstIndex = currentIndex;
                if (i == size - 1) lastIndex = currentIndex;

                if (node.left != null) {
                    queue.offer(new Pair<>(node.left, 2 * currentIndex + 1));
                }
                if (node.right != null) {
                    queue.offer(new Pair<>(node.right, 2 * currentIndex + 2));
                }
            }
            maxWidth = Math.max(maxWidth, lastIndex - firstIndex + 1);
        }

        return maxWidth;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(N), where N is the number of nodes since each node is processed exactly once.
- **Space Complexity**: O(N), due to the queue storing at most the maximum number of nodes in a given level.

## Approach 2: Level Order Traversal with Queue

### Note
This approach is conceptually similar to Approach 1. It makes use of Java's `Queue` to perform level order traversal. The primary difference lies in the explicit enqueuing of indices while handling node nullity and index adjustments specific to the concept of a "complete" binary tree.

### Intuition and Steps are similar, therefore skipping detailed explanation, but it would follow the same pattern as described in Approach 1 using possibly different logic to avoid index normalization, etc. (e.g., adjusting with base indices).

In practical application, the BFS index-tracking method remains very efficient and aligns well with problem constraints, which is why the code is provided for this approach. A deeper dive could squash allowable differences favorably.

Use this base structure to explore alternative tweaks within context, potentially opting for higher-level abstract constructs in JVM environments or tweaking index handling.

