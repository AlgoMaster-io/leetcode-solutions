[LeetCode Problem 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Optimized Recursive Solution with HashMap](#approach-2-optimized-recursive-solution-with-hashmap)

---

### Approach 1: Recursive Solution

**Intuition**

The core idea of this approach is understanding how the binary tree's properties are represented in the preorder and inorder traversals:

- Preorder traversal visits nodes in the order: `Root -> Left subtree -> Right subtree`.
- Inorder traversal visits nodes in the order: `Left subtree -> Root -> Right subtree`.

Using these properties, we can recursively determine the position of the root, and then divide the trees into subtrees and construct them recursively.

**Algorithm**

1. Base Case: If there are no elements to construct the tree, return `null`.
2. The first element of `preorder` is the root.
3. Find the root in `inorder`. This tells us how many nodes are in the left subtree.
4. Recursively construct the left subtree with the corresponding elements from `preorder` and `inorder`.
5. Recursively construct the right subtree with the corresponding elements.
6. Connect the left and right subtrees to the root and return the root.

**Code**

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public TreeNode BuildTree(int[] preorder, int[] inorder) {
    return Build(preorder, 0, preorder.Length - 1, inorder, 0, inorder.Length - 1);
}

private TreeNode Build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
    // Base case: No elements to construct the tree
    if (preStart > preEnd || inStart > inEnd) {
        return null;
    }
    
    // The first element in preorder is the root
    int rootVal = preorder[preStart];
    TreeNode root = new TreeNode(rootVal);
    
    // Find the root in inorder
    int inRootIndex = inStart;
    while (inorder[inRootIndex] != rootVal) {
        inRootIndex++;
    }
    
    // Number of nodes in left subtree
    int leftTreeSize = inRootIndex - inStart;
    
    // Recursively build left and right subtrees
    root.left = Build(preorder, preStart + 1, preStart + leftTreeSize, inorder, inStart, inRootIndex - 1);
    root.right = Build(preorder, preStart + leftTreeSize + 1, preEnd, inorder, inRootIndex + 1, inEnd);
    
    return root;
}
```

**Complexity**
- Time Complexity: O(n^2) because each recursive call requires a linear search through the inorder array.
- Space Complexity: O(n) for the recursion stack, where n is the number of nodes.

---

### Approach 2: Optimized Recursive Solution with HashMap

**Intuition**

We can improve the efficiency of the previous method by using a HashMap to store the index of each element in the inorder traversal. This allows us to find the root index in constant time, thus reducing the time complexity.

**Algorithm**

1. Precompute a HashMap to store the value-index pairs from the inorder traversal.
2. Use this HashMap to quickly find the root's index in the inorder traversal instead of searching linearly.
3. Use the same recursive approach as before to build the tree.

**Code**

```csharp
public TreeNode BuildTreeOptimized(int[] preorder, int[] inorder) {
    Dictionary<int, int> inorderMap = new Dictionary<int, int>();
    for (int i = 0; i < inorder.Length; i++) {
        inorderMap[inorder[i]] = i;
    }
    return BuildOptimized(preorder, 0, preorder.Length - 1, inorder, 0, inorder.Length - 1, inorderMap);
}

private TreeNode BuildOptimized(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd, Dictionary<int, int> inorderMap) {
    if (preStart > preEnd || inStart > inEnd) {
        return null;
    }
    
    int rootVal = preorder[preStart];
    TreeNode root = new TreeNode(rootVal);

    int inRootIndex = inorderMap[rootVal]; // Constant time retrieval from HashMap

    int leftTreeSize = inRootIndex - inStart;

    root.left = BuildOptimized(preorder, preStart + 1, preStart + leftTreeSize, inorder, inStart, inRootIndex - 1, inorderMap);
    root.right = BuildOptimized(preorder, preStart + leftTreeSize + 1, preEnd, inorder, inRootIndex + 1, inEnd, inorderMap);
    
    return root;
}
```

**Complexity**
- Time Complexity: O(n) due to the use of the HashMap which allows constant time lookups for root indices.
- Space Complexity: O(n) for the recursion stack and the HashMap storage.

---

These approaches effectively construct a binary tree using preorder and inorder traversal data, balancing recursive logic with optimized data structures for improved efficiency.

