[Leetcode Problem 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approaches
1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [Iterative Depth-First Search (DFS) with Stack](#iterative-depth-first-search-dfs-with-stack)

### 1. Recursive Depth-First Search (DFS)

This approach employs a recursive depth-first search to explore each path from the root to the leaf nodes. The basic idea is to keep track of the current path, and when we reach a leaf node, we add this path to our result list.

**Intuition:**

- Start from the root node and traverse down the tree.
- Use a helper function that takes the current node, the ongoing path, and the list of all paths.
- For each node, append the node's value to the current path.
- If a leaf node is reached, add the current path to the results list.
- If it's not a leaf, continue the traversal on both left and right child nodes (if they exist).

**Code:**

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<string> BinaryTreePaths(TreeNode root) {
        var paths = new List<string>();
        if (root != null) {
            DFS(root, "", paths);
        }
        return paths;
    }

    private void DFS(TreeNode node, string path, IList<string> paths) {
        // Append current node value to the path
        path += node.val.ToString();
        
        // Check if the current node is a leaf node
        if (node.left == null && node.right == null) {
            paths.Add(path); // Add complete path to paths list
        } else {
            // If not a leaf, append '->' and continue DFS traversal
            if (node.left != null) {
                DFS(node.left, path + "->", paths);
            }
            if (node.right != null) {
                DFS(node.right, path + "->", paths);
            }
        }
    }
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree. We visit each node once.

**Space Complexity:** O(H), where H is the height of the tree. This is due to the recursion stack that holds up to H function calls. In the worst case, H = N, so the space complexity can be O(N) in an unbalanced tree.

### 2. Iterative Depth-First Search (DFS) with Stack

Instead of relying on recursive calls, we can emulate the recursion using a stack to achieve an iterative solution. This can be preferable in environments that have a recursion depth limit.

**Intuition:**

- Use a stack to simulate the DFS. The stack keeps track of nodes and the paths leading up to them.
- Start with the root node on the stack. While the stack is not empty:
- Pop a node from the stack. If it's a leaf, add the path to the results.
- If it’s not a leaf, push its children onto the stack and extend the current path.

**Code:**

```csharp
public class Solution {
    public IList<string> BinaryTreePaths(TreeNode root) {
        var paths = new List<string>();
        if (root == null) return paths;

        Stack<(TreeNode node, string path)> stack = new Stack<(TreeNode, string)>();
        stack.Push((root, root.val.ToString()));

        while (stack.Count > 0) {
            var (node, path) = stack.Pop();
            
            // Check if current node is a leaf node
            if (node.left == null && node.right == null) {
                paths.Add(path); // Add complete path to paths list
            }
            
            // Push right child if exists
            if (node.right != null) {
                stack.Push((node.right, path + "->" + node.right.val.ToString()));
            }
            
            // Push left child if exists
            if (node.left != null) {
                stack.Push((node.left, path + "->" + node.left.val.ToString()));
            }
        }
        
        return paths;
    }
}
```

**Time Complexity:** O(N), similar to the recursive approach.

**Space Complexity:** O(N), in the worst case for the stack, which must store each node’s path information in the tree.

