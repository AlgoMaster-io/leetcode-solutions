# [LeetCode 236: Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Solutions:
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)
- [Approach 2: Iterative with Parent Pointers](#approach-2-iterative-with-parent-pointers)

### Approach 1: Recursive DFS

**Intuition:**
The problem can be solved using a recursive approach. The main idea is to traverse the tree from the root down to the leaves. If current node matches either `p` or `q`, it can be a potential candidate for LCA. The recursion should then attempt to find `p` and `q` in both left and right subtrees. If both `p` and `q` are found in different subtrees of the same node, then this node is the LCA.

**Steps:**
1. If the current node is null or matches `p` or `q`, return the current node.
2. Recursively search for the LCA in the left subtree.
3. Recursively search for the LCA in the right subtree.
4. If both left and right calls return non-null, the current node is the LCA.
5. If only one of them is non-null, return the non-null child (which means both p and q are in one subtree).

```csharp
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case: if the current node is null or matches one of the target nodes (p or q), return it.
        if (root == null || root == p || root == q) {
            return root;
        }

        // Recursively search for LCA in the left subtree.
        TreeNode left = LowestCommonAncestor(root.left, p, q);
        
        // Recursively search for LCA in the right subtree.
        TreeNode right = LowestCommonAncestor(root.right, p, q);
        
        // If both sides return a non-null value, this node is the LCA.
        if (left != null && right != null) {
            return root;
        }
        
        // Return either the left or right non-null child node, if available.
        return left ?? right;
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** O(N), where N is the number of nodes in the binary tree. In the worst case, we might visit all nodes.
- **Space Complexity:** O(N), due to the recursive call stack.

### Approach 2: Iterative with Parent Pointers

**Intuition:**
To solve the problem iteratively, we can make use of a dictionary to track the parent of each node. Then, using the parent pointers, we can find all ancestors of one of the nodes (say `p`), and then find the first common ancestor of the other node `q`.

**Steps:**
1. Traverse the tree and store parent pointers for each node.
2. Use the parent pointers to gather all ancestors of node `p` in a set.
3. Traverse from node `q` upwards towards the root and at each step, check if the current node has already been visited (is in the ancestor set of `p`).

```csharp
public class Solution {
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Dictionary to store parent pointers of each node
        Dictionary<TreeNode, TreeNode> parent = new Dictionary<TreeNode, TreeNode>();
        // Stack for iterating over nodes
        Stack<TreeNode> stack = new Stack<TreeNode>();
        
        parent[root] = null;
        stack.Push(root);
        
        // Traverse the tree and record parent pointers until both p and q are discovered
        while (!parent.ContainsKey(p) || !parent.ContainsKey(q)) {
            TreeNode node = stack.Pop();
            
            // If left child exists, update its parent and add to stack
            if (node.left != null) {
                parent[node.left] = node;
                stack.Push(node.left);
            }
            
            // If right child exists, update its parent and add to stack
            if (node.right != null) {
                parent[node.right] = node;
                stack.Push(node.right);
            }
        }
        
        // Set to record all ancestors of node p
        HashSet<TreeNode> ancestors = new HashSet<TreeNode>();
        
        // Record the path from p to root
        while (p != null) {
            ancestors.Add(p);
            p = parent[p];
        }
        
        // Traverse the path from q to root until we find a common ancestor
        while (!ancestors.Contains(q)) {
            q = parent[q];
        }
        
        return q;
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** O(N), where N is the number of nodes in the binary tree. Each node is visited once to establish parent relationships.
- **Space Complexity:** O(N), due to storing parent pointers and ancestors set.

