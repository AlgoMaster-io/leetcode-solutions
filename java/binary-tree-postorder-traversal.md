[LeetCode Problem 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Two Stacks](#iterative-approach-using-two-stacks)
3. [Iterative Approach with One Stack](#iterative-approach-with-one-stack)
4. [Morris Traversal (Optimized Approach)](#morris-traversal-optimized-approach)

### Recursive Approach

#### Intuition:
The simplest way to perform a postorder traversal on a binary tree is by using recursion. The nature of postorder traversal is to access children nodes before their parent nodes (Left-Right-Root). Recursion naturally takes care of this backtracking for us.

#### Code:
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorder(root, result);
        return result;
    }
    
    // Helper function to perform postorder traversal
    private void postorder(TreeNode node, List<Integer> result) {
        if (node == null) return; // Base case: If the node is null, return
        postorder(node.left, result);  // Recursively visit left subtree
        postorder(node.right, result); // Recursively visit right subtree
        result.add(node.val);          // Add node's value after children
    }
}
```

#### Time Complexity:
- O(N), where N is the number of nodes in the tree.

#### Space Complexity:
- O(N) due to the recursion call stack.

---

### Iterative Approach using Two Stacks

#### Intuition:
The iterative approach using two stacks mimics the recursive postorder traversal process by using stacks to explore nodes. We can push nodes into the first stack to manage depth, and the second stack to reverse the traversal order temporarily.

#### Code:
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);

        while (!stack1.isEmpty()) {
            TreeNode node = stack1.pop();
            stack2.push(node);
            if (node.left != null) stack1.push(node.left); // Push left child to stack1
            if (node.right != null) stack1.push(node.right); // Push right child to stack1
        }

        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val); // Pop from stack2 to get the postorder traversal
        }

        return result;
    }
}
```

#### Time Complexity:
- O(N), because we process each node twice (pushed into two stacks).

#### Space Complexity:
- O(N) due to the extra space used by the two stacks.

---

### Iterative Approach with One Stack

#### Intuition:
Using one stack to mimic the backtracking operations needed for postorder traversal is more efficient. We need to keep track of previously visited nodes to correctly process the right subtrees after finishing the left subtrees.

#### Code:
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode lastVisited = null; // Keep track of the last visited node
        TreeNode current = root;
        
        while (!stack.isEmpty() || current != null) {
            if (current != null) {
                stack.push(current); // Explore left depth
                current = current.left;
            } else {
                TreeNode peekNode = stack.peek(); // Check the top of the stack
                if (peekNode.right != null && lastVisited != peekNode.right) {
                    current = peekNode.right; // Explore the right subtree
                } else {
                    result.add(peekNode.val); // Add the root node's value
                    lastVisited = stack.pop(); // Mark last visited
                }
            }
        }
        
        return result;
    }
}
```

#### Time Complexity:
- O(N), since each node is visited exactly once.

#### Space Complexity:
- O(N) in the worst case, elements in the stack might be as large as the height of the tree.

---

### Morris Traversal (Optimized Approach)

#### Intuition:
Morris Traversal is a way to traverse the tree while maintaining O(1) space complexity, but it is complex for postorder traversal compared to inorder and preorder. As such, this is not a typical approach used for postorder.

#### Note:
We will not detail the Morris Traversal here for postorder as it is less applicable in practical scenarios compared to the above methods.

---

The solutions above range from straightforward to more optimal in terms of space complexity, providing a comprehensive approach to solving the Binary Tree Postorder Traversal problem.

