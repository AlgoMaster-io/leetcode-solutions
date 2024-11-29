# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
c#
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        List<IList<int>> result = new List<IList<int>>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        bool leftToRight = true;

        while (queue.Count > 0) {
            int levelSize = queue.Count;
            LinkedList<int> currentLevel = new LinkedList<int>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.Dequeue(); // Process the current node

                if (leftToRight) {
                    currentLevel.AddLast(currentNode.val); // Add at the end
                } else {
                    currentLevel.AddFirst(currentNode.val); // Add at the beginning
                }

                if (currentNode.left != null) {
                    queue.Enqueue(currentNode.left); // Add left child to the queue
                }
                if (currentNode.right != null) {
                    queue.Enqueue(currentNode.right); // Add right child to the queue
                }
            }

            result.Add(new List<int>(currentLevel)); // Add the current level to the result
            leftToRight = !leftToRight; // Toggle the direction for the next level
        }

        return result;
    }
}
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
c#
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        List<IList<int>> result = new List<IList<int>>();
        Dfs(root, 0, result);
        return result;
    }

    private void Dfs(TreeNode node, int level, List<IList<int>> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        if (level == result.Count) {
            result.Add(new LinkedList<int>()); // Create a new level in the result
        }

        if (level % 2 == 0) {
            result[level].Add(node.val); // Add normally for even levels
        } else {
            ((LinkedList<int>)result[level]).AddFirst(node.val); // Add to the front for odd levels
        }

        Dfs(node.left, level + 1, result); // Recurse for the left child
        Dfs(node.right, level + 1, result); // Recurse for the right child
    }
}
```

Note that the `TreeNode` class is assumed to be predefined in the C# implementations as follows:

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```


