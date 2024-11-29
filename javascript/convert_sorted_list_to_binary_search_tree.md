# [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approach 1: Recursion with Slow and Fast Pointers

### Solution
```javascript
// Time Complexity: O(n log n), where n is the length of the list
// Space Complexity: O(log n) (due to recursion stack)
class Solution {
    sortedListToBST(head) {
        if (head === null) return null; // Base case: empty list
        if (head.next === null) return new TreeNode(head.val); // Single node

        // Find the middle of the list using slow and fast pointers
        let prev = null, slow = head, fast = head;
        while (fast !== null && fast.next !== null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        // Disconnect the left half of the list
        if (prev !== null) prev.next = null;

        // Create the root node
        const root = new TreeNode(slow.val);

        // Recursively build the left and right subtrees
        root.left = this.sortedListToBST(head);
        root.right = this.sortedListToBST(slow.next);

        return root;
    }
}
```

## Approach 2: In-Order Simulation with List Conversion

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    sortedListToBST(head) {
        // Convert the linked list to an array for easier access
        const values = [];
        while (head !== null) {
            values.push(head.val);
            head = head.next;
        }

        // Recursively construct the BST
        return this.buildBST(values, 0, values.length - 1);
    }

    buildBST(values, left, right) {
        if (left > right) return null; // Base case

        const mid = left + Math.floor((right - left) / 2); // Find the middle element
        const root = new TreeNode(values[mid]); // Create the root node

        // Recursively build left and right subtrees
        root.left = this.buildBST(values, left, mid - 1);
        root.right = this.buildBST(values, mid + 1, right);

        return root;
    }
}
```

