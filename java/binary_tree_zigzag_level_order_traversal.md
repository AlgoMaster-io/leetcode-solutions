# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        boolean leftToRight = true;

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            LinkedList<Integer> currentLevel = new LinkedList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll(); // Process the current node

                if (leftToRight) {
                    currentLevel.addLast(currentNode.val); // Add at the end
                } else {
                    currentLevel.addFirst(currentNode.val); // Add at the beginning
                }

                if (currentNode.left != null) {
                    queue.add(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right); // Add right child to the queue
                }
            }

            result.add(currentLevel); // Add the current level to the result
            leftToRight = !leftToRight; // Toggle the direction for the next level
        }

        return result;
    }
}
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, 0, result);
        return result;
    }

    private void dfs(TreeNode node, int level, List<List<Integer>> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (level == result.size()) {
            result.add(new LinkedList<>()); // Create a new level in the result
        }

        if (level % 2 == 0) {
            result.get(level).add(node.val); // Add normally for even levels
        } else {
            result.get(level).add(0, node.val); // Add to the front for odd levels
        }

        dfs(node.left, level + 1, result); // Recurse for the left child
        dfs(node.right, level + 1, result); // Recurse for the right child
    }
}
```