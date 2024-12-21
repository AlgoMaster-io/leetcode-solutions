# [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

## Table of Contents
- [Approach 1: Level Order Traversal with Indexing](#approach-1-level-order-traversal-with-indexing)
- [Approach 2: Depth-First Search with Indexing](#approach-2-depth-first-search-with-indexing)

## Approach 1: Level Order Traversal with Indexing

### Intuition:
The problem asks us to find the maximum width of a binary tree, which is defined as the maximum number of nodes on any depth level. To achieve this, we can perform a level order traversal (breadth-first traversal) and track the indices of each node in the tree. The difference between the maximum and minimum indices on each level (plus one) will give us the width of the tree at that level. The maximum of these widths across all levels will be our answer.

### Steps:
1. Use a queue to perform a level order traversal. The queue will store pairs of nodes and their respective indices.
2. For each level, calculate the width as `(maxIndex - minIndex + 1)`.
3. Keep track of the maximum width encountered.

### Code:
```csharp
using System;
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public int WidthOfBinaryTree(TreeNode root) {
        if (root == null) return 0;

        int maxWidth = 0;
        Queue<(TreeNode, int)> queue = new Queue<(TreeNode, int)>();
        queue.Enqueue((root, 0));

        while (queue.Count > 0) {
            int levelSize = queue.Count;
            int firstIndex = queue.Peek().Item2;
            int lastIndex = firstIndex;

            for (int i = 0; i < levelSize; i++) {
                var (node, index) = queue.Dequeue();
                lastIndex = index;

                if (node.left != null)
                    queue.Enqueue((node.left, 2 * index));
                if (node.right != null)
                    queue.Enqueue((node.right, 2 * index + 1));
            }

            maxWidth = Math.Max(maxWidth, lastIndex - firstIndex + 1);
        }

        return maxWidth;
    }
}
```
### Complexity:
- **Time complexity**: O(N) where N is the number of nodes in the tree since each node is processed once.
- **Space complexity**: O(N) due to storage in the queue that could store all nodes if the tree is a full binary tree.

## Approach 2: Depth-First Search with Indexing

### Intuition:
A recursive approach can also be used employing a depth-first search (DFS). For each node, calculate its index, and maintain a record of the leftmost node’s index at each level. The max width at each level can be calculated using the index of the current node minus the leftmost node's index at that level plus one.

### Steps:
1. Use a helper recursive function to traverse the tree.
2. For each node, update the leftmost index for that depth if not already set.
3. Calculate the width at each node and update the maximum width.

### Code:
```csharp
using System;
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    private int maxWidth = 0;
    private Dictionary<int, int> leftMostIndex = new Dictionary<int, int>();

    public int WidthOfBinaryTree(TreeNode root) {
        DFS(root, 0, 0);
        return maxWidth;
    }

    private void DFS(TreeNode node, int depth, int index) {
        if (node == null) return;

        if (!leftMostIndex.ContainsKey(depth))
            leftMostIndex[depth] = index;

        maxWidth = Math.Max(maxWidth, index - leftMostIndex[depth] + 1);

        DFS(node.left, depth + 1, 2 * index);
        DFS(node.right, depth + 1, 2 * index + 1);
    }
}
```

### Complexity:
- **Time complexity**: O(N) due to visiting each node exactly once.
- **Space complexity**: O(H) where H is the height of the tree due to the recursion stack. Additionally, O(D) where D is the number of depth levels, if considering dictionary storage.

