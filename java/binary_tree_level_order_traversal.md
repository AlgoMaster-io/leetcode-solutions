# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll(); // Process the current node
                currentLevel.add(currentNode.val);

                if (currentNode.left != null) {
                    queue.add(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right); // Add right child to the queue
                }
            }

            result.add(currentLevel); // Add the current level to the result
        }

        return result;
    }
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, 0, result);
        return result;
    }

    private void dfs(TreeNode node, int level, List<List<Integer>> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (level == result.size()) {
            result.add(new ArrayList<>()); // Create a new level in the result
        }

        result.get(level).add(node.val); // Add the current node's value to the current level

        dfs(node.left, level + 1, result); // Recurse for the left child
        dfs(node.right, level + 1, result); // Recurse for the right child
    }
}
```