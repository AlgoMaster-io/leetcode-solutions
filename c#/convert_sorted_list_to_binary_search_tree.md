# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
csharp
```csharp
// Time Complexity: O(n log n), where n is the length of the list
// Space Complexity: O(log n) (due to recursion stack)
public class Solution {
    public TreeNode SortedListToBST(ListNode head) {
        if (head == null) return null; // Base case: empty list
        if (head.next == null) return new TreeNode(head.val); // Single node

        // Find the middle of the list using slow and fast pointers
        ListNode prev = null, slow = head, fast = head;
        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        // Disconnect the left half of the list
        if (prev != null) prev.next = null;

        // Create the root node
        TreeNode root = new TreeNode(slow.val);

        // Recursively build the left and right subtrees
        root.left = SortedListToBST(head);
        root.right = SortedListToBST(slow.next);

        return root;
    }
}
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public TreeNode SortedListToBST(ListNode head) {
        // Convert the linked list to an array for easier access
        List<int> values = new List<int>();
        while (head != null) {
            values.Add(head.val);
            head = head.next;
        }

        // Recursively construct the BST
        return BuildBST(values, 0, values.Count - 1);
    }

    private TreeNode BuildBST(List<int> values, int left, int right) {
        if (left > right) return null; // Base case

        int mid = left + (right - left) / 2; // Find the middle element
        TreeNode root = new TreeNode(values[mid]); // Create the root node

        // Recursively build left and right subtrees
        root.left = BuildBST(values, left, mid - 1);
        root.right = BuildBST(values, mid + 1, right);

        return root;
    }
}
```

