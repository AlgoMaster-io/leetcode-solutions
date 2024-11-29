# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<string> BinaryTreePaths(TreeNode root) {
        List<string> result = new List<string>();
        if (root != null) {
            Dfs(root, "", result);
        }
        return result;
    }

    private void Dfs(TreeNode node, string path, List<string> result) {
        path += node.val;

        if (node.left == null && node.right == null) {
            result.Add(path);
            return;
        }

        if (node.left != null) {
            Dfs(node.left, path + "->", result);
        }
        if (node.right != null) {
            Dfs(node.right, path + "->", result);
        }
    }
}
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<string> BinaryTreePaths(TreeNode root) {
        List<string> result = new List<string>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();
        Stack<string> paths = new Stack<string>();
        stack.Push(root);
        paths.Push(root.val.ToString());

        while (stack.Count > 0) {
            TreeNode currentNode = stack.Pop();
            string currentPath = paths.Pop();

            if (currentNode.left == null && currentNode.right == null) {
                result.Add(currentPath);
            }

            if (currentNode.right != null) {
                stack.Push(currentNode.right);
                paths.Push(currentPath + "->" + currentNode.right.val);
            }
            if (currentNode.left != null) {
                stack.Push(currentNode.left);
                paths.Push(currentPath + "->" + currentNode.left.val);
            }
        }

        return result;
    }
}
```

## Approach 3: Backtracking

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;
using System.Text;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<string> BinaryTreePaths(TreeNode root) {
        List<string> result = new List<string>();
        if (root == null) {
            return result;
        }
        Backtrack(root, new StringBuilder(), result);
        return result;
    }

    private void Backtrack(TreeNode node, StringBuilder path, List<string> result) {
        int len = path.Length;
        path.Append(node.val);

        if (node.left == null && node.right == null) {
            result.Add(path.ToString());
        } else {
            path.Append("->");
            if (node.left != null) {
                Backtrack(node.left, path, result);
            }
            if (node.right != null) {
                Backtrack(node.right, path, result);
            }
        }

        path.Length = len;
    }
}
```

