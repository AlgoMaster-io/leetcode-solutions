# [LeetCode 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approaches
1. [Recursive Depth-First Search (DFS) Approach](#recursive-depth-first-search-dfs-approach)
2. [Iterative Depth-First Search (DFS) Using a Stack](#iterative-depth-first-search-dfs-using-a-stack)
3. [Breadth-First Search (BFS) Using a Queue](#breadth-first-search-bfs-using-a-queue)

---

## Recursive Depth-First Search (DFS) Approach

The recursive DFS approach is straightforward and follows the natural structure of a binary tree. The idea is to start from the root node and explore each path from the root to the leaf nodes. Whenever a leaf node is reached, the path from the root to this leaf node is a completed path and can be stored.

Intuition:
- Traverse the tree recursively. Start from the root and keep track of the path.
- If a leaf node is reached (both left and right children are null), the current path is saved.
- Recursively explore both left and right subtrees while updating the current path until all paths are explored.

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> paths = new ArrayList<>();
        if (root != null) {
            dfs(root, "", paths);
        }
        return paths;
    }
    
    private void dfs(TreeNode node, String path, List<String> paths) {
        // Append current node to the path
        path += node.val;
        // If it's a leaf, append the path to paths list
        if (node.left == null && node.right == null) {
            paths.add(path);
        } else {
            // If not a leaf, continue exploring further with arrows
            if (node.left != null) dfs(node.left, path + "->", paths);
            if (node.right != null) dfs(node.right, path + "->", paths);
        }
    }
}
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree, since we visit each node once.

**Space Complexity**: O(H), where H is the height of the binary tree due to recursive call stack.

---

## Iterative Depth-First Search (DFS) Using a Stack

In this approach, we simulate the behavior of a recursive DFS using an explicit stack. This is useful when recursion depth might be a concern.

Intuition:
- Use a stack to manage nodes to be visited along with the path to each node.
- Similar to the recursive approach, but maintain state explicitly using stack operations.
- When a node is a leaf, record its path.

```java
import java.util.*;
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> paths = new ArrayList<>();
        if (root == null) return paths;

        Stack<Tuple> stack = new Stack<>();
        stack.push(new Tuple(root, String.valueOf(root.val)));

        while (!stack.isEmpty()) {
            Tuple current = stack.pop();
            TreeNode node = current.node;
            String path = current.path;

            // If it's a leaf, append the path to the paths list
            if (node.left == null && node.right == null) {
                paths.add(path);
            } 

            // If not a leaf, add the children to the stack with updated paths
            if (node.right != null) {
                stack.push(new Tuple(node.right, path + "->" + node.right.val));
            }
            if (node.left != null) {
                stack.push(new Tuple(node.left, path + "->" + node.left.val));
            }
        }

        return paths;
    }
    
    private static class Tuple {
        TreeNode node;
        String path;
        Tuple(TreeNode node, String path) {
            this.node = node;
            this.path = path;
        }
    }
}
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree.

**Space Complexity**: O(N), due to the usage of the stack that may store up to N nodes in the worst case.

---

## Breadth-First Search (BFS) Using a Queue

This approach employs breadth-first traversal using a queue to explore the tree level by level. It applies a similar logic as DFS but tracks paths in parallel for nodes on the same level.

Intuition:
- Traverse using a queue instead of a stack, maintaining the node and its path.
- As each node is processed, add its children (with updated paths) to the traversal queue.
- Whenever a leaf node is encountered, cache its path.

```java
import java.util.*;
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> paths = new ArrayList<>();
        if (root == null) return paths;

        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, "" + root.val));

        while (!queue.isEmpty()) {
            Pair current = queue.poll();
            TreeNode node = current.node;
            String path = current.path;

            // If it's a leaf, append the entire path to the paths list
            if (node.left == null && node.right == null) {
                paths.add(path);
            } 

            // Add children with updated paths to the queue
            if (node.left != null) {
                queue.offer(new Pair(node.left, path + "->" + node.left.val));
            }
            if (node.right != null) {
                queue.offer(new Pair(node.right, path + "->" + node.right.val));
            }
        }

        return paths;
    }
    
    private static class Pair {
        TreeNode node;
        String path;
        Pair(TreeNode node, String path) {
            this.node = node;
            this.path = path;
        }
    }
}
```

**Time Complexity**: O(N), where N is the number of nodes in the binary tree.

**Space Complexity**: O(N), due to the queue which needs to store nodes and their paths.

