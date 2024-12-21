## [Leetcode Problem 106: Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### Approaches:
1. [Recursive Solution](#recursive-solution)
2. [Optimized Recursive Solution Using HashMap](#optimized-recursive-solution-using-hashmap)

### Recursive Solution

**Intuition:**

In a binary tree, the postorder traversal visits the left subtree, the right subtree, and the node itself. The last element in the postorder array is the root of the tree. For the inorder array, the left subtree is on its left, and the right subtree is on its right. We can recursively build the tree using these properties.

**Explanation:**

1. The last element in the postorder traversal is always the root node.
2. Find this root in the inorder array. The inorder array splits into left and right subtrees based on the root's position.
3. Recursively build the left and right subtrees using the same logic.

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return helper(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
    }
    
    private TreeNode helper(int[] inorder, int[] postorder, int inStart, int inEnd, int postStart, int postEnd) {
        if (inStart > inEnd || postStart > postEnd) {
            return null;
        }
        
        // The last element in postorder is the root node
        int rootValue = postorder[postEnd];
        TreeNode root = new TreeNode(rootValue);
        
        // Find root index in inorder list
        int rootIndex = 0;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == rootValue) {
                rootIndex = i;
                break;
            }
        }
        
        // The number of nodes in the left subtree
        int leftTreeSize = rootIndex - inStart;
        
        // Recursively construct left and right subtrees
        root.left = helper(inorder, postorder, inStart, rootIndex - 1, postStart, postStart + leftTreeSize - 1);
        root.right = helper(inorder, postorder, rootIndex + 1, inEnd, postStart + leftTreeSize, postEnd - 1);
        
        return root;
    }
}
```

**Time Complexity:** O(n^2) – We might end up searching through the inorder list for the root multiple times.
**Space Complexity:** O(n) – for the recursion stack.

### Optimized Recursive Solution Using HashMap

**Intuition:**

To improve the efficiency of finding the root index in the inorder traversal, we can use a HashMap to store the indices of elements in the inorder array. This allows us to find the root index in O(1) time.

**Explanation:**

1. HashMap is used to store the indices of each value in inorder traversal.
2. We find the root element position from the map directly, reducing time complexity.

```java
import java.util.HashMap;

public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // Map will store value as key and index as the value
        HashMap<Integer, Integer> inorderIndexMap = new HashMap<>();
        
        // Fill the map with current index of inorder traversal
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        
        return helper(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1, inorderIndexMap);
    }
    
    private TreeNode helper(int[] inorder, int[] postorder, int inStart, int inEnd, int postStart, int postEnd, Map<Integer, Integer> inorderIndexMap) {
        if (inStart > inEnd || postStart > postEnd) {
            return null;
        }
        
        // The last element in postorder is the root node
        int rootValue = postorder[postEnd];
        TreeNode root = new TreeNode(rootValue);
        
        // Retrieve root index from map directly
        int rootIndex = inorderIndexMap.get(rootValue);
        
        // Calculate the size of left subtree
        int leftTreeSize = rootIndex - inStart;
        
        // Recursively construct left and right subtrees
        root.left = helper(inorder, postorder, inStart, rootIndex - 1, postStart, postStart + leftTreeSize - 1, inorderIndexMap);
        root.right = helper(inorder, postorder, rootIndex + 1, inEnd, postStart + leftTreeSize, postEnd - 1, inorderIndexMap);
        
        return root;
    }
}
```

**Time Complexity:** O(n) – Improved by using a map to get indices in constant time.
**Space Complexity:** O(n) – for the hashmap and recursion stack.

