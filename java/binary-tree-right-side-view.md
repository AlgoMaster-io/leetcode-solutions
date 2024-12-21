# [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approaches:
1. [Level Order Traversal](#level-order-traversal)
2. [Depth First Search (DFS) Preorder Traversal](#dfs-preorder-traversal)

### Level Order Traversal

#### Intuition:
The idea is to perform a level order traversal (BFS) of the tree. During the traversal, record the last node you encounter at each level, as that is the node visible from the right side for that level.

#### Solution:
```java
// Definition for a binary tree node.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

import java.util.*;

public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            TreeNode rightMostNode = null;

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                rightMostNode = currentNode; // Keep updating to the last node at current level

                // Enqueue left and right children
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            // Add rightmost node of this level to the result
            if (rightMostNode != null) {
                result.add(rightMostNode.val);
            }
        }
        return result;
    }
}
```

#### Time Complexity:
- O(N), where N is the number of nodes in the tree since each node is visited once.

#### Space Complexity:
- O(D), where D is the diameter of the tree, because the queue will hold at most one level of nodes.

---

### DFS Preorder Traversal

#### Intuition:
We can use a modified DFS where we always attempt to visit the right child before the left child. This way, the first time we visit a new depth level, it's guaranteed to be the rightmost element. Store the value of such a node if the current depth is equal to the size of the result list.

#### Solution:
```java
public class Solution {
    private List<Integer> result = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0);
        return result;
    }

    private void dfs(TreeNode node, int depth) {
        if (node == null) return;

        // Checking if this is the first node of a new depth level
        if (depth == result.size()) {
            result.add(node.val);
        }

        // Prioritize going to the right
        dfs(node.right, depth + 1);
        dfs(node.left, depth + 1);
    }
}
```

#### Time Complexity:
- O(N), where N is the number of nodes because we are potentially visiting all nodes.

#### Space Complexity:
- O(H), where H is the height of the tree due to the recursion stack.

By using the DFS approach, we efficiently capture the rightmost node at each depth level without needing a queue, making it a neat and compact solution.

