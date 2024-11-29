# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if (root != null) {
            dfs(root, "", result);
        }
        return result;
    }

    private void dfs(TreeNode node, String path, List<String> result) {
        // Append the current node's value to the path
        path += node.val;

        // If the current node is a leaf, add the path to the result
        if (node.left == null && node.right == null) {
            result.add(path);
            return;
        }

        // If not a leaf, continue the path with "->" and recurse
        if (node.left != null) {
            dfs(node.left, path + "->", result);
        }
        if (node.right != null) {
            dfs(node.right, path + "->", result);
        }
    }
}
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Stack<TreeNode> stack = new Stack<>();
        Stack<String> paths = new Stack<>();
        stack.push(root);
        paths.push(Integer.toString(root.val));

        while (!stack.isEmpty()) {
            TreeNode currentNode = stack.pop();
            String currentPath = paths.pop();

            // If the current node is a leaf, add the path to the result
            if (currentNode.left == null && currentNode.right == null) {
                result.add(currentPath);
            }

            // If not a leaf, push children to the stack with updated paths
            if (currentNode.right != null) {
                stack.push(currentNode.right);
                paths.push(currentPath + "->" + currentNode.right.val);
            }
            if (currentNode.left != null) {
                stack.push(currentNode.left);
                paths.push(currentPath + "->" + currentNode.left.val);
            }
        }

        return result;
    }
}
```

## Approach 3: Backtracking

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }
        backtrack(root, new StringBuilder(), result);
        return result;
    }

    private void backtrack(TreeNode node, StringBuilder path, List<String> result) {
        int len = path.length();
        path.append(node.val);

        // If the current node is a leaf, add the path to the result
        if (node.left == null && node.right == null) {
            result.add(path.toString());
        } else {
            // If not a leaf, continue the path
            path.append("->");
            if (node.left != null) {
                backtrack(node.left, path, result);
            }
            if (node.right != null) {
                backtrack(node.right, path, result);
            }
        }

        // Backtrack to remove the current node's value and "->"
        path.setLength(len);
    }
}
```