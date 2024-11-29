# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
using System.Collections.Generic;

public class Solution {
    public IList<int> RightSideView(TreeNode root) {
        List<int> result = new List<int>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while (queue.Count > 0) {
            int levelSize = queue.Count;
            TreeNode lastNode = null;

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.Dequeue(); // Process the current node
                lastNode = currentNode;

                if (currentNode.left != null) {
                    queue.Enqueue(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.Enqueue(currentNode.right); // Add right child to the queue
                }
            }

            result.Add(lastNode.val); // Add the last node's value from this level
        }

        return result;
    }
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<int> RightSideView(TreeNode root) {
        List<int> result = new List<int>();
        Dfs(root, 0, result);
        return result;
    }

    private void Dfs(TreeNode node, int depth, List<int> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (depth == result.Count) {
            result.Add(node.val); // Add the first node of this depth (rightmost node)
        }

        Dfs(node.right, depth + 1, result); // Visit the right subtree first
        Dfs(node.left, depth + 1, result); // Visit the left subtree next
    }
}
```

Note: In both approaches, `TreeNode` is assumed to be a class with properties `int val`, `TreeNode left`, and `TreeNode right`.

