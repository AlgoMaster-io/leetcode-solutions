# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            TreeNode lastNode = null;

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll(); // Process the current node
                lastNode = currentNode;

                if (currentNode.left != null) {
                    queue.add(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right); // Add right child to the queue
                }
            }

            result.add(lastNode.val); // Add the last node's value from this level
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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        dfs(root, 0, result);
        return result;
    }

    private void dfs(TreeNode node, int depth, List<Integer> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (depth == result.size()) {
            result.add(node.val); // Add the first node of this depth (rightmost node)
        }

        dfs(node.right, depth + 1, result); // Visit the right subtree first
        dfs(node.left, depth + 1, result); // Visit the left subtree next
    }
}
```