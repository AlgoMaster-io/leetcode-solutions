# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Approaches
- [Approach 1: Inorder Traversal with ArrayList](#approach-1-inorder-traversal-with-arraylist)
- [Approach 2: Inorder Traversal with Constant Space](#approach-2-inorder-traversal-with-constant-space)

## Approach 1: Inorder Traversal with ArrayList

**Intuition:**

The Binary Search Tree (BST) property ensures that an inorder traversal of the tree will yield nodes in ascending order. By storing these nodes in a list during the traversal, we can then easily compute the minimum absolute difference between consecutive elements.

**Steps:**

1. Perform an inorder traversal of the BST to get the nodes in sorted order.
2. Store the values of the nodes in an `ArrayList`.
3. Compute the minimum difference between adjacent elements in the list.

```java
class Solution {
    
    public int getMinimumDifference(TreeNode root) {
        // List to store inorder traversal of the tree
        List<Integer> inOrderValues = new ArrayList<>();
        
        // Perform inorder traversal and store values
        inorderTraversal(root, inOrderValues);
        
        // Initialize minimum difference as maximum integer value
        int minDiff = Integer.MAX_VALUE;
        
        // Iterate over the sorted inOrderValues list to find the minimum difference
        for (int i = 1; i < inOrderValues.size(); i++) {
            // Calculate difference between consecutive elements 
            int diff = inOrderValues.get(i) - inOrderValues.get(i - 1);
            // Update minDiff if the current difference is smaller
            minDiff = Math.min(minDiff, diff);
        }
        
        return minDiff;
    }
    
    private void inorderTraversal(TreeNode node, List<Integer> inOrderValues) {
        if (node == null) return;
        
        // Visit left subtree
        inorderTraversal(node.left, inOrderValues);
        
        // Add current node value to list
        inOrderValues.add(node.val);
        
        // Visit right subtree
        inorderTraversal(node.right, inOrderValues);
    }
}
```

**Complexity Analysis:**
- Time Complexity: O(N), where N is the number of nodes in the BST. We visit each node once.
- Space Complexity: O(N), due to the storage of the inorder traversal.

## Approach 2: Inorder Traversal with Constant Space

**Intuition:**

Instead of storing the entire list of values, we can maintain a running track of the previously visited node and compute the minimum difference on-the-fly. This approach utilizes the properties of inorder traversal while reducing space usage.

**Steps:**

1. Use a recursive function for the inorder traversal.
2. Keep track of the previous node's value.
3. Update the minimum difference during traversal without using additional space for storage.

```java
class Solution {
    
    private int minDiff = Integer.MAX_VALUE;
    private Integer prev = null;  // To track the value of the previously visited node
    
    public int getMinimumDifference(TreeNode root) {
        // Start the inorder traversal
        inorderTraversal(root);
        return minDiff;
    }
    
    private void inorderTraversal(TreeNode node) {
        if (node == null) return;
        
        // Visit left subtree
        inorderTraversal(node.left);
        
        // Calculate the difference with previous node
        if (prev != null) {
            int diff = node.val - prev;
            // Update the minimum difference if necessary
            minDiff = Math.min(minDiff, diff);
        }
        
        // Update prev to the current node's value
        prev = node.val;
        
        // Visit right subtree
        inorderTraversal(node.right);
    }
}
```

**Complexity Analysis:**
- Time Complexity: O(N), where N is the number of nodes in the BST. Each node is visited once.
- Space Complexity: O(H), where H is the height of the tree, due to the recursion stack. This space is not used for data storage, but for function calls in the call stack.

