## [LeetCode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

### Approaches:
- [Approach 1: In-Order Traversal using Array](#approach-1-in-order-traversal-using-array)
- [Approach 2: In-Order Traversal without extra space](#approach-2-in-order-traversal-without-extra-space)

---

### Approach 1: In-Order Traversal using Array

**Intuition**:
In a Binary Search Tree (BST), the in-order traversal yields nodes in a sorted order. By storing the values of the nodes in an in-order fashion in a list, we can then compute the minimal difference between consecutive elements in this sorted list.

**Steps**:
1. Perform an in-order traversal of the BST and store the node values in a list. This will give you a sorted list of values.
2. Iterate over the list of values to find the minimal absolute difference between consecutive elements.

**Code**:
```csharp
public class Solution {
    public int MinDiffInBST(TreeNode root) {
        List<int> values = new List<int>();
        InOrderTraversal(root, values);

        int minDiff = int.MaxValue;
        for (int i = 1; i < values.Count; i++) {
            int diff = values[i] - values[i - 1];
            if (diff < minDiff) {
                minDiff = diff;
            }
        }
        return minDiff;
    }

    private void InOrderTraversal(TreeNode node, List<int> values) {
        if (node == null) return;
        InOrderTraversal(node.left, values); // Traverse left subtree
        values.Add(node.val); // Visit current node
        InOrderTraversal(node.right, values); // Traverse right subtree
    }
}
```

**Time Complexity**: O(N), where N is the number of nodes in the BST. This is because we visit each node once.
**Space Complexity**: O(N), due to the space needed to store the values of all nodes in the list.

---

### Approach 2: In-Order Traversal without extra space

**Intuition**:
Instead of storing all node values and then finding the minimum difference, we can compute the minimum difference on-the-fly during the in-order traversal. We keep track of the previous value encountered and update the minimum difference accordingly.

**Steps**:
1. Maintain a variable to store the minimum difference and another to track the previous value during the traversal.
2. During the in-order traversal, calculate the difference between the current node's value and the previous node's value and update the minimum difference.

**Code**:
```csharp
public class Solution {
    private int minDiff = int.MaxValue;
    private int? prevVal = null;

    public int MinDiffInBST(TreeNode root) {
        InOrderTraversal(root);
        return minDiff;
    }

    private void InOrderTraversal(TreeNode node) {
        if (node == null) return;

        InOrderTraversal(node.left); // Traverse left subtree

        if (prevVal != null) {
            // Calculate and update minimum difference
            minDiff = Math.Min(minDiff, node.val - prevVal.Value);
        }
        prevVal = node.val; // Update the previous value

        InOrderTraversal(node.right); // Traverse right subtree
    }
}
```

**Time Complexity**: O(N), since we still need to visit each node once.
**Space Complexity**: O(H), where H is the height of the BST. This is the space used by the recursion stack. In the worst case of an unbalanced tree, this can be O(N). However, in a balanced tree, this is O(log N). 

This method is more space-efficient since it does not require storing the entire list of values.

