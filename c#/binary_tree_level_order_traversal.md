# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage

using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) {
        IList<IList<int>> result = new List<IList<int>>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while (queue.Count > 0) {
            int levelSize = queue.Count;
            List<int> currentLevel = new List<int>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.Dequeue(); // Process the current node
                currentLevel.Add(currentNode.val);

                if (currentNode.left != null) {
                    queue.Enqueue(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.Enqueue(currentNode.right); // Add right child to the queue
                }
            }

            result.Add(currentLevel); // Add the current level to the result
        }

        return result;
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) {
        IList<IList<int>> result = new List<IList<int>>();
        Dfs(root, 0, result);
        return result;
    }

    private void Dfs(TreeNode node, int level, IList<IList<int>> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (level == result.Count) {
            result.Add(new List<int>()); // Create a new level in the result
        }

        result[level].Add(node.val); // Add the current node's value to the current level

        Dfs(node.left, level + 1, result); // Recurse for the left child
        Dfs(node.right, level + 1, result); // Recurse for the right child
    }
}

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

