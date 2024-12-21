## [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach with Stack](#iterative-approach-with-stack)
3. [Morris Traversal](#morris-traversal)

### Recursive Approach

#### Intuition

In an inorder traversal, we visit the left subtree first, then the root, and finally the right subtree. The recursive approach is a direct and simple way to implement this traversal. We can use a recursive function to traverse the left subtree, visit the root node, and then traverse the right subtree.

#### Java Code

```java
import java.util.ArrayList;
import java.util.List;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class BinaryTreeInorderTraversal {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderTraversalHelper(root, result);
        return result;
    }

    private void inorderTraversalHelper(TreeNode node, List<Integer> result) {
        if (node != null) {
            // Traverse the left subtree
            inorderTraversalHelper(node.left, result);
            // Visit the root
            result.add(node.val);
            // Traverse the right subtree
            inorderTraversalHelper(node.right, result);
        }
    }
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity:** O(n) in the worst case due to the system stack when the tree is skewed. In case of a balanced tree, it's O(log n).

### Iterative Approach with Stack

#### Intuition

Instead of using the system's call stack which the recursive approach does implicitly, we can use an explicit stack to simulate the traversal. This approach iteratively manages the nodes to be processed by continuously traversing the left while recording nodes on the stack. When it reaches a node without a left child, it pops from the stack and moves to the right subtree.

#### Java Code

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class BinaryTreeInorderTraversalIterative {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;

        while (!stack.isEmpty() || current != null) {
            // Reach the leftmost node of the current node
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            
            // Current must be null at this point
            current = stack.pop();
            result.add(current.val); // Visit the node
            
            // We have visited the node and its left subtree. Now, visit the right subtree
            current = current.right;
        }

        return result;
    }
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity:** O(n) in the worst case (for a completely skewed tree) and O(log n) for a balanced tree.

### Morris Traversal

#### Intuition

Morris Traversal provides a way to perform the inorder traversal without extra space. The idea is to temporarily make a link from the rightmost node of left subtree to the root node, which allows us to traverse without a stack. After visiting, we have to restore the tree back to its original structure.

#### Java Code

```java
import java.util.ArrayList;
import java.util.List;

public class BinaryTreeInorderTraversalMorris {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        TreeNode current = root;
        while (current != null) {
            if (current.left == null) {
                result.add(current.val); // Visit the node
                current = current.right; // Move to right subtree
            } else {
                // Find the inorder predecessor of current
                TreeNode predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                
                // Make current as the right child of its inorder predecessor
                if (predecessor.right == null) {
                    predecessor.right = current;
                    current = current.left;
                } else {
                    // Revert changes made to the tree
                    predecessor.right = null;
                    result.add(current.val); // Visit the node
                    current = current.right; // Move to right subtree
                }
            }
        }
        return result;
    }
}
```

#### Complexity Analysis

- **Time Complexity:** O(n), where n is the number of nodes in the tree. We visit each node at most twice (once to create a thread and once to remove it).
- **Space Complexity:** O(1), as no extra space is used apart from variables.

