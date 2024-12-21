# [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Table of Contents
- [Approach 1: Recursive Solution Using Postorder's Last Element as Root](#approach-1)
- [Approach 2: Optimized Recursive with Inorder Index Map](#approach-2)

---

## Approach 1: Recursive Solution Using Postorder's Last Element as Root

### Intuition:
The key to solving this problem is understanding the traversal characteristics:
- **Postorder Traversal**: nodes are visited in Left, Right, Root order.
- **Inorder Traversal**: nodes are visited in Left, Root, Right order.

Given these, the postorder's last element is the root of the tree. By locating this root in the inorder array:
- Left subtree elements are at the left of the root in the inorder array.
- Right subtree elements are at the right of the root.

### Steps:
1. Base Case: If `postorder` or `inorder` list is empty, return `null`.
2. The last element of the `postorder` array is the root.
3. Find the index of this root in the `inorder` array.
4. Recursively construct the left subtree with elements left to this root in `inorder` and the appropriate elements in `postorder`.
5. Recursively construct the right subtree with elements right to this root in `inorder` and appropriate `postorder`.

### Time Complexity:
- **O(n^2)**: Each recursive call searches the `inorder` array, leading to worst-case quadratic time when repeated for every node.

### Space Complexity:
- **O(n)**: Stack space is used for the recursion, where `n` is the number of nodes.

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public TreeNode BuildTree(int[] inorder, int[] postorder) {
        if (inorder.Length == 0 || postorder.Length == 0) return null;
        
        // The last element in postorder is the current root
        int rootVal = postorder[postorder.Length - 1];
        TreeNode root = new TreeNode(rootVal);

        // Find the root index in inorder array
        int inorderRootIndex = Array.IndexOf(inorder, rootVal);
        
        // Recursively construct the left and right subtree
        root.left = BuildTree(inorder[..inorderRootIndex], postorder[..inorderRootIndex]);
        root.right = BuildTree(inorder[(inorderRootIndex + 1)..], postorder[inorderRootIndex..^1]);

        return root;
    }
}
```

---

## Approach 2: Optimized Recursive with Inorder Index Map

### Intuition:
To speed up the root index lookup in the inorder array, we can use a hashmap to record the index of each value. This reduces the time complexity of finding the root index from O(n) to O(1).

### Steps:
1. Construct a map to store the index of each node in the inorder traversal.
2. Use a helper function that utilizes the current range in the postorder array and limiting indexes in the inorder array to construct the binary tree.

### Time Complexity:
- **O(n)**: Each node is processed once, and using a hashmap allows constant-time index fetching.

### Space Complexity:
- **O(n)**: For storing the hashmap and recursive call stack.

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    private Dictionary<int, int> inorderIndexMap;

    public TreeNode BuildTree(int[] inorder, int[] postorder) {
        // Build a map to find index of the root in inorder array
        inorderIndexMap = new Dictionary<int, int>();
        for (int i = 0; i < inorder.Length; i++) {
            inorderIndexMap[inorder[i]] = i;
        }

        return BuildTreeRecursive(inorder, postorder, 0, inorder.Length - 1, 0, postorder.Length - 1);
    }

    private TreeNode BuildTreeRecursive(int[] inorder, int[] postorder, int inStart, int inEnd, int postStart, int postEnd) {
        if (inStart > inEnd || postStart > postEnd) return null;

        // The last element in the current postorder segment is the root
        int rootVal = postorder[postEnd];
        TreeNode root = new TreeNode(rootVal);

        // Find the index of this root in the inorder list using the map
        int inorderRootIndex = inorderIndexMap[rootVal];

        // Calculate the length of the left subtree
        int leftTreeSize = inorderRootIndex - inStart;

        // Construct left and right subtrees
        root.left = BuildTreeRecursive(inorder, postorder, inStart, inorderRootIndex - 1, postStart, postStart + leftTreeSize - 1);
        root.right = BuildTreeRecursive(inorder, postorder, inorderRootIndex + 1, inEnd, postStart + leftTreeSize, postEnd - 1);

        return root;
    }
}
```

This approach optimizes the process with reduced time complexity by leveraging constant-time index lookup in the inorder traversal.

