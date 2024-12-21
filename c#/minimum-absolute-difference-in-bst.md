# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Approaches

- [Approach 1: Brute Force O(n^2)](#approach-1-brute-force-on2)
- [Approach 2: Inorder Traversal with List O(n log n)](#approach-2-inorder-traversal-with-list-on-log-n)
- [Approach 3: Optimized Inorder Traversal O(n)](#approach-3-optimized-inorder-traversal-on)

---

## Approach 1: Brute Force O(n^2)

### Intuition
The brute-force approach involves finding the minimum absolute difference by comparing every possible pair of nodes in the binary search tree (BST). This effectively turns into a nested traversal, examining each node's value against every other node's value, leading to a time complexity of O(n^2).

### Code
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public int GetMinimumDifference(TreeNode root) {
        List<int> values = new List<int>();
        TraverseTree(root, values);
        int minDifference = int.MaxValue;

        // Compare each pair of values
        for (int i = 0; i < values.Count; i++) {
            for (int j = i + 1; j < values.Count; j++) {
                int diff = Math.Abs(values[i] - values[j]);
                minDifference = Math.Min(minDifference, diff);
            }
        }
        
        return minDifference;
    }
    
    private void TraverseTree(TreeNode node, List<int> values) {
        if (node == null) return;
        values.Add(node.val);
        TraverseTree(node.left, values);
        TraverseTree(node.right, values);
    }
}
```

### Complexity

- **Time Complexity**: O(n^2) - Due to the nested loops traversing n nodes for each node.
- **Space Complexity**: O(n) - For storing the values of each node in the list.

---

## Approach 2: Inorder Traversal with List O(n log n)

### Intuition
By using an inorder traversal on the BST, we can retrieve node values in a sorted order due to the nature of BST. We can then compute differences between consecutive elements in this sorted list, which reduces the computation needed to find the minimum absolute difference.

### Code
```csharp
public class Solution {
    public int GetMinimumDifference(TreeNode root) {
        List<int> sortedValues = new List<int>();
        InorderTraversal(root, sortedValues);
        
        int minDifference = int.MaxValue;
        // Calculate difference between consecutive sorted elements
        for (int i = 1; i < sortedValues.Count; i++) {
            int diff = sortedValues[i] - sortedValues[i - 1];
            minDifference = Math.Min(minDifference, diff);
        }
        
        return minDifference;
    }
    
    private void InorderTraversal(TreeNode node, List<int> values) {
        if (node == null) return;
        InorderTraversal(node.left, values);
        values.Add(node.val);
        InorderTraversal(node.right, values);
    }
}
```

### Complexity

- **Time Complexity**: O(n log n) - Due to sorting the list, but actually O(n) since the list is sorted by default in inorder traversal.
- **Space Complexity**: O(n) - For storing the traversal list.

---

## Approach 3: Optimized Inorder Traversal O(n)

### Intuition
Leveraging BST properties more directly, we can track the previous node's value during the inorder traversal, calculating the difference between the current node and the previous node online. This avoids additional space for storing tree values.

### Code
```csharp
public class Solution {
    private int minDifference = int.MaxValue;
    private int? prev = null;

    public int GetMinimumDifference(TreeNode root) {
        InorderTraversal(root);
        return minDifference;
    }

    private void InorderTraversal(TreeNode node) {
        if (node == null) return;

        // Traverse the left child
        InorderTraversal(node.left);

        // Process current node
        if (prev.HasValue) {
            int diff = node.val - prev.Value;
            minDifference = Math.Min(minDifference, diff);
        }
        prev = node.val; // update previous node's value

        // Traverse the right child
        InorderTraversal(node.right);
    }
}
```

### Complexity

- **Time Complexity**: O(n) - Each node is visited exactly once.
- **Space Complexity**: O(h) - Best O(log n) in balanced trees, worst O(n) in skewed trees, due to recursion stack.

