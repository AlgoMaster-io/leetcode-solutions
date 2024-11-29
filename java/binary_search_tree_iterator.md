# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
```java
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack
import java.util.Stack;

public class BSTIterator {

    private Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        pushLeft(root);
    }

    // Push all left children of the current node to the stack
    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }

    /** @return the next smallest number */
    public int next() {
        TreeNode node = stack.pop(); // Get the topmost node (smallest available)
        if (node.right != null) {
            pushLeft(node.right); // Process the right subtree
        }
        return node.val;
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty(); // Check if the stack is non-empty
    }
}
```

## Approach 2: Precomputed Inorder Traversal

### Solution
```java
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - next(): O(1)
//   - hasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree
import java.util.ArrayList;
import java.util.List;

public class BSTIterator {

    private List<Integer> inorder;
    private int index;

    public BSTIterator(TreeNode root) {
        inorder = new ArrayList<>();
        index = 0;
        inorderTraversal(root);
    }

    // Perform an inorder traversal to store the elements
    private void inorderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }
        inorderTraversal(node.left);
        inorder.add(node.val);
        inorderTraversal(node.right);
    }

    /** @return the next smallest number */
    public int next() {
        return inorder.get(index++);
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return index < inorder.size();
    }
}
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
```java
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(1), no additional storage
public class BSTIterator {

    private TreeNode current;
    private TreeNode predecessor;

    public BSTIterator(TreeNode root) {
        current = root;
    }

    /** @return the next smallest number */
    public int next() {
        int value = -1;

        while (current != null) {
            if (current.left == null) {
                value = current.val;
                current = current.right; // Move to the right subtree
                break;
            } else {
                predecessor = current.left;

                // Find the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    predecessor.right = current; // Create a temporary link
                    current = current.left;
                } else {
                    predecessor.right = null; // Remove the temporary link
                    value = current.val;
                    current = current.right; // Move to the right subtree
                    break;
                }
            }
        }

        return value;
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return current != null;
    }
}
```