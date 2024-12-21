# [Leetcode 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Solutions:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using BFS](#iterative-approach-using-bfs)
3. [Iterative Approach Using DFS with Stack](#iterative-approach-using-dfs-with-stack)

### Recursive Approach

**Intuition**:
The recursive approach is elegant and leverages the natural recursive structure of trees. The idea is to swap the left and right children of a node recursively. For each node, invert the left subtree and the right subtree. This results in a mirrored version of the original tree.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    public TreeNode invertTree(TreeNode root) {
        // Base case: if the tree is empty, return null
        if (root == null) {
            return null;
        }
        
        // Recursively invert the left and right subtrees
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        
        // Swap the left and right children
        root.left = right;
        root.right = left;
        
        // Return the root node which now represents the root of the inverted subtree
        return root;
    }
}
```

**Time Complexity**: O(n)  
We visit each node exactly once.

**Space Complexity**: O(h)  
The recursion stack space is proportional to the height of the tree `h`.

### Iterative Approach Using BFS

**Intuition**:
We can also solve this problem iteratively using a breadth-first search (BFS) approach. The idea is to use a queue to perform a level-order traversal of the tree. At each node, swap the left and right children.

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            
            // Swap the left and right children
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;
            
            // If the left child is not null, add it to the queue for further processing
            if (current.left != null) {
                queue.offer(current.left);
            }
            // If the right child is not null, add it to the queue for further processing
            if (current.right != null) {
                queue.offer(current.right);
            }
        }
        
        return root;
    }
}
```

**Time Complexity**: O(n)  
Each node is visited once.

**Space Complexity**: O(n)  
In the worst case, the queue will hold all the nodes in a level of the tree.

### Iterative Approach Using DFS with Stack

**Intuition**:
Another iterative method is using depth-first search (DFS) with a stack. Similar to BFS, traverse the tree and swap left and right children for each node encountered.

```java
import java.util.Stack;

class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            
            // Swap the left and right children
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            
            // If the left child is not null, add it to the stack for further processing
            if (node.left != null) {
                stack.push(node.left);
            }
            // If the right child is not null, add it to the stack for further processing
            if (node.right != null) {
                stack.push(node.right);
            }
        }
        
        return root;
    }
}
```

**Time Complexity**: O(n)  
Every node is pushed and popped from the stack once.

**Space Complexity**: O(n)  
In the worst case, the stack will hold all nodes in a path from root to a leaf.

