# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorder(root, result);
        return result;
    }

    private void postorder(TreeNode node, List<Integer> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        postorder(node.left, result); // Recurse for the left subtree
        postorder(node.right, result); // Recurse for the right subtree
        result.add(node.val); // Visit the root
    }
}
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);

        while (!stack1.isEmpty()) {
            TreeNode current = stack1.pop();
            stack2.push(current);

            if (current.left != null) {
                stack1.push(current.left); // Push left child to stack1
            }
            if (current.right != null) {
                stack1.push(current.right); // Push right child to stack1
            }
        }

        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val); // Add nodes in postorder from stack2
        }

        return result;
    }
}
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        TreeNode lastVisited = null;

        while (current != null || !stack.isEmpty()) {
            if (current != null) {
                stack.push(current); // Push nodes to the stack
                current = current.left; // Move to the left subtree
            } else {
                TreeNode peekNode = stack.peek();
                if (peekNode.right != null && lastVisited != peekNode.right) {
                    current = peekNode.right; // Move to the right subtree
                } else {
                    result.add(peekNode.val); // Visit the node
                    lastVisited = stack.pop();
                }
            }
        }

        return result;
    }
}
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        TreeNode dummy = new TreeNode(0);
        dummy.left = root;
        TreeNode current = dummy, predecessor = null;

        while (current != null) {
            if (current.left == null) {
                current = current.right;
            } else {
                predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    predecessor.right = current; // Create a temporary thread
                    current = current.left;
                } else {
                    reverseAddPath(current.left, predecessor, result);
                    predecessor.right = null; // Remove the temporary thread
                    current = current.right;
                }
            }
        }

        return result;
    }

    private void reverseAddPath(TreeNode start, TreeNode end, List<Integer> result) {
        List<Integer> temp = new ArrayList<>();
        while (start != end) {
            temp.add(start.val);
            start = start.right;
        }
        temp.add(end.val);
        for (int i = temp.size() - 1; i >= 0; i--) {
            result.add(temp.get(i)); // Add nodes in reverse order
        }
    }
}
```