# [Leetcode 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approaches
- [Approach 1: Recursive Construction (Naive)](#approach-1)
- [Approach 2: Optimized Recursive Construction using HashMap](#approach-2)

---

## Approach 1: Recursive Construction (Naive)

**Intuition:**

The main idea is to understand the properties of preorder and inorder traversal:
- In preorder traversal, the first element is always the root of the tree.
- In inorder traversal, the elements left of the root belong to the left subtree and the elements right of the root belong to the right subtree.

We can recursively construct the tree by following these steps:
1. Identify the root from the preorder list.
2. Find the index of this root in the inorder list.
3. Split the inorder list into left and right subtrees.
4. Recursively build the left and right subtrees.

### Java Code
```java
public class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // Call the helper function with initial indices
        return buildTree(preorder, inorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode buildTree(int[] preorder, int[] inorder, 
                               int preStart, int preEnd, int inStart, int inEnd) {
        // Base case: if there are no elements to construct the tree
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }

        // The first element in preorder is the root
        TreeNode root = new TreeNode(preorder[preStart]);

        // Find the root element index in inorder array
        int inRootIndex = 0;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == root.val) {
                inRootIndex = i;
                break;
            }
        }

        // Calculate the size of the left subtree
        int leftTreeSize = inRootIndex - inStart;

        // Recursive construction of left and right subtrees
        root.left = buildTree(preorder, inorder, preStart + 1, preStart + leftTreeSize, inStart, inRootIndex - 1);
        root.right = buildTree(preorder, inorder, preStart + leftTreeSize + 1, preEnd, inRootIndex + 1, inEnd);

        return root;
    }
}
```

**Time Complexity:** O(N^2), where N is the number of nodes. This is because for each node, we potentially scan the entire inorder array.

**Space Complexity:** O(N), for the recursion call stack.

---

## Approach 2: Optimized Recursive Construction using HashMap

**Intuition:**

The inefficiency in the previous approach is due to scanning the inorder array to find the root index. We can use a HashMap to store the index of each value in the inorder array, allowing O(1) lookup for the root index.

### Java Code
```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // Map to store the indices of inorder elements for O(1) access
        Map<Integer, Integer> inorderMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderMap.put(inorder[i], i);
        }
        // Call helper function
        return buildTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1, inorderMap);
    }

    private TreeNode buildTree(int[] preorder, int preStart, int preEnd,
                               int[] inorder, int inStart, int inEnd,
                               Map<Integer, Integer> inorderMap) {
        // Base case: no elements left to construct the tree
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }

        // First element in preorder is the root
        TreeNode root = new TreeNode(preorder[preStart]);

        // Find the index of the root in inorder array using the map
        int inRootIndex = inorderMap.get(root.val);
        int leftTreeSize = inRootIndex - inStart;

        // Recursive construction of left and right subtrees
        root.left = buildTree(preorder, preStart + 1, preStart + leftTreeSize, inorder, inStart, inRootIndex - 1, inorderMap);
        root.right = buildTree(preorder, preStart + leftTreeSize + 1, preEnd, inorder, inRootIndex + 1, inEnd, inorderMap);

        return root;
    }
}
```

**Time Complexity:** O(N), where N is the number of nodes. We avoid repeated scanning of inorder by using HashMap.

**Space Complexity:** O(N), for the recursion call stack and the HashMap. 

Both approaches yield the same tree but use different methods to locate the root index in the inorder traversal, significantly improving efficiency in the second, optimized approach.

